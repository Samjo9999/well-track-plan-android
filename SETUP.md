# Setup für signierte APK-Releases

## GitHub Secrets einrichten (optional)

Im Repo: **Settings → Secrets and variables → Actions**

**Nur nötig, wenn das Web-App-Repo privat ist:**
- **WEB_APP_REPO_TOKEN**: GitHub Token mit Zugriff auf well-track-plan Repo

**Hinweis:** Für Debug-APKs sind keine Signing-Secrets nötig.

## Release erstellen

1. Gehe zu **Actions** im Repo
2. Wähle **"Build and Release APK"**
3. Klicke **"Run workflow"**
4. Fülle aus:
   - **Version name**: z.B. `1.0.0`
   - **Version code**: z.B. `1` (bei jedem Release erhöhen!)
   - **Tag name**: z.B. `v1.0.0`
   - **Release notes**: Beschreibung für Tester
   - **Web app ref**: Branch/Tag/Commit vom Web-App-Repo (z.B. `main` oder `stable`)

## Wichtig

- **Version Code** muss bei jedem Release erhöht werden (1 → 2 → 3...)
- Das Android-Projekt wird automatisch vom Web-App-Repo kopiert
- Es wird eine **Debug-APK** gebaut (für Beta-Testing, nicht signiert)
- Die APK wird automatisch als `well-track-plan-beta-{version}.apk` benannt
- Die APK wird als "Pre-release" markiert

