# Sichere API-Key-Verwaltung

Diese Dokumentation beschreibt verschiedene Optionen zur sicheren Verwaltung des mittwald AI-Hosting API-Keys für diese Anwendung.

## ⚠️ Wichtiger Hinweis

**Diese Anwendung ist eine reine clientseitige HTML/JavaScript-Anwendung ohne Backend.** Das bedeutet, dass der API-Key immer im Browser sichtbar sein muss, um API-Calls zu machen. Es gibt keine 100% sichere Lösung für clientseitige Anwendungen - nur verschiedene Sicherheitsstufen.

---

## Optionen nach Sicherheitsstufe

### 🔴 NICHT SICHER: API-Key im Code

**Nicht empfohlen für:**
- ❌ Öffentliche Repositories
- ❌ Produktionsumgebungen
- ❌ Geteilte Projekte

**Nur für:**
- ✅ Private Repositories
- ✅ Lokale Entwicklung
- ✅ Einmalige Tests

```javascript
const DEFAULT_API_KEY = 'dein-api-key-hier'; // ⚠️ NICHT für öffentliche Repos!
```

---

### 🟡 AKZEPTABEL: localStorage (aktuell implementiert)

**Vorteile:**
- ✅ Key nicht im Quellcode sichtbar
- ✅ Key wird nicht ins Repository committed
- ✅ Funktioniert ohne Backend
- ✅ Einfach zu verwenden

**Nachteile:**
- ⚠️ Key ist im Browser sichtbar (DevTools)
- ⚠️ Key ist für alle Benutzer des Browsers zugänglich
- ⚠️ XSS-Angriffe könnten den Key stehlen
- ⚠️ Nicht sicher auf gemeinsam genutzten Computern

**Verwendung:**
- Der Key wird automatisch im Browser-LocalStorage gespeichert
- Kann jederzeit mit dem "🗑️ Löschen" Button entfernt werden

**Empfohlen für:**
- ✅ Private, gesicherte Geräte
- ✅ Persönliche Nutzung
- ✅ Entwicklungsumgebungen

---

### 🟢 SICHERER: Backend-Proxy (empfohlen für Produktion)

**Vorteile:**
- ✅ API-Key bleibt komplett server-seitig
- ✅ Key ist nie im Browser sichtbar
- ✅ Rate Limiting möglich
- ✅ Logging und Monitoring möglich
- ✅ CORS-Probleme werden gelöst

**Nachteile:**
- ⚠️ Erfordert Backend-Server
- ⚠️ Zusätzliche Infrastruktur nötig
- ⚠️ Höhere Komplexität

**Implementierung:**

#### Option A: Node.js/Express Proxy

```javascript
// server.js
const express = require('express');
const cors = require('cors');
const fetch = require('node-fetch');
const app = express();

app.use(cors());
app.use(express.json({ limit: '50mb' }));

const API_KEY = process.env.MITTWALD_API_KEY; // Aus Environment Variable

// Proxy für Whisper API
app.post('/api/transcribe', async (req, res) => {
    try {
        const formData = new FormData();
        // ... Audio-Daten verarbeiten ...
        
        const response = await fetch('https://llm.aihosting.mittwald.de/v1/audio/transcriptions', {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${API_KEY}`
            },
            body: formData
        });
        
        const data = await response.json();
        res.json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Proxy für Mistral API
