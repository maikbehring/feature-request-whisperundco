# Feature Request Tool für mittwald

Eine einfache Webseite zum Erstellen von Feature Requests für den [mittwald Feature Tracker](https://github.com/mittwald/feature-requests) per Voice oder Text. Die Seite nutzt mittwald's Whisper-Modell für Voice-to-Text und Mistral für die automatische Strukturierung der Feature Requests.

## Was macht diese Seite?

Die Seite ermöglicht es, Feature Requests für mittwald zu erstellen, indem du einfach alles in einer Sprachnachricht ansagst. Die mittwald AI analysiert deine Nachricht und erstellt automatisch die passenden Felder:

- **Titel** - Kurze Zusammenfassung deines Feature-Wunsches
- **Problem** - Was möchtest du lösen? Wann tritt es auf?
- **Lösung** - Deine Lösungsideen und Vorschläge
- **Zusätzliche Infos** - Screenshots, Links, technische Details

Die Seite verwendet:
- 🎤 **mittwald Whisper-Large-V3-Turbo** für präzise Sprachtranskription
- 🤖 **mittwald Mistral-Small-3.2-24B-Instruct** für intelligente Strukturierung der Texte
- 📋 Direkte GitHub-Integration zum Erstellen von Issues

## Verwendung

### 1. Voraussetzungen

- Einen **mittwald AI-Hosting API-Key** (erhältlich im [mittwald mStudio](https://developer.mittwald.de/de/docs/v2/platform/aihosting/))
- Einen **GitHub-Account** (erforderlich zum Publizieren im [mittwald Feature Tracker](https://github.com/mittwald/feature-requests))
- Einen modernen Browser mit Mikrofon-Zugriff (Chrome, Firefox, Safari, Edge)
- HTTPS oder localhost (für Mikrofon-Zugriff erforderlich)

### 2. Seite öffnen

**Option A: GitHub Pages (empfohlen)**
- Die Seite ist verfügbar unter: `https://maikbehring.github.io/feature-request-whisperundco/`
- Oder öffne direkt: `https://maikbehring.github.io/feature-request-whisperundco/feature-request.html`
- ✅ Automatisches HTTPS (für Mikrofon-Zugriff erforderlich)
- ✅ Keine Installation nötig

**Option B: Lokal öffnen**

Öffne einfach die `feature-request.html` Datei in deinem Browser:

```bash
# macOS/Linux
open feature-request.html

# Windows
start feature-request.html

# oder einfach per Doppelklick im Datei-Explorer
```

⚠️ **Hinweis:** Für Mikrofon-Zugriff ist HTTPS oder localhost erforderlich. Bei lokaler Nutzung über `file://` funktioniert das Mikrofon nicht.

### 3. API-Key eingeben

1. Gib deinen mittwald AI-Hosting API-Key in das Feld oben ein
2. Der Key wird automatisch lokal in deinem Browser gespeichert
3. Du kannst den Key jederzeit anzeigen/verstecken mit dem 👁️ Button

### 4. Feature Request erstellen

**Option A: Per Voice (empfohlen)**

1. Klicke auf **🎤 Sprachaufnahme**
2. Erlaube den Mikrofon-Zugriff in deinem Browser
3. Sage einfach alles in einer Sprachnachricht:
   - Was möchtest du?
   - Welches Problem soll gelöst werden?
   - Welche Lösungsideen hast du?
   - Zusätzliche Infos (Screenshots, Links, etc.)
4. Klicke auf **⏹️ Aufnahme stoppen**
5. Das Audio wird automatisch transkribiert und strukturiert
6. Die Felder werden automatisch von der AI ausgefüllt

**Option B: Per Text**

1. Tippe deinen Feature Request direkt in das große Textfeld
2. Klicke auf **🤖 Felder mit AI extrahieren**
3. Die AI analysiert deinen Text und füllt die Felder automatisch aus

### 5. Felder bearbeiten (optional)

- Klicke auf **✏️ Felder bearbeiten** um die automatisch erstellten Felder anzupassen
- Klicke auf **← Zurück bearbeiten** um zum Haupttext zurückzukehren

### 6. Feature Request abschicken

**Auf GitHub öffnen:**
- Klicke auf **📝 Auf GitHub öffnen**
- Die Seite öffnet GitHub Issues mit vorausgefüllten Daten
- **Wichtig**: Du musst bei GitHub angemeldet sein, um das Issue zu erstellen
- Überprüfe den Inhalt und klicke auf "Submit new issue"

**In Zwischenablage kopieren:**
- Klicke auf **📋 In Zwischenablage kopieren**
- Öffne [mittwald Feature Requests](https://github.com/mittwald/feature-requests/issues/new) manuell
- Melde dich bei GitHub an (falls noch nicht geschehen)
- Füge den kopierten Text ein und erstelle das Issue

## Technische Details

### Verwendete mittwald AI-Modelle

- **Whisper-Large-V3-Turbo**: Für hochqualitative Sprachtranskription
  - Endpoint: `https://llm.aihosting.mittwald.de/v1/audio/transcriptions`
  - Format: WAV (automatische Konvertierung von WebM)
  - Max. Dateigröße: 25 MB

- **Mistral-Small-3.2-24B-Instruct**: Für intelligente Textstrukturierung
  - Endpoint: `https://llm.aihosting.mittwald.de/v1/chat/completions`
  - Extrahiert automatisch Titel, Problem, Lösung und zusätzliche Infos

### Browser-Kompatibilität

- ✅ Chrome/Edge (vollständig unterstützt)
- ✅ Firefox (vollständig unterstützt)
- ✅ Safari (unterstützt, erfordert HTTPS)
- ✅ Mobile Browser (iOS Safari, Chrome Mobile)

### Lokale Speicherung

- API-Key wird im Browser-LocalStorage gespeichert
- Keine Daten werden an externe Server gesendet (außer an die mittwald API)

### ⚠️ WICHTIGE Sicherheitshinweise

**NIEMALS den API-Key in gemeinsam genutzten Browsern speichern:**
- ❌ Öffentliche Computer (Bibliotheken, Internet-Cafés, etc.)
- ❌ Geteilte Arbeitsplätze
- ❌ Familien-Computer oder gemeinsam genutzte Geräte
- ❌ Jeder Computer, auf den andere Personen Zugriff haben

**Warum?** localStorage ist für alle Benutzer des gleichen Browsers auf dem Gerät zugänglich. Andere Personen könnten deinen API-Key einsehen und missbrauchen.

**Sicherheitsbest Practices:**
- ✅ Nutze diese Seite nur auf deinem privaten, gesicherten Gerät
- ✅ Lösche den API-Key nach jeder Nutzung mit dem "🗑️ Löschen" Button
- ✅ Teile deinen API-Key niemals mit anderen
- ✅ Bei Verlust oder Kompromittierung: Erstelle sofort einen neuen Key im mStudio
- ✅ Überprüfe regelmäßig deine API-Key-Nutzung im mittwald mStudio
- ✅ Nutze die Seite nur auf vertrauenswürdigen Websites (XSS-Schutz)

## Beispiel

**Sprachnachricht:**
> "Ich möchte, dass die Projektsuche auch nach Domains funktioniert. Als Entwickler muss ich immer durch alle Projekte klicken, um eine spezifische Domain zu finden. Die Lösung könnte sein, dass die Projektsuche auch Domain-Suche unterstützt. Dann kann ich direkt nach 'example.com' suchen und finde sofort das richtige Projekt."

**Automatisch erstellt:**
- **Titel**: Projektsuche um Domain-Suche erweitern
- **Problem**: Als Entwickler muss man immer durch alle Projekte klicken, um eine spezifische Domain zu finden
- **Lösung**: Projektsuche sollte auch Domain-Suche unterstützen, sodass man direkt nach 'example.com' suchen kann
- **Zusätzliche Infos**: (leer)

## Bekannte Einschränkungen

- **HTTPS erforderlich**: Für Mikrofon-Zugriff ist HTTPS oder localhost erforderlich
- **API-Key benötigt**: Ein gültiger mittwald AI-Hosting API-Key ist erforderlich
- **GitHub-Account erforderlich**: Zum Publizieren im Feature Tracker benötigst du einen GitHub-Account. Falls du noch keinen hast, kannst du dich kostenlos bei [GitHub registrieren](https://github.com/signup)
- **GitHub-Login**: Du musst dich bei GitHub anmelden, um Issues im [mittwald Feature Tracker](https://github.com/mittwald/feature-requests) zu erstellen
- **Audio-Format**: Das Audio wird automatisch von WebM zu WAV konvertiert

## Entwicklung

Die Seite ist eine reine HTML/CSS/JavaScript-Anwendung ohne externe Dependencies. Einfach öffnen und nutzen!

```bash
# Keine Installation erforderlich
# Einfach die HTML-Datei im Browser öffnen
```

## Links

- 🌐 **Live-Version:** [GitHub Pages](https://maikbehring.github.io/feature-request-whisperundco/)
- 📚 **GitHub Pages Setup:** Siehe [GITHUB_PAGES.md](GITHUB_PAGES.md)
- 🔒 **API-Key Sicherheit:** Siehe [API_KEY_SECURITY.md](API_KEY_SECURITY.md)
- 🔐 **API-Key mit GitHub Pages:** Siehe [GITHUB_PAGES_API_KEY.md](GITHUB_PAGES_API_KEY.md) - Spezielle Anleitung für sichere API-Key-Speicherung auf GitHub Pages
- 🗄️ **API-Key Stores & Services:** Siehe [API_KEY_STORES.md](API_KEY_STORES.md) - Übersicht über verfügbare API-Key-Management-Services
- [mittwald Feature Requests auf GitHub](https://github.com/mittwald/feature-requests)
- [mittwald Roadmap](https://mitt.link/roadmap)
- [mittwald Developer Portal](https://developer.mittwald.de/)
- [mittwald AI-Hosting Dokumentation](https://developer.mittwald.de/de/docs/v2/platform/aihosting/)

## Lizenz

Diese Seite ist ein Tool für die mittwald Community. Das mittwald Feature Request Repository hat seine eigene Lizenz.
