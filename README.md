# Veo Production Studio for Windows x64

## Version 0.2.2

- Captures the original core-process bootstrap exception and stack trace instead of reporting only exit code 1.
- Waits for an explicit `CORE_READY` message before sending database requests.
- Uses exponential restart backoff and shows specific guidance for database locks, inaccessible AppData, and corrupt SQLite data.

## Requirements

- Windows 10 or Windows 11, 64-bit.
- Google Chrome or Microsoft Edge for connecting Gemini and Google Flow.
- Internet access to the application login service, Gemini, Google Flow, and the watermark service.
- A Veo Production Studio login supplied privately by the owner. Do not post passwords in the release page.

FFmpeg, FFprobe, Electron, Chromium, and their required Windows DLL files are included in the installer. Python, Node.js, npm, pnpm, and a separate FFmpeg installation are not required.

## Install

1. Download `Veo-Production-Studio-Setup-0.2.2-x64.exe` and `SHA256SUMS.txt` from the public release.
2. Verify the SHA-256 checksum in PowerShell:

   ```powershell
   Get-FileHash .\Veo-Production-Studio-Setup-0.2.2-x64.exe -Algorithm SHA256
   ```

3. Run the installer. This friend-test build is not Authenticode-signed, so Windows SmartScreen may say **Unknown publisher**. Continue only when the checksum matches the public release.
4. Sign in with the credentials supplied privately by the owner.
5. Open **Tài khoản Google**, connect a Chrome profile, and finish Google sign-in if requested.

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
