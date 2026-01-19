# GitHub Pages Setup für Feature Request Tool

Diese Anleitung erklärt, wie du diese Anwendung mit GitHub Pages hosten kannst.

## Was ist GitHub Pages?

GitHub Pages ist ein kostenloser Hosting-Service von GitHub für statische Websites. Du kannst HTML, CSS und JavaScript direkt aus deinem Repository hosten.

## ✅ Was du mit GitHub Pages machen kannst:

### 1. **Kostenloses Hosting**
- ✅ Komplett kostenlos
- ✅ Unbegrenzte Bandbreite (für öffentliche Repos)
- ✅ Automatisches HTTPS (SSL-Zertifikat)
- ✅ Automatisches Deployment bei jedem Push

### 2. **Für dieses Projekt:**
- ✅ Perfekt geeignet (reine HTML/CSS/JavaScript)
- ✅ HTTPS für Mikrofon-Zugriff (erforderlich!)
- ✅ Keine Installation nötig - einfach öffnen und nutzen
- ✅ localStorage funktioniert (API-Key-Speicherung)

### 3. **URL-Struktur:**
Nach Aktivierung ist deine Seite erreichbar unter:
```
https://maikbehring.github.io/feature-request-whisperundco/
```

**Oder mit eigener Domain:**
```
https://deine-domain.de/
```

## ❌ Was GitHub Pages NICHT kann:

- ❌ **Keine Environment Variables** - API-Keys können nicht server-seitig gespeichert werden
- ❌ **Kein Backend** - Keine Server-seitige Logik möglich
- ❌ **Keine Datenbank** - Nur client-seitige Speicherung (localStorage)
- ❌ **Keine Serverless Functions** - Keine API-Proxies ohne zusätzliche Services

## 🚀 GitHub Pages aktivieren

### Schritt 1: Repository-Einstellungen öffnen

1. Gehe zu deinem Repository: `https://github.com/maikbehring/feature-request-whisperundco`
2. Klicke auf **Settings** (oben rechts)
3. Scrolle zu **Pages** (links in der Sidebar)

### Schritt 2: Source auswählen

1. Unter **Source** wähle:
   - **Branch:** `main` (oder `master`)
   - **Folder:** `/ (root)` (oder `/docs` wenn du die Dateien dort hast)

2. Klicke auf **Save**

### Schritt 3: Warten und testen

- GitHub benötigt 1-2 Minuten zum Deployment
- Du erhältst eine Benachrichtigung, wenn die Seite live ist
- Die URL wird angezeigt: `https://maikbehring.github.io/feature-request-whisperundco/`

### Schritt 4: Index-Datei prüfen

GitHub Pages sucht nach:
- `index.html` (wird automatisch geladen)
- Oder: `feature-request.html` (muss explizit aufgerufen werden)

**Option A: Umbenennen (empfohlen)**
```bash
git mv feature-request.html index.html
git commit -m "Rename to index.html for GitHub Pages"
git push
```

**Option B: Redirect erstellen**
Erstelle eine `index.html` die zu `feature-request.html` weiterleitet:
```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="refresh" content="0; url=feature-request.html">
    <title>Feature Request Tool</title>
</head>
<body>
    <p>Weiterleitung zu <a href="feature-request.html">Feature Request Tool</a></p>
</body>
</html>
```

## 📝 Für dieses Projekt: Empfohlene Struktur

### Option 1: index.html erstellen (einfach)

```bash
# Im Repository-Verzeichnis
cp feature-request.html index.html
git add index.html
git commit -m "Add index.html for GitHub Pages"
git push
```

### Option 2: docs/ Ordner verwenden

```bash
# Erstelle docs Ordner
mkdir docs
cp feature-request.html docs/index.html

# In GitHub Pages Settings:
# Source: Branch: main, Folder: /docs
```

## 🔒 Sicherheit mit GitHub Pages

### ✅ Was funktioniert:

1. **HTTPS automatisch aktiviert**
   - Mikrofon-Zugriff funktioniert
   - Keine zusätzliche Konfiguration nötig

2. **localStorage für API-Keys**
   - Funktioniert wie lokal
   - Key wird im Browser gespeichert
   - ⚠️ Aber: Key ist immer noch client-seitig sichtbar

### ⚠️ Wichtige Hinweise:

1. **API-Key bleibt client-seitig**
   - GitHub Pages kann keine Environment Variables verwenden
   - API-Key muss weiterhin im Browser eingegeben werden
   - localStorage ist die beste Option für GitHub Pages

2. **Öffentliches Repository = öffentliche Seite**
   - Wenn das Repo öffentlich ist, ist die Seite öffentlich
   - Code ist für alle sichtbar
   - ⚠️ Keine API-Keys im Code committen!

