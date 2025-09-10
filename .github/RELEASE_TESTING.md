Code - OSS (Webview Media Enabled) — Release Testing Guide

Targets
- Linux x64: .deb + .tar.gz
- Windows x64: zip + UserSetup.exe (unsigned)

Mic/Cam Feature Summary
- Webview iframes include allow="microphone; camera"
- Electron main process permits media for vscode-webview origins
- Expect navigator.mediaDevices.getUserMedia({ audio: true }) and { video: true } to succeed

Linux (Debian/Ubuntu) Testing
1) Install .deb:
   sudo dpkg -i code-oss-*.deb || sudo apt -f install
2) Or extract tar.gz:
   tar xzf code-oss-*.tar.gz
   ./code-oss-*/bin/code-oss --version
3) Launch Code - OSS.
4) Open a test Webview extension that calls:
   navigator.mediaDevices.getUserMedia({ audio: true })
   navigator.mediaDevices.getUserMedia({ video: true })
5) Expect to receive a MediaStream; no Permissions Policy violations.

Windows Testing
1) Unsigned installer will trigger SmartScreen:
   - Click “More info” → “Run anyway”
2) Install and run Code - OSS.
3) Use a test Webview extension and call:
   navigator.mediaDevices.getUserMedia({ audio: true })
   navigator.mediaDevices.getUserMedia({ video: true })
4) Expect to receive a MediaStream.

Notes
- On macOS, OS prompts may appear the first time mic/cam are accessed.
- These builds are intended for validation; security hardening may be added later.
- SHA256SUMS.txt will be attached in the draft release for Linux. Windows checksums may be added later via CI.

Troubleshooting
- If media calls fail, check:
  - Permissions Policy console errors (iframe allow attributes)
  - App-level Electron permission handler coverage for the requesting URL
  - OS privacy settings (microphone/camera globally enabled)
- For .deb issues:
  - dpkg -I code-oss-*.deb | head -n 50
  - sudo apt -f install
