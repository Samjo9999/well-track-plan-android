# Setup für signierte APK-Releases

## GitHub Secrets einrichten

Im Repo: **Settings → Secrets and variables → Actions**

Füge folgende Secrets hinzu:

1. **KEYSTORE_BASE64**: Base64-kodierter Keystore
2. **KEYSTORE_PASSWORD**: Keystore-Passwort  
3. **KEY_ALIAS**: `welltrackplan` (oder dein Alias)
4. **KEY_PASSWORD**: Key-Passwort
5. **WEB_APP_REPO_TOKEN** (optional): GitHub Token mit Zugriff auf well-track-plan Repo (nur wenn privat)

## Keystore erstellen

```bash
keytool -genkey -v -keystore release.keystore -alias welltrackplan -keyalg RSA -keysize 2048 -validity 10000
```

## Keystore zu Base64 konvertieren (Windows PowerShell)

```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("release.keystore")) | Set-Clipboard
```

Dann den Inhalt in das Secret `KEYSTORE_BASE64` einfügen.

## Release erstellen

1. Gehe zu **Actions** im Repo
2. Wähle **"Build and Release APK"**
3. Klicke **"Run workflow"**
4. Fülle aus:
   - **Release notes**: (optional) Beschreibung für Tester - Standard: "Update from server"
   - **Web app ref**: Branch/Tag/Commit vom Web-App-Repo (z.B. `main` oder `stable`) - Standard: `main`

**Versionsnummern werden automatisch verwaltet:**
- **Version Code** wird automatisch erhöht (startet bei 1, dann 2, 3, 4...)
- **Version Name** verwendet das Datum im Format `YYYYMMDD`
- **Tag Name** wird automatisch als `v{version_code}` erstellt

## Wichtig

- Versionsnummern werden **automatisch** verwaltet - du musst sie nicht eingeben!
- Das Android-Projekt wird automatisch vom Web-App-Repo kopiert
- Die Signierung wird automatisch über GitHub Secrets konfiguriert
- Es wird eine **signierte Release-APK** gebaut
- Die APK wird automatisch als `well-track-plan-beta-{date}.apk` benannt (z.B. `well-track-plan-beta-20251213.apk`)
- Die APK wird als "Pre-release" markiert