3. **Rate Limiting**
   - GitHub Pages selbst hat keine Rate Limits
   - Aber: mittwald API hat Rate Limits
   - Alle Nutzer teilen sich den API-Key (wenn fest hinterlegt)

## 🌐 Custom Domain einrichten (optional)

### Schritt 1: Domain konfigurieren

1. In GitHub Pages Settings:
   - Scrolle zu **Custom domain**
   - Gib deine Domain ein: `deine-domain.de`
   - Aktiviere **Enforce HTTPS**

### Schritt 2: DNS konfigurieren

Bei deinem Domain-Provider:

**Option A: A-Record (empfohlen)**
```
Type: A
Name: @
Value: 185.199.108.153
Value: 185.199.109.153
Value: 185.199.110.153
Value: 185.199.111.153
```

**Option B: CNAME**
```
Type: CNAME
Name: www
Value: maikbehring.github.io
```

### Schritt 3: CNAME-Datei erstellen

GitHub erstellt automatisch eine `CNAME` Datei im Repository.

## 🔄 Automatisches Deployment

GitHub Pages deployed automatisch bei jedem Push:

1. **Push zu main Branch**
   ```bash
   git add .
   git commit -m "Update feature request tool"
   git push
   ```

2. **GitHub Actions**
   - Deployment startet automatisch
   - Dauert 1-2 Minuten
   - Status siehst du unter **Actions** Tab

## 📊 Monitoring

### GitHub Pages Status prüfen:

1. **Repository Settings → Pages**
   - Siehst du Deployment-Status
   - Letzte Deployment-Zeit
   - Custom Domain Status

2. **Actions Tab**
   - Siehst du alle Deployments
   - Logs bei Fehlern

## 🐛 Troubleshooting

### Problem: Seite lädt nicht

**Lösung:**
- Prüfe, ob `index.html` existiert
- Prüfe Branch-Einstellungen (muss `main` oder `master` sein)
- Warte 2-3 Minuten nach dem ersten Setup

### Problem: HTTPS funktioniert nicht

**Lösung:**
- Aktiviere "Enforce HTTPS" in Settings → Pages
- Warte bis SSL-Zertifikat generiert wurde (kann einige Minuten dauern)

### Problem: Mikrofon-Zugriff wird verweigert

**Lösung:**
- Stelle sicher, dass HTTPS aktiviert ist
- Prüfe Browser-Konsole auf Fehler
- Teste in verschiedenen Browsern

### Problem: API-Calls schlagen fehl (CORS)

**Lösung:**
- mittwald API sollte CORS für alle Origins erlauben
- Falls nicht: Backend-Proxy erforderlich (siehe `API_KEY_SECURITY.md`)

## 🎯 Best Practices für GitHub Pages

### ✅ DO's:

1. **index.html verwenden**
   - Erleichtert den Zugriff
   - Keine URL mit Dateinamen nötig

2. **HTTPS aktivieren**
   - Für Mikrofon-Zugriff erforderlich
   - Bessere Sicherheit

3. **README aktualisieren**
   - Link zur GitHub Pages URL hinzufügen
   - Deployment-Status dokumentieren

4. **Branch-Strategie**
   - `main` für Produktion
   - `gh-pages` Branch nur wenn nötig

### ❌ DON'Ts:

1. **Keine API-Keys committen**
   - Auch nicht in `DEFAULT_API_KEY`
   - Für öffentliche Repos

2. **Keine sensiblen Daten**
   - Keine Passwörter
   - Keine Tokens

3. **Keine großen Dateien**
   - GitHub Pages hat Limits
   - Audio-Dateien sollten nicht im Repo sein

## 📚 Weitere Ressourcen

- [GitHub Pages Dokumentation](https://docs.github.com/en/pages)
- [GitHub Pages Getting Started](https://docs.github.com/en/pages/getting-started-with-github-pages)
- [Custom Domain Setup](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)

## 🚀 Quick Start

```bash
# 1. Repository klonen (falls noch nicht geschehen)
git clone https://github.com/maikbehring/feature-request-whisperundco.git
cd feature-request-whisperundco

# 2. index.html erstellen
cp feature-request.html index.html

# 3. Committen und pushen
git add index.html
git commit -m "Add index.html for GitHub Pages"
git push

# 4. GitHub Pages aktivieren:
# - Gehe zu Settings → Pages
# - Wähle Branch: main, Folder: / (root)
# - Warte 1-2 Minuten
# - Öffne: https://maikbehring.github.io/feature-request-whisperundco/
```

## ✨ Vorteile für dieses Projekt

1. **Kostenlos** - Keine Hosting-Kosten
2. **HTTPS automatisch** - Perfekt für Mikrofon-Zugriff
3. **Einfach zu deployen** - Einfach pushen
4. **Schnell** - CDN von GitHub
5. **Zuverlässig** - GitHub Infrastruktur

---

**Fragen?** Öffne ein Issue im Repository!
