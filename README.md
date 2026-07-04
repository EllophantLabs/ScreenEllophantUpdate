# Screen Ellophant - Release- & Update-Prozess

## 📋 Übersicht
Dieses Dokument definiert den verbindlichen Deployment- und Release-Zyklus für die Bühnenpräsentations-Software **Screen Ellophant**. Die strikte Einhaltung dieser Sequenz sichert die Integrität der Versionsstände und gewährleistet den vollautomatisierten Build-Flow von Ellophant Labs.

Für tiefergehende Details und historische Versionen kann auf das Dokument `Ellophant_Labs_Release_Process_2026-v4.pdf` verwiesen werden.

* **Dokumenten-ID:** EL-SE-REL-2026
* **Stand:** 2026
* **Vertraulichkeit:** Intern Ellophant Labs Engineering

---

## 🔢 1. Versions-Definition
Sämtliche Versionierungen folgen strikt den Vorgaben von **Semantic Versioning 2.0.0** (Muster: `x.x.x`):

* **Major (`x.0.0`):** Breaking Changes oder fundamentale Architektur-Updates.
* **Minor (`0.x.0`):** Abwärtskompatible funktionale Erweiterungen und neue Features.
* **Patch (`0.0.x`):** Abwärtskompatible Bugfixes, Hotfixes und Performance-Optimierungen.

---

## 🚀 2. Schritt-für-Schritt Release-Anleitung

> ⚠️ **Wichtig:** Ersetze das Muster `v0.4.3` in den folgenden Befehlen stets durch die jeweils aktuell vergebene Versionsnummer.

### Schritt 2.1: Änderungen stagen
Überführung aller finalisierten Quellcodedateien und Assets in den Git-Staging-Bereich:
```bash
git add .
```

### Schritt 2.2: Build-Commit erstellen
Verbindliches Benennungsmuster für den Vorab-Commit verwenden (`-m "build vx.x.x"`):
```bash
git commit -m "build v0.4.3"
```

### Schritt 2.3: Push in den Entwicklungs-Branch
Aktualisierung des Remote-Repositories mit dem aktuellen Build-Stand:
```bash
git push
```

### Schritt 2.4: Automatisiertes Tagging via npm
Generierung der neuen Version. Durch diesen Befehl wird automatisiert ein nativer Git-Tag injiziert:
```bash
npm version 0.4.3
```

#### 🔄 Abgleich durch `npm version`
Der Workspace-Abgleich modifiziert die Versions-Strings vollautomatisch an exakt **7 Systemstellen**:
* `package.json` (1x)
* `package-lock.json` (1x)
* `tauri.conf.json` (1x)
* `StartingWindow/index.html` (1x)
* `Updater.json` (3x)

### Schritt 2.5: Build-Trigger & Tag-Push
Pushen des generierten Commits inklusive aller Tags, um die CI/CD-Pipeline zu initiieren:
```bash
git push --follow-tags
```
> ℹ️ **Hinweis:** Erst durch das Flag `--follow-tags` detektieren die GitHub Actions den Release-Tag und starten den Compile-Prozess.

### Schritt 2.6: Public Signature extrahieren
1. Navigiere auf GitHub in das Tab **Actions** des `Screen-Ellophant`-Repositories.
2. Wähle den durch den Tag-Push getriggerten **Build-Workflow** aus.
3. Kopiere die verschlüsselte **Public Signature** aus der Konsolenausgabe des Build-Logs.

### Schritt 2.7: Updater-Konfiguration aktualisieren
Trage die kopierte Public Signature in die lokale Datei `update.json` ein, um den Client-seitigen Autoupdater zu autorisieren.

### Schritt 2.8: Finalen Release versiegeln
Abschluss des Release-Prozesses durch Stagen der signierten Konfiguration und finalen Push:
```bash
git add .
git commit -m "release v0.4.3"
git push
```

---
🎨 **Ellophant Labs © 2026**
