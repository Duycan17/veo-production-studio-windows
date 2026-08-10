# Veo Production Studio for Windows x64

## Version 0.2.12

- Verifies every visible match of Google sign-in markers instead of trusting the first hidden DOM match.
- Checks Gemini in the account worker before publishing a session as `VALID`, so Flow-only cookies fail at import instead of failing later in script or analysis.
- The session-export extension checks the active Gemini prompt surface and visible sign-in state before copying a token.

- Captures the original core-process bootstrap exception and stack trace instead of reporting only exit code 1.
- Waits for an explicit `CORE_READY` message before sending database requests.
- Uses exponential restart backoff and shows specific guidance for database locks, inaccessible AppData, and corrupt SQLite data.
- Ignores expected worker shutdown messages, prevents late replies from becoming unhandled rejections, and retries a transiently missing analysis frame on Windows.
- Keeps already-claimed work alive during a temporary auth-server outage and logs non-result-affecting background cleanup without interrupting the UI.
- Renews worker leases and heartbeats for generation, script generation, and reference analysis, and safely requeues stale local work after a worker restart instead of leaving it stuck in `RUNNING`.

## Requirements

- Windows 10 or Windows 11, 64-bit.
- Google Chrome or Microsoft Edge for connecting Gemini and Google Flow.
- Internet access to the application login service, Gemini, Google Flow, and the watermark service.
- A Veo Production Studio login supplied privately by the owner. Do not post passwords in the release page.

FFmpeg, FFprobe, Electron, Chromium, and their required Windows DLL files are included in the installer. Python, Node.js, npm, pnpm, and a separate FFmpeg installation are not required.

## Install

1. Download `Veo-Production-Studio-Setup-0.2.12-x64.exe` and `SHA256SUMS.txt` from the public release.
2. Verify the SHA-256 checksum in PowerShell:

   ```powershell
   Get-FileHash .\Veo-Production-Studio-Setup-0.2.12-x64.exe -Algorithm SHA256
   ```

   Expected Windows installer SHA-256:

   ```text
   8d9c9ad724c7194ab25ac2928f5448a61ea657cea7251e82caffdd65c5e7f049
   ```

3. Run the installer. This friend-test build is not Authenticode-signed, so Windows SmartScreen may say **Unknown publisher**. Continue only when the checksum matches the public release.
4. Sign in with the credentials supplied privately by the owner.
5. Open **Tài khoản Google**, connect a Chrome profile, and finish Google sign-in if requested.

## macOS Apple Silicon

The current macOS package is an unsigned internal-test build for Apple Silicon
(`arm64`: M1, M2, M3, M4, and newer). It requires macOS 13 or newer. The
current package does not support Intel Macs (`x86_64`) because the approved
Intel FFmpeg runtime is not included.