app.post('/api/chat', async (req, res) => {
    try {
        const response = await fetch('https://llm.aihosting.mittwald.de/v1/chat/completions', {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${API_KEY}`,
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(req.body)
        });
        
        const data = await response.json();
        res.json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

app.listen(3000);
```

**Environment Variable setzen:**
```bash
export MITTWALD_API_KEY="dein-api-key-hier"
```

**Frontend anpassen:**
```javascript
const WHISPER_API_URL = '/api/transcribe'; // Relativ zum eigenen Server
const CHAT_API_URL = '/api/chat';
```

#### Option B: Serverless Function (Vercel/Netlify)

**Vercel Serverless Function:**

```javascript
// api/transcribe.js
export default async function handler(req, res) {
    const apiKey = process.env.MITTWALD_API_KEY;
    
    // ... Proxy-Logik ...
}
```

**Netlify Function:**

```javascript
// netlify/functions/transcribe.js
exports.handler = async (event, context) => {
    const apiKey = process.env.MITTWALD_API_KEY;
    
    // ... Proxy-Logik ...
}
```

**Environment Variables in Vercel/Netlify:**
- Vercel: Settings → Environment Variables
- Netlify: Site settings → Environment variables

---

### 🟢 SICHERER: Environment Variables beim Hosting

**Für statische Hosting-Anbieter:**

#### GitHub Pages
- ❌ Unterstützt keine Environment Variables
- → Backend-Proxy erforderlich

#### Netlify
- ✅ Environment Variables unterstützt
- ✅ Build-time Injection möglich
- ⚠️ Aber: Werden trotzdem im Client-Code sichtbar (nicht sicher!)

#### Vercel
- ✅ Environment Variables unterstützt
- ✅ Build-time Injection möglich
- ⚠️ Aber: Werden trotzdem im Client-Code sichtbar (nicht sicher!)

**Wichtig:** Environment Variables werden bei statischen Seiten zur Build-Zeit in den Code eingefügt und sind dann im Browser sichtbar. **Nicht sicher für API-Keys!**

---

### 🟢 SICHERSTER: Backend mit Authentifizierung

**Für Multi-User-Szenarien:**

1. **Backend mit User-Authentifizierung**
   - User meldet sich an
   - Backend speichert API-Keys pro User (verschlüsselt)
   - API-Calls gehen über Backend
   - Key nie im Browser

2. **API-Key-Management-Service**
   - Zentraler Service für API-Keys
   - User können ihre Keys verwalten
   - Backend holt Keys aus Service
   - Höchste Sicherheit

---

## Empfehlungen nach Anwendungsfall

### 🏠 Persönliche Nutzung / Demo
- **Empfohlen:** localStorage (aktuell implementiert)
- **Sicherheitsmaßnahmen:**
  - Nur auf privaten Geräten nutzen
  - Key nach Nutzung löschen
  - Nicht auf gemeinsam genutzten Computern

### 🏢 Team-Intern / Private Nutzung
- **Empfohlen:** Backend-Proxy
- **Vorteile:**
  - Zentraler API-Key-Management
  - Rate Limiting
  - Monitoring möglich

### 🌐 Öffentliche Website
- **Empfohlen:** Backend-Proxy mit Rate Limiting
- **Zusätzlich:**
  - User-Authentifizierung
  - API-Key pro User (optional)
  - Monitoring und Logging

### 🔬 Entwicklung / Testing
- **Empfohlen:** localStorage oder DEFAULT_API_KEY
- **Hinweis:** Key sollte nicht ins Repository committed werden

---

## Best Practices

### ✅ DO's

1. **Für Produktion:**
   - Backend-Proxy verwenden
   - API-Key in Environment Variables speichern
   - Rate Limiting implementieren
   - Logging und Monitoring einrichten

2. **Für Entwicklung:**
   - localStorage verwenden
   - Key nach Nutzung löschen
   - `.gitignore` prüfen (keine Keys committen!)

3. **Allgemein:**
   - API-Keys regelmäßig rotieren
   - Nutzung im mStudio überwachen
   - Bei Kompromittierung: Sofort neuen Key erstellen

### ❌ DON'Ts

1. **NIEMALS:**
   - API-Keys in öffentlichen Repositories committen
   - API-Keys in Screenshots oder Dokumentation zeigen
   - API-Keys per E-Mail oder Chat teilen
   - API-Keys in gemeinsam genutzten Browsern speichern

2. **Vermeiden:**
   - API-Keys in URL-Parametern
   - API-Keys in Browser-History
   - API-Keys in Logs

---

## Migration zu Backend-Proxy

Wenn du von localStorage zu einem Backend-Proxy migrieren möchtest:

1. **Backend erstellen** (siehe Beispiel oben)
2. **API-Key als Environment Variable setzen**
3. **Frontend anpassen:**
   - URLs zu `/api/transcribe` und `/api/chat` ändern
   - API-Key-Input-Feld entfernen (optional)
   - `getApiKey()` Funktion entfernen

---

## Weitere Ressourcen

- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)
- [mittwald AI-Hosting Dokumentation](https://developer.mittwald.de/de/docs/v2/platform/aihosting/)
- [GitHub Security Best Practices](https://docs.github.com/en/code-security)

---

## Fragen?

Bei Fragen zur sicheren API-Key-Verwaltung:
- Öffne ein Issue im Repository
- Kontaktiere das mittwald Support-Team
