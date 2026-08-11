# Veo Production Studio for Windows x64

## Version 0.2.13

This is an unsigned friend-test build. Windows SmartScreen may show
**Unknown publisher**; verify the checksum before continuing.

## Install

1. Download `Veo-Production-Studio-Setup-0.2.13-x64.exe` and
   `SHA256SUMS.txt` from the public release.
2. Verify it in PowerShell:

   ```powershell
   Get-FileHash .\Veo-Production-Studio-Setup-0.2.13-x64.exe -Algorithm SHA256
   ```

   The expected SHA-256 is:

   ```text
   4d1c2db79be11b0d2b0c3b7553724e406bc90375087501e048f648a979103653
   ```

3. Run the installer and choose the installation folder.
4. If SmartScreen appears, choose **More info → Run anyway** only after the
   checksum matches.
5. Sign in with the private Veo Studio account supplied by the owner.

## Connect Gemini and Flow

Use Chrome or Edge. In the same browser profile, open Gemini and Flow, finish
Google sign-in/authorization, and confirm Gemini shows its prompt box and Flow
shows **New project**. Keep Gemini active when using the session-export
extension. The extension rejects a visible Gemini sign-in page, and Veo Studio
will reject a Flow-only session before any script or analysis job starts.

Do not send passwords, cookies, session tokens, or browser profile files.

## Diagnostics

Non-fatal runtime errors appear in the app and are written to:

```text
%APPDATA%\Veo Production Studio\diagnostics\startup-and-runtime-errors.log
```

Send the owner the error code, screenshot, and relevant job/run ID only.
