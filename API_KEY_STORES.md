# API-Key Management Services & Stores

Übersicht über verfügbare Services zur sicheren Speicherung und Verwaltung von API-Keys, speziell für GitHub Pages und client-seitige Anwendungen.

## 🎯 Kategorien von API-Key Stores

### 1. **Cloud Secrets Management** (Enterprise)
Für große Organisationen mit komplexen Anforderungen.

### 2. **Developer-Focused Services** (Empfohlen)
Einfach zu nutzen, gute Developer Experience.

### 3. **CI/CD & Deployment Services** (Für Backend-Proxies)
Integriert in Deployment-Pipelines.

### 4. **Browser-basierte Lösungen** (Für Client-Side)
Speziell für client-seitige Anwendungen.

---

## 🔐 Cloud Secrets Management Services

### AWS Secrets Manager
**URL:** [aws.amazon.com/secrets-manager](https://aws.amazon.com/secrets-manager/)

**Features:**
- ✅ Automatische Key-Rotation
- ✅ Verschlüsselung mit AWS KMS
- ✅ IAM-basierte Zugriffskontrolle
- ✅ Audit-Logging
- ✅ Multi-Region Replikation

**Kosten:** ~$0.40 pro Secret/Monat + $0.05 pro 10.000 API-Calls

**Für dich:** ❌ Overkill für GitHub Pages, benötigt AWS-Infrastruktur

---

### Azure Key Vault
**URL:** [azure.microsoft.com/products/key-vault](https://azure.microsoft.com/products/key-vault/)

**Features:**
- ✅ Hardware Security Modules (HSM) Support
- ✅ Automatische Rotation
- ✅ Integration mit Azure Services
- ✅ Role-Based Access Control

**Kosten:** ~$0.03 pro 10.000 Transaktionen

**Für dich:** ❌ Overkill, benötigt Azure-Infrastruktur

---

### Google Cloud Secret Manager
**URL:** [cloud.google.com/secret-manager](https://cloud.google.com/secret-manager/)

**Features:**
- ✅ Versionierung von Secrets
- ✅ IAM-Integration
- ✅ Audit-Logging
- ✅ Automatische Rotation

**Kosten:** ~$0.06 pro Secret/Monat + $0.03 pro 10.000 Zugriffe

**Für dich:** ❌ Overkill, benötigt GCP-Infrastruktur

---

### HashiCorp Vault
**URL:** [vaultproject.io](https://www.vaultproject.io/)

**Features:**
- ✅ Open Source (kostenlos selbst hosten)
- ✅ SaaS-Version verfügbar
- ✅ Dynamische Secrets
- ✅ Viele Auth-Methoden

**Kosten:** Open Source (selbst hosten) oder SaaS ab ~$5/Monat

**Für dich:** ⚠️ Komplex, benötigt eigene Infrastruktur oder SaaS-Account

---

## 🚀 Developer-Focused Services (Empfohlen)

### 1Password Secrets Automation
**URL:** [1password.com/features/secrets-management](https://1password.com/features/secrets-management/)

**Features:**
- ✅ Einfache Bedienung
- ✅ End-to-End Verschlüsselung
- ✅ CLI & API verfügbar
- ✅ Integration mit CI/CD
- ✅ Team-Sharing

**Kosten:** Ab $7.99/Monat (Team)

**Für dich:** ✅ Gut für persönliche/Team-Nutzung, einfach zu verwenden

**Integration:**
```bash
# CLI Beispiel
op inject -i config.template -o config.json
```

---

### KeyStash
**URL:** [keystash.dev](https://keystash.dev/)

**Features:**
- ✅ Speziell für API-Keys
- ✅ Einfaches Setup
- ✅ Custom Permissions
- ✅ Audit Logs
- ✅ API & SDKs

**Kosten:** Freemium verfügbar

**Für dich:** ✅ Gut für API-Key-Management, einfach zu nutzen

---

### Keymize
**URL:** [keymize.com](https://keymize.com/)

**Features:**
- ✅ Speziell für AI/ML APIs
- ✅ Load Balancing
- ✅ Caching
- ✅ Kostenoptimierung
- ✅ Echtzeit-Statistiken

**Kosten:** Freemium verfügbar

**Für dich:** ✅ Perfekt für mittwald AI-Hosting APIs!

**Besonderheit:** Unified Endpoint - du kannst mehrere API-Keys hinterlegen und Keymize wählt automatisch den besten.

---

### Akeyless
**URL:** [akeyless.io](https://www.akeyless.io/)

**Features:**
- ✅ Cloud-native
- ✅ Starke Audit-Funktionen
- ✅ Automatische Rotation
- ✅ Zero-Trust Security

**Kosten:** Freemium verfügbar

**Für dich:** ✅ Gut für Produktionsumgebungen

---

## 🔧 CI/CD & Deployment Services

### GitHub Secrets (für GitHub Actions)
**URL:** Repository Settings → Secrets and variables → Actions

**Features:**
- ✅ Direkt in GitHub integriert
- ✅ Kostenlos
- ✅ Für GitHub Actions verfügbar
- ✅ Verschlüsselt gespeichert

**Kosten:** ✅ Kostenlos

**Für dich:** ✅ Perfekt wenn du GitHub Actions verwendest!

**Verwendung:**
```yaml
# .github/workflows/deploy.yml
- name: Use API Key
  env:
    API_KEY: ${{ secrets.MITTWALD_API_KEY }}
```

**Einschränkung:** ❌ Nur für GitHub Actions, nicht für client-seitige Apps direkt nutzbar

---

### Vercel Environment Variables
**URL:** [vercel.com](https://vercel.com) → Project Settings → Environment Variables

**Features:**
- ✅ Direkt in Vercel integriert
- ✅ Für Serverless Functions
- ✅ Kostenlos
- ✅ Einfach zu setzen

**Kosten:** ✅ Kostenlos

**Für dich:** ✅ Perfekt wenn du Vercel für Backend-Proxy nutzt!

**Verwendung:**
```javascript
// api/transcribe.js
const apiKey = process.env.MITTWALD_API_KEY;
```

---

### Netlify Environment Variables
**URL:** [netlify.com](https://netlify.com) → Site Settings → Environment Variables

**Features:**
- ✅ Direkt in Netlify integriert
- ✅ Für Netlify Functions
- ✅ Kostenlos
- ✅ Build-time & Runtime Variables

**Kosten:** ✅ Kostenlos

**Für dich:** ✅ Perfekt wenn du Netlify für Backend-Proxy nutzt!

---

### Railway Environment Variables
**URL:** [railway.app](https://railway.app)

**Features:**
- ✅ Einfaches Deployment
- ✅ Environment Variables
- ✅ Kostenloser Plan verfügbar

**Kosten:** Freemium verfügbar

**Für dich:** ✅ Gut für einfache Backend-Services

---

## 🌐 Browser-basierte Lösungen (Für Client-Side)

### Browser localStorage (Aktuell)
**Status:** Bereits implementiert

**Features:**
- ✅ Keine zusätzlichen Services
- ✅ Funktioniert sofort
- ✅ User hat Kontrolle

**Nachteile:**
- ⚠️ Key ist im Browser sichtbar
- ⚠️ Nicht sicher auf gemeinsam genutzten Computern

**Für dich:** ✅ Aktuell die beste Lösung für GitHub Pages!

---

### Browser Extension (z.B. 1Password, Bitwarden)
**Features:**
- ✅ Keys in verschlüsselter Extension
- ✅ Auto-Fill möglich
- ✅ Synchronisiert zwischen Geräten

**Nachteile:**
- ⚠️ User muss Extension installieren
- ⚠️ Nicht automatisch in JavaScript verfügbar

**Für dich:** ⚠️ Nicht direkt integrierbar, User müsste manuell kopieren

---

## 📊 Vergleich für deine Situation

| Service | Kosten | Einfachheit | Für GitHub Pages | Empfehlung |
|---------|--------|-------------|------------------|------------|
| **localStorage** | ✅ Kostenlos | ✅ Sehr einfach | ✅ Perfekt | ✅ **Aktuell optimal** |
| **Vercel Env Vars** | ✅ Kostenlos | ✅ Einfach | ✅ Mit Proxy | ✅ **Für Backend-Proxy** |
| **Netlify Env Vars** | ✅ Kostenlos | ✅ Einfach | ✅ Mit Proxy | ✅ **Für Backend-Proxy** |
| **Keymize** | ✅ Freemium | 🟡 Mittel | ✅ Mit Proxy | ✅ **Für AI APIs** |
| **1Password** | 💰 $7.99/Mo | ✅ Einfach | ⚠️ Manuell | 🟡 Für persönliche Nutzung |
| **KeyStash** | ✅ Freemium | ✅ Einfach | ✅ Mit Proxy | ✅ Gut für API-Keys |
| **GitHub Secrets** | ✅ Kostenlos | ✅ Einfach | ❌ Nur Actions | ⚠️ Nur für CI/CD |

---

## 🎯 Empfehlungen nach Anwendungsfall

### Für GitHub Pages (aktuell): ✅ localStorage
**Warum:**
- Keine zusätzlichen Services nötig
- Funktioniert sofort
- User hat volle Kontrolle

**Beibehalten!**

---

### Für Backend-Proxy auf Vercel: ✅ Vercel Environment Variables
**Warum:**
- Direkt integriert
- Kostenlos
- Einfach zu setzen

**Setup:**
1. Vercel Account erstellen
2. Projekt erstellen
3. Environment Variable setzen: `MITTWALD_API_KEY`
4. In Serverless Function nutzen: `process.env.MITTWALD_API_KEY`

---

### Für Backend-Proxy auf Netlify: ✅ Netlify Environment Variables
**Warum:**
- Direkt integriert
- Kostenlos
- Ähnlich wie Vercel

**Setup:**
1. Netlify Account erstellen
2. Site erstellen
3. Environment Variable setzen: `MITTWALD_API_KEY`
4. In Netlify Function nutzen: `process.env.MITTWALD_API_KEY`

---

### Für AI/ML APIs (mittwald): ✅ Keymize
**Warum:**
- Speziell für AI-APIs entwickelt
- Load Balancing
- Kostenoptimierung
- Unified Endpoint

**Besonderheit:** Du kannst mehrere mittwald API-Keys hinterlegen und Keymize verteilt die Requests automatisch.

**Setup:**
1. Account bei Keymize erstellen
2. mittwald API-Keys hinzufügen
3. Keymize Endpoint verwenden statt direkter mittwald API

---

### Für Team-Sharing: ✅ 1Password Secrets Automation
**Warum:**
- Einfache Bedienung
- Gute Team-Features
- CLI verfügbar

**Für:** Wenn mehrere Entwickler Zugriff auf API-Keys brauchen

---

## 🚀 Quick Start: Vercel Environment Variables

**Schritt 1: Vercel Account**
```
1. Gehe zu vercel.com
2. Melde dich mit GitHub an
3. Klicke "New Project"
```

**Schritt 2: Environment Variable setzen**
```
1. Gehe zu Project Settings
2. Klicke "Environment Variables"
3. Name: MITTWALD_API_KEY
4. Value: Dein API-Key
5. Environment: Production, Preview, Development
6. Klicke "Save"
```

**Schritt 3: In Code nutzen**
```javascript
// api/transcribe.js
const apiKey = process.env.MITTWALD_API_KEY;
```

**Schritt 4: Deploy**
```bash
git push
# Vercel deployed automatisch
```

---

## 🚀 Quick Start: Keymize (für AI APIs)

**Schritt 1: Account erstellen**
```
1. Gehe zu keymize.com
2. Erstelle kostenlosen Account
```

**Schritt 2: API-Keys hinzufügen**
```
1. Klicke "Add API Key"
2. Wähle "mittwald AI-Hosting"
3. Füge deinen API-Key ein
4. Optional: Füge weitere Keys hinzu (Load Balancing)
```

**Schritt 3: Endpoint verwenden**
```javascript
// Statt direktem mittwald API:
const WHISPER_API_URL = 'https://api.keymize.com/v1/mittwald/audio/transcriptions';

// Keymize fügt automatisch den API-Key hinzu
```

**Vorteile:**
- ✅ Mehrere Keys = Load Balancing
- ✅ Automatisches Failover
- ✅ Kostenoptimierung
- ✅ Statistiken & Monitoring

---

## 🔒 Sicherheits-Checkliste

### ✅ Für localStorage (aktuell):
- [x] Key nicht im Code
- [x] User gibt Key selbst ein
- [x] Warnhinweise vorhanden
- [x] Löschen-Button vorhanden

### ✅ Für Backend-Proxy (Vercel/Netlify):
- [ ] Environment Variable gesetzt
- [ ] Nicht in Code committed
- [ ] CORS korrekt konfiguriert
- [ ] Rate Limiting implementiert

### ✅ Für Keymize:
- [ ] Account erstellt
- [ ] API-Keys hinzugefügt
- [ ] Endpoint konfiguriert
- [ ] Monitoring aktiviert

---

## 📚 Weitere Ressourcen

- [Vercel Environment Variables Docs](https://vercel.com/docs/concepts/projects/environment-variables)
- [Netlify Environment Variables Docs](https://docs.netlify.com/environment-variables/overview/)
- [Keymize Documentation](https://docs.keymize.com/)
- [1Password Secrets Automation](https://1password.com/features/secrets-management/)

---

## ❓ FAQ

**Q: Welcher Service ist am sichersten?**
A: Cloud Secrets Manager (AWS, Azure, GCP) sind am sichersten, aber Overkill für GitHub Pages. Für deine Situation: Vercel/Netlify Environment Variables oder Keymize.

**Q: Kann ich mehrere Services kombinieren?**
A: Ja! z.B. Keymize für API-Management + Vercel für Backend-Proxy.

**Q: Was kostet das?**
A: Vercel, Netlify, GitHub Secrets sind kostenlos. Keymize hat Freemium. Cloud Secrets Manager kosten ab ~$0.03/Monat.

**Q: Brauche ich einen Service wenn ich localStorage nutze?**
A: Nein! localStorage ist für GitHub Pages völlig ausreichend. Services sind nur nötig für Backend-Proxies.

---

## 🎯 Finale Empfehlung

### Für deine aktuelle Situation (GitHub Pages):
✅ **localStorage beibehalten** - Das ist optimal!

### Wenn du Backend-Proxy hinzufügst:
✅ **Vercel Environment Variables** - Einfach, kostenlos, direkt integriert

### Wenn du mehrere API-Keys hast oder Load Balancing willst:
✅ **Keymize** - Speziell für AI-APIs, kostenlos verfügbar

---

**Fragen?** Öffne ein Issue im Repository!
