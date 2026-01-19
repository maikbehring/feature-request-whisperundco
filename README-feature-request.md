# mittwald Feature Request Tool

Eine benutzerfreundliche Webseite zum Erstellen von Feature Requests für den mittwald Feature Tracker mit Voice-to-Text und Text-Eingabe.

## Features

- 🎤 **Voice-to-Text Eingabe mit mittwald Whisper**: Nutze das mittwald Whisper-Large-V3-Turbo Modell für präzise Sprachtranskription
- ⌨️ **Text-Eingabe**: Traditionelle Text-Eingabe in alle Felder
- 🔐 **API-Key Verwaltung**: API-Key wird sicher lokal im Browser gespeichert
- 📋 **Kopieren**: Kopiere den formatierten Issue-Text in die Zwischenablage
- 🚀 **Direkt zu GitHub**: Öffne GitHub Issues mit vorausgefüllten Daten
- 📱 **Responsive Design**: Funktioniert auf Desktop und Mobilgeräten
- ✨ **Modernes UI**: Schönes, benutzerfreundliches Design
- 🎯 **Audio-Aufnahme**: MediaRecorder API für hochqualitative Audio-Aufnahme

## Verwendung

1. Öffne `feature-request.html` in einem modernen Webbrowser
2. **API-Key eingeben**: Gib deinen mittwald AI-Hosting API-Key ein (wird automatisch lokal gespeichert)
   - Den API-Key erhältst du im [mittwald mStudio](https://developer.mittwald.de/de/docs/v2/platform/aihosting/)
3. Fülle die Felder aus:
   - **Titel** (erforderlich): Kurze Beschreibung deines Feature-Wunsches
   - **Problem** (erforderlich): Welches Problem möchtest du lösen? Wann tritt es auf?
   - **Lösungsideen** (optional): Welche Lösungsideen hast du?
   - **Zusätzliche Informationen** (optional): Screenshots, Links, etc.

3. **Voice-Eingabe mit mittwald Whisper**:
   - Klicke auf den "🎤 Sprachaufnahme" Button neben einem Feld
   - Erlaube den Mikrofon-Zugriff in deinem Browser
   - Spreche dein Feature-Request klar und deutlich
   - Klicke auf "⏹️ Aufnahme stoppen" um die Aufnahme zu beenden
   - Das Audio wird automatisch an die mittwald Whisper API gesendet und transkribiert
   - Die Transkription erscheint automatisch im Textfeld

4. **Issue erstellen**:
   - **Option 1**: Klicke auf "📝 Auf GitHub öffnen" - öffnet GitHub Issues mit vorausgefüllten Daten
   - **Option 2**: Klicke auf "📋 In Zwischenablage kopieren" - kopiert den formatierten Text zum manuellen Einfügen

## Browser-Kompatibilität

### Voice-to-Text (MediaRecorder API + mittwald Whisper)
- ✅ Chrome/Edge (vollständig unterstützt)
- ✅ Firefox (vollständig unterstützt)
- ✅ Safari (unterstützt, erfordert möglicherweise HTTPS)
- ✅ Mobile Browser (iOS Safari, Chrome Mobile)

**Hinweis**: Die Voice-Funktion nutzt die MediaRecorder API für Audio-Aufnahme und sendet das Audio dann an die mittwald Whisper API. Ein aktiver mittwald AI-Hosting API-Key ist erforderlich.

### Anforderungen
- **HTTPS**: Für Mikrofon-Zugriff ist HTTPS oder localhost erforderlich
- **Mikrofon-Zugriff**: Browser-Berechtigung für Mikrofon erforderlich
- **mittwald API-Key**: Gültiger API-Key für mittwald AI-Hosting erforderlich

## GitHub Integration

Das Tool erstellt eine URL zur GitHub Issue-Seite mit vorausgefüllten Parametern:

```
https://github.com/mittwald/feature-requests/issues/new?title=...&body=...
```

Nach dem Öffnen dieser URL musst du:
1. Dich bei GitHub anmelden (falls nicht bereits angemeldet)
2. Das Issue überprüfen
3. Auf "Submit new issue" klicken

## Template-Struktur

Das Tool folgt dem offiziellen mittwald Feature Request Template:

- **Titel**: Kurze, prägnante Beschreibung
- **Problem**: Was möchtest du lösen? Wann tritt es auf?
- **Lösungsideen**: Welche Lösungsvorschläge hast du?
- **Zusätzliche Informationen**: Screenshots, Links, etc.

## Lokale Entwicklung

Einfach die `feature-request.html` Datei in einem Browser öffnen. Keine zusätzlichen Dependencies oder Build-Schritte erforderlich.

```bash
# Öffne die Datei in deinem Browser
open feature-request.html
# oder
start feature-request.html  # Windows
xdg-open feature-request.html  # Linux
```

## Bekannte Einschränkungen

1. **Browser-Kompatibilität**: Voice-to-Text funktioniert nicht in allen Browsern (siehe oben)
2. **Mikrofon-Berechtigung**: Browser benötigt Mikrofon-Zugriff für Voice-Eingabe
3. **HTTPS**: Web Speech API funktioniert am besten über HTTPS (oder localhost)
4. **GitHub-Login**: Du musst dich bei GitHub anmelden, um Issues zu erstellen

## Technische Details

- **Pure HTML/CSS/JavaScript**: Keine Frameworks erforderlich
- **MediaRecorder API**: Für Audio-Aufnahme im Browser
- **mittwald Whisper API**: Für hochqualitative Sprachtranskription
  - **Model**: Whisper-Large-V3-Turbo
  - **Endpoint**: `https://ai.mittwald.de/v1/audio/transcriptions`
  - **Format**: WAV oder WebM (automatische Konvertierung)
  - **Max. Größe**: 25 MB
- **Local Storage**: API-Key wird lokal im Browser gespeichert
- **Responsive Design**: Mobile-first Ansatz
- **Audio-Konvertierung**: Automatische Konvertierung von WebM zu WAV für optimale API-Kompatibilität

## Links

- [mittwald Feature Requests auf GitHub](https://github.com/mittwald/feature-requests)
- [mittwald Roadmap](https://mitt.link/roadmap)
- [Web Speech API Dokumentation](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)

## Lizenz

Diese Seite ist ein Tool für die mittwald Community. Mittwald's Feature Request Repository hat seine eigene Lizenz.
