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
   - **Version name**: z.B. `1.0.0`
   - **Version code**: z.B. `1` (bei jedem Release erhöhen!)
   - **Tag name**: z.B. `v1.0.0`
   - **Release notes**: Beschreibung für Tester
   - **Web app ref**: Branch/Tag/Commit vom Web-App-Repo (z.B. `main` oder `stable`)

## Wichtig

- **Version Code** muss bei jedem Release erhöht werden (1 → 2 → 3...)
- Das Android-Projekt wird automatisch vom Web-App-Repo kopiert
- Die Signierung wird über Environment-Variablen aktiviert
- Die APK wird als "Pre-release" markiert

