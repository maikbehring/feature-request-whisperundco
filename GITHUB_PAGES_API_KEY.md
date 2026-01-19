# Sichere API-Key-Speicherung mit GitHub Pages

Da deine Seite bereits auf GitHub Pages live ist, gibt es spezielle Überlegungen zur API-Key-Speicherung.

## ⚠️ Die Herausforderung

**GitHub Pages kann nur statische Dateien hosten:**
- ❌ Kein Backend möglich
- ❌ Keine Environment Variables
- ❌ Keine Server-seitige Logik
- ⚠️ API-Key muss client-seitig sein

## 🎯 Aktuelle Situation

Deine Seite verwendet aktuell:
- ✅ **localStorage** für API-Key-Speicherung
- ✅ **User gibt Key selbst ein** (nicht im Code)
- ✅ **DEFAULT_API_KEY ist leer** (gut!)

**Das ist die beste Lösung für GitHub Pages ohne zusätzliche Infrastruktur!**

---

## 📊 Optionen für GitHub Pages

### Option 1: localStorage (aktuell) ✅ EMPFOHLEN für GitHub Pages

**Status:** Bereits implementiert

**Vorteile:**
- ✅ Funktioniert sofort
- ✅ Keine zusätzliche Infrastruktur
- ✅ Key nicht im Code sichtbar
- ✅ User hat volle Kontrolle

**Nachteile:**
- ⚠️ Key ist im Browser sichtbar (DevTools)
- ⚠️ Nicht sicher auf gemeinsam genutzten Computern
- ⚠️ XSS-Angriffe könnten Key stehlen

**Sicherheitsmaßnahmen:**
- ✅ User muss Key selbst eingeben
- ✅ Key wird nur lokal gespeichert
- ✅ "🗑️ Löschen" Button vorhanden
- ✅ Warnhinweise in der UI

**Empfehlung:** ✅ **Beibehalten** - Das ist die beste Lösung für GitHub Pages!

---

### Option 2: Backend-Proxy auf separatem Service 🟢 SICHERER

**Für:** Produktionsumgebungen, öffentliche Nutzung

**Konzept:**
- GitHub Pages hostet das Frontend (wie jetzt)
- Separater Service hostet Backend-Proxy
- API-Key bleibt server-seitig

**Vorteile:**
- ✅ API-Key nie im Browser sichtbar
- ✅ Rate Limiting möglich
- ✅ Monitoring und Logging
- ✅ CORS-Probleme gelöst

**Nachteile:**
- ⚠️ Zusätzliche Infrastruktur nötig
- ⚠️ Zusätzliche Kosten (meist kostenlos bei Limits)
- ⚠️ Höhere Komplexität

---

## 🚀 Backend-Proxy Setup (Option 2)

### Option A: Vercel Serverless Functions (kostenlos)