The macOS DMG and ZIP are available from this repository's
[Veo Production Studio v0.2.12 release](https://github.com/Duycan17/veo-production-studio-windows/releases/tag/v0.2.12).
The DMG is recommended. GitHub may display the files as
`Veo.Production.Studio-0.2.12-arm64.dmg` and
`Veo.Production.Studio-0.2.12-arm64-mac.zip`.

### Requirements

- macOS 13 or newer on Apple Silicon.
- Google Chrome or Microsoft Edge for Google account enrollment.
- Internet access to the application login service, Gemini, Google Flow, and
  the configured watermark service.
- An owner-issued Veo Production Studio login. Never publish the password,
  cookies, or Google session token.

### Verify the Mac before installing

Open **Terminal** and run:

```bash
uname -m
sw_vers -productVersion
```

The architecture command must print `arm64`. If it prints `x86_64`, this
release cannot be installed on that Mac.

After downloading the DMG, verify its checksum before opening it:

```bash
cd ~/Downloads
shasum -a 256 'Veo.Production.Studio-0.2.12-arm64.dmg'
```

Expected DMG SHA-256:

```text
7b92d161b0f1340aa96dc90042cb3bd7a8b2b34ee6c266e40ae1c0c8be59850a
```

For the ZIP alternative, the expected SHA-256 is:

```text
eb28c758411cd8093ff4eb46ef45a010114ceb8c86c0c8533497b1783dd29da7
```

Do not install an artifact whose checksum does not match.

### Install from the DMG

1. Double-click the downloaded `.dmg`.
2. Drag **Veo Production Studio** into **Applications**.
3. Eject the mounted DMG.
4. Open Finder → **Applications**, right-click **Veo Production Studio**, and
   choose **Open**.
5. Choose **Open** again when macOS asks to confirm the unidentified
   developer. Copy the app to Applications first; do not run it from the DMG
   or directly from Downloads.

### Gatekeeper warning

Because this build is not signed or notarized, macOS may show “cannot be
opened because the developer cannot be verified.” If that happens:

1. Try opening the app once.
2. Open **System Settings → Privacy & Security**.
3. In the Security section, click **Open Anyway** for Veo Production Studio.
4. Authenticate with the Mac password or Touch ID, then open the app again.

For a trusted, checksum-verified copy that is still blocked, remove only that
app's quarantine attribute:

```bash
xattr -dr com.apple.quarantine "/Applications/Veo Production Studio.app"
```

Then right-click the app and choose **Open**. This does not disable Gatekeeper
globally. Use the command only for the verified release artifact; never use it
on an unknown application.

### ZIP alternative

Double-click the ZIP, drag **Veo Production Studio.app** into Applications,
and follow the same right-click **Open** and Gatekeeper steps. The DMG and ZIP
contain the same Apple Silicon application.

### First launch

1. Sign in to Veo Production Studio with the owner-issued application account.
2. When Chrome or Edge opens for Google enrollment, complete Google sign-in,
   passkey, or MFA yourself.
3. Close the browser window after sign-in.
4. Return to the app and choose **Verify login**.
5. Wait for Gemini and Flow verification to finish before starting a job.

Keep all application passwords, Google cookies, and session tokens private. Do
not upload them with screenshots, diagnostics, or release files.

## Chrome extension session export

The release also includes `Veo-Studio-Google-Session-Export-0.1.1.zip`. This is
an unpacked Manifest V3 extension for transferring a user-approved,
encrypted Google/Gemini/Flow session into Veo Production Studio. It is not a
Chrome Web Store installation.

1. Download and verify the ZIP using `SHA256SUMS.txt`.
2. Extract it to a trusted local folder. Do not edit or upload the extracted
   files.
3. Open `chrome://extensions` in Chrome or Edge, enable **Developer mode**,
   choose **Load unpacked**, and select the extracted folder.
4. In that same browser profile, open Gemini and Google Flow and finish
   sign-in/authorization. Gemini must show its prompt box and Flow should show
   the signed-in workspace, such as **New project**. Keep Gemini as the active
   tab.
5. Open the extension and choose **Copy encrypted session**. The extension
   rejects a visible Gemini sign-in state or a missing prompt surface.
6. In Veo Studio → **Tài khoản Google**, choose **Thêm tài khoản Google** →
   **Dán cookie / session**, paste the token beginning with `VEOCLIP1.`, and
   click **Dán và nhập session**.

The extension uses `cookies`, `clipboardWrite`, `activeTab`, and `scripting`,
plus explicit Google/Flow host permissions. It does not upload the token. The
copied value is a bearer credential: keep it private and clear the clipboard
after importing.

## Visible diagnostics

The application performs startup checks before opening the main window. Missing files, broken FFmpeg DLLs, invalid service configuration, or unwritable data/output folders open a native Windows error dialog and stop startup safely.

Non-fatal runtime problems—such as missing Chrome/Edge, a crashed background worker, or a stopped core process—remain visible in a red banner inside the application. They are also written to:

```text
%APPDATA%\Veo Production Studio\diagnostics\startup-and-runtime-errors.log
```

For any problem, send the owner:

- A screenshot of the complete message and error code.
- The diagnostics log above.
- The job/run ID shown in **Hàng đợi**, **Phân tích video**, or **Tạo kịch bản**.

Do not send Google passwords, application passwords, cookies, or browser profile files.
