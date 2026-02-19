# 🔒 Qlerky Password Manager

> **Store passwords and 2FA codes the right way — locally, encrypted, offline.**

Qlerky is an Android password manager built for people who don't want their credentials on someone else's server. Everything is encrypted with your master password and stored exclusively on your device. No cloud, no sync, no backend.

<br>

[![Download on Google Play](https://img.shields.io/badge/Google_Play-Download-414141?style=for-the-badge&logo=google-play&logoColor=white)](https://play.google.com/store/apps/details?id=com.hiddenflame.qlerkypassword)
![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Encryption](https://img.shields.io/badge/Encryption-AES--GCM-F25B2A?style=for-the-badge)
![KDF](https://img.shields.io/badge/KDF-Argon2id-F25B2A?style=for-the-badge)
![Network](https://img.shields.io/badge/Network_Requests-Zero-success?style=for-the-badge)

<br>

---

## ✨ Features

- **100% local storage** — vault never leaves your device
- **AES-GCM encryption** — authenticated encryption with Argon2id key derivation
- **Password + 2FA in one entry** — store TOTP codes alongside credentials
- **Next 2FA code preview** — see the upcoming code before the current one expires
- **4 export formats** — Qlerky File, QR Code, Inside Image (steganography), CSV
- **Biometric unlock** — fingerprint / face via Android Keystore
- **No account required** — install and go, no email, no registration

<br>

---

## 🔐 Security Model

All data is encrypted with a key derived from your master password using **Argon2id** (winner of the Password Hashing Competition 2015). The derived key exists only in memory during an active session and is never written to disk.

```
Master Password
      │
      ▼
  Argon2id (memory=256MB, iterations=3, parallelism=4)
      │
      ▼
  256-bit AES Key  ──►  AES-GCM Encrypt  ──►  Encrypted Vault (local)
      │
   [zeroed on lock / app background]
```

**Properties:**
- No network requests — verified by running in airplane mode
- AES-GCM provides authenticated encryption (tamper detection built in)
- Argon2id's 256 MB memory requirement makes GPU brute-force attacks hardware-prohibitive
- Random 256-bit salt per vault — rainbow tables are useless
- Master password is never stored, only its derived key is held in memory

<br>

---

## 📤 Export Formats

Qlerky supports four ways to export individual entries, each suited to a different use case:

| Format | Encrypted | Readable by | Best for |
|---|---|---|---|
| **Qlerky File** (`.qlerky`) | ✅ AES-GCM | Qlerky only | Backups, device migration |
| **QR Code** | ✅ AES-GCM | Qlerky (scan) | Quick device-to-device transfer |
| **Inside Image** | ✅ AES-GCM + LSB | Qlerky (image import) | Covert transfer |
| **CSV** | ❌ Plaintext | Any password manager | Migration to other apps |

### 🖼️ Inside Image — Steganography

The most unique export. Encrypted entry data is embedded into a cover image using **Least Significant Bit (LSB) steganography** — the data is encoded into the least-significant bits of RGB pixel color channels.

```
Original pixel:  RGB(200, 140, 80)  →  binary: 11001000 10001100 01010000
After encoding:  RGB(201, 140, 80)  →  binary: 11001001 10001100 01010000
                                                       ↑
                                              1 bit of payload (change = 1/255)
```

The resulting image looks **completely identical** to the original to the human eye. Share a photo and secretly carry encrypted credentials inside it.

> ⚠️ **Important:** Requires lossless file transfer. Messengers that re-compress images (WhatsApp, Instagram) will destroy the payload. Use Google Drive, Telegram "Send as File", USB, or AirDrop.

<br>

---

## ⏭️ Next 2FA Code Preview

Unlike most authenticator apps, Qlerky computes both the current and the next TOTP code simultaneously. You'll never race against a 2-second countdown again.

```
currentT    = floor(unixTime / 30)
nextT       = currentT + 1

currentCode = TOTP(secret, currentT)   // shown with countdown
nextCode    = TOTP(secret, nextT)      // preview displayed alongside
```

<br>

---

## 📱 Requirements

- Android 8.0+ (API 26)
- No internet permission required for core functionality
- Biometric hardware optional (for biometric unlock feature)

<br>

---

## 🗺️ Roadmap

- [x] Android release
- [x] TOTP 2FA with next-code preview
- [x] Steganographic image export
- [x] QR code encrypted export
- [x] Qlerky native file format
- [ ] iOS release
- [ ] macOS / Windows / Linux desktop
- [ ] HOTP (counter-based OTP) support
- [ ] Cross-platform Qlerky File import

<br>

---

## 💬 Feedback & Contributing

We built Qlerky because no existing app gave us a comfortable, private place to store exchange credentials and 2FA codes. We use it daily.

**Found a bug? Have a feature idea?**
- Open an [Issue](../../issues)
- Leave a review on [Google Play](https://play.google.com/store/apps/details?id=com.hiddenflame.qlerkypassword)
- Drop a comment — we ready for everything

**Responsible disclosure:** If you find a security vulnerability, please open a private issue or contact us directly before public disclosure.

<br>

---

## ⚠️ Important Notes

- **Losing your master password = losing your vault.** There is no account recovery, no reset email, no backdoor. This is by design.
- **Always keep a backup.** Export your vault using the Qlerky File format before switching or resetting devices.
- **CSV export is plaintext.** Delete it immediately after importing into another app.

<br>

---

<p align="center">
  Made by <strong>Hidden Flame</strong> &nbsp;·&nbsp;
  <a href="https://play.google.com/store/apps/details?id=com.hiddenflame.qlerkypassword">Google Play</a>
  <br><br>
  <em>No cloud. No sync. No compromise.</em>
</p>