**Schritt 1: Vercel Account erstellen**
- Gehe zu [vercel.com](https://vercel.com)
- Melde dich mit GitHub an

**Schritt 2: Neues Projekt erstellen**
- Klicke auf "New Project"
- Importiere dein GitHub Repository
- Oder erstelle ein separates Repository für das Backend

**Schritt 3: Serverless Functions erstellen**

Erstelle `api/transcribe.js`:
```javascript
export default async function handler(req, res) {
    // CORS Headers
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        return res.status(200).end();
    }

    if (req.method !== 'POST') {
        return res.status(405).json({ error: 'Method not allowed' });
    }

    const apiKey = process.env.MITTWALD_API_KEY;
    if (!apiKey) {
        return res.status(500).json({ error: 'API key not configured' });
    }

    try {
        // Audio-Daten aus Request Body holen
        const formData = new FormData();
        const audioBlob = req.body.audio;
        formData.append('file', audioBlob, 'audio.wav');
        formData.append('model', 'whisper-large-v3-turbo');

        const response = await fetch('https://llm.aihosting.mittwald.de/v1/audio/transcriptions', {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${apiKey}`
            },
            body: formData
        });

        if (!response.ok) {
            const error = await response.text();
            return res.status(response.status).json({ error });
        }

        const data = await response.json();
        return res.status(200).json(data);
    } catch (error) {
        return res.status(500).json({ error: error.message });
    }
}
```

Erstelle `api/chat.js`:
```javascript
export default async function handler(req, res) {
    // CORS Headers
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        return res.status(200).end();
    }

    if (req.method !== 'POST') {
        return res.status(405).json({ error: 'Method not allowed' });
    }

    const apiKey = process.env.MITTWALD_API_KEY;
    if (!apiKey) {
        return res.status(500).json({ error: 'API key not configured' });
    }

    try {
        const response = await fetch('https://llm.aihosting.mittwald.de/v1/chat/completions', {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${apiKey}`,
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(req.body)
        });

        if (!response.ok) {
            const error = await response.text();
            return res.status(response.status).json({ error });
        }

        const data = await response.json();
        return res.status(200).json(data);
    } catch (error) {
        return res.status(500).json({ error: error.message });
    }
}
```

**Schritt 4: Environment Variable setzen**
- In Vercel Dashboard: Settings → Environment Variables
- Name: `MITTWALD_API_KEY`
- Value: Dein API-Key
- Environment: Production, Preview, Development

**Schritt 5: Frontend anpassen**

In `feature-request.html` ändern:
```javascript
// Statt direkter API-Calls:
const WHISPER_API_URL = 'https://dein-proxy.vercel.app/api/transcribe';
const CHAT_API_URL = 'https://dein-proxy.vercel.app/api/chat';

// getApiKey() Funktion entfernen oder vereinfachen:
function getApiKey() {
    return ''; // Kein Key mehr nötig!
}
```

**Schritt 6: Deploy**
```bash
git add api/
git commit -m "Add Vercel serverless functions"
git push
```

Vercel deployed automatisch!

---

### Option B: Netlify Functions (kostenlos)

**Ähnlich wie Vercel:**

1. Erstelle `netlify/functions/transcribe.js`
2. Erstelle `netlify/functions/chat.js`
3. Setze Environment Variable in Netlify Dashboard
4. Deploy

**Beispiel:**
```javascript
// netlify/functions/transcribe.js
exports.handler = async (event, context) => {
    const apiKey = process.env.MITTWALD_API_KEY;
    
    // ... Proxy-Logik ...
    
    return {
        statusCode: 200,
        headers: {
            'Access-Control-Allow-Origin': '*',
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    };
};
```

---

### Option C: Cloudflare Workers (kostenlos)

**Für:** Sehr schnelle, globale Verteilung

```javascript
// worker.js
export default {
    async fetch(request) {
        const apiKey = env.MITTWALD_API_KEY;
        
        // ... Proxy-Logik ...
        
        return new Response(JSON.stringify(data), {
            headers: {
                'Access-Control-Allow-Origin': '*',
                'Content-Type': 'application/json'
            }
        });
    }
};
```

---

## 🔄 Hybrid-Ansatz (empfohlen)

**Konzept:**
- GitHub Pages für Frontend (wie jetzt)
- Vercel/Netlify für Backend-Proxy
- Frontend ruft Proxy auf
- API-Key bleibt server-seitig

**Vorteile:**
- ✅ Beste Performance (CDN für Frontend)
- ✅ Sicherer API-Key (server-seitig)
- ✅ Beide Services kostenlos
- ✅ Einfaches Deployment

---

## 📋 Vergleich der Optionen

| Option | Sicherheit | Komplexität | Kosten | Empfehlung |
|--------|-----------|-------------|--------|------------|
| **localStorage** | 🟡 Mittel | ✅ Niedrig | ✅ Kostenlos | ✅ Für persönliche Nutzung |
| **Vercel Proxy** | 🟢 Hoch | 🟡 Mittel | ✅ Kostenlos* | ✅ Für Produktion |
| **Netlify Proxy** | 🟢 Hoch | 🟡 Mittel | ✅ Kostenlos* | ✅ Für Produktion |
| **Cloudflare Workers** | 🟢 Hoch | 🟡 Mittel | ✅ Kostenlos* | ✅ Für hohe Performance |

*Kostenlos innerhalb der Limits (meist ausreichend für normale Nutzung)

---

## 🎯 Empfehlung für deine Situation

### Aktuell (localStorage): ✅ BEIBEHALTEN

**Warum:**
- ✅ Funktioniert perfekt für GitHub Pages
- ✅ Keine zusätzliche Infrastruktur nötig
- ✅ User hat volle Kontrolle über seinen Key
- ✅ Einfach zu verwenden

**Für:**
- Persönliche Nutzung
- Demo/Testing
- Kleine Projekte

### Für Produktion: Backend-Proxy hinzufügen

**Wenn:**
- Viele Nutzer erwartet werden
- Öffentliche Nutzung geplant ist
- Rate Limiting wichtig ist
- Monitoring gewünscht ist

**Dann:**
- Vercel oder Netlify Functions hinzufügen
- Frontend auf Proxy umstellen
- API-Key als Environment Variable setzen

---

## 🛠️ Migration zu Backend-Proxy

Wenn du zu einem Backend-Proxy migrieren möchtest:

1. **Backend-Service erstellen** (Vercel/Netlify)
2. **Environment Variable setzen** (API-Key)
3. **Frontend anpassen:**
   - URLs zu Proxy ändern
   - API-Key-Input entfernen (optional)
   - `getApiKey()` Funktion entfernen
4. **Testen** auf GitHub Pages
5. **Deploy** beide Services

**Beispiel Frontend-Änderung:**
```javascript
// Vorher:
const WHISPER_API_URL = 'https://llm.aihosting.mittwald.de/v1/audio/transcriptions';
const apiKey = getApiKey();

// Nachher:
const WHISPER_API_URL = 'https://dein-proxy.vercel.app/api/transcribe';
// Kein API-Key mehr nötig!
```

---

## 🔒 Sicherheits-Checkliste

### ✅ Aktuell (localStorage):
- [x] Key nicht im Code
- [x] User gibt Key selbst ein
- [x] Warnhinweise vorhanden
- [x] Löschen-Button vorhanden
- [x] HTTPS aktiviert (GitHub Pages)

### ✅ Mit Backend-Proxy:
- [ ] API-Key in Environment Variable
- [ ] CORS korrekt konfiguriert
- [ ] Rate Limiting implementiert
- [ ] Error Handling vorhanden
- [ ] Monitoring eingerichtet

---

## 📚 Weitere Ressourcen

- [Vercel Serverless Functions](https://vercel.com/docs/functions)
- [Netlify Functions](https://docs.netlify.com/functions/overview/)
- [Cloudflare Workers](https://workers.cloudflare.com/)
- [API_KEY_SECURITY.md](API_KEY_SECURITY.md) - Allgemeine Sicherheitsrichtlinien

---

## ❓ FAQ

**Q: Muss ich einen Backend-Proxy verwenden?**
A: Nein! localStorage ist für GitHub Pages völlig ausreichend, wenn jeder User seinen eigenen Key verwendet.

**Q: Kann ich den API-Key sicher in GitHub Pages speichern?**
A: Nein, GitHub Pages unterstützt keine Environment Variables. Backend-Proxy ist die einzige sichere Option.

**Q: Was kostet ein Backend-Proxy?**
A: Vercel, Netlify und Cloudflare Workers sind kostenlos innerhalb der Limits (meist ausreichend).

**Q: Kann ich beide Optionen parallel nutzen?**
A: Ja! Du kannst localStorage als Fallback behalten und Backend-Proxy als primäre Option nutzen.

---

**Fragen?** Öffne ein Issue im Repository!
