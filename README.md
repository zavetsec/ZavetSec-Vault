# ZavetSec Vault

> Single-file offline password manager using Web Crypto (AES-256-GCM). No install, no cloud, portable HTML vault.

![License](https://img.shields.io/badge/license-MIT-blue) ![Crypto](https://img.shields.io/badge/crypto-AES--256--GCM-green) ![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen) ![Size](https://img.shields.io/badge/size-~70KB-lightgrey) ![Languages](https://img.shields.io/badge/lang-RU%20%7C%20EN-informational)

**Download → Open in browser → Works.** No installation. No account. No network requests except optional favicons.

---

## ⚠️ Read this before you start

**Data is stored in the browser** (`localStorage`) — not on disk, not in the cloud.

| Action | Result |
|---|---|
| Close and reopen the browser | ✅ Data preserved |
| Reload the page (F5) | ✅ Data preserved |
| Open in **incognito / private mode** | ❌ No data — isolated storage |
| Open in **a different browser** | ❌ No data — each browser has its own localStorage |
| Clear browser history / site data | ❌ **Data permanently deleted** |
| Reinstall the browser | ❌ **Data permanently deleted** |
| Switch computers | ❌ No data without a backup |

### How to avoid losing your passwords

Use the 💾 button in the toolbar to export a `.vault` file — an AES-encrypted backup that only opens with your master password. The app reminds you every 7 days. Store it on an encrypted USB drive or in a VeraCrypt / BitLocker container.

---

## How it compares

| | ZavetSec Vault | KeePass | Bitwarden | 1Password |
|---|---|---|---|---|
| Installation | **None** | Required | Required | Required |
| Dependencies | **Zero** | .NET / Mono | Electron | Electron |
| Cloud | **Never** | Optional | On by default | On by default |
| Network activity | **None** | Update checks | Sync to cloud | Sync to cloud |
| Open source | ✓ | ✓ | ✓ | ✗ |
| Works fully offline | ✓ | ✓ | Partial | Partial |
| KeePass XML import/export | ✓ | — | ✓ | ✓ |
| Codebase size | **~1,400 lines, 1 file** | Large ecosystem | Large ecosystem | Closed |
| Interface language | RU + EN | Multi | Multi | Multi |
| Known breaches | None | None (see note) | None | None |

ZavetSec Vault is not a replacement for KeePass or Bitwarden — it occupies a different niche: **maximum simplicity and zero installation friction**, at the cost of being browser-bound. Choose the tool that fits your threat model.

### Why "no cloud" matters more than it sounds

**Cloud-based managers are a high-value target.** In 2022, LastPass — one of the world's most popular password managers — suffered a breach where attackers exfiltrated encrypted vault backups for millions of users. By 2025, blockchain investigators traced over **$150 million in cryptocurrency theft** directly to those stolen vaults, with attacks still ongoing as weak master passwords continue to be cracked offline. The breach resulted in LastPass being fined £1.6 million by the UK ICO.

The structural risk with any cloud-based manager is that **your encrypted vault lives on someone else's server**. A single breach at the provider gives attackers unlimited offline time to brute-force your master password — no matter how good the encryption is.

**Installed desktop apps introduce their own risks.** KeePass itself is trustworthy open-source software — it has been audited twice, including by the EU's FOSSA project. However, a documented 2024 campaign showed attackers distributing trojanized KeePass builds through fake websites promoted via legitimate ad networks. The malicious installers were signed with valid certificates, bypassing Windows warnings. Five distinct trojanized variants were found, all silently exfiltrating passwords to attacker servers. This is not a flaw in KeePass itself — it is the fundamental risk of downloading and installing binary software: **you must trust the source, the build process, and the update channel.**

ZavetSec Vault eliminates both risks. There is no server to breach. There is no binary to trojanize. The entire codebase is a single human-readable HTML file — open it in a text editor and read every line before trusting it.

---

## Threat model

### Protects against
- Disk theft (data is AES-256-GCM encrypted at rest)
- Offline attacker with access to your filesystem
- Cloud compromise (no cloud involved)
- Server-side breach (no server)

### Does NOT protect against
- A malicious or compromised browser extension (extensions can read `sessionStorage`)
- A live memory attacker with access to your running browser process
- Keyloggers capturing your master password
- Physical access to an unlocked, active session
- Browser localStorage being wiped (→ always keep a `.vault` backup)

---

## Security

### Cryptographic parameters

| Parameter | Value |
|---|---|
| Encryption | AES-256-GCM |
| Key derivation | PBKDF2 |
| KDF iterations | 310,000 |
| KDF hash | SHA-256 |
| Salt | 256 bit, random per vault |
| IV / nonce | 96 bit, random per encryption |
| Master password stored | Never |
| Key stored | RAM only (current tab) |

### What is stored where

| Storage | Contents | Cleared when |
|---|---|---|
| `localStorage` | Encrypted blob + salt | Browser data cleared |
| `sessionStorage` | Derived key reference (F5 restore) | Tab closed |
| RAM | Decrypted data | Vault locked / tab closed |
| Nowhere | Master password | Never stored |

### Auto-lock

Locks after **15 minutes of inactivity** with a visible countdown in the status bar. The final 60 seconds turn yellow.

### Favicon privacy note

When an entry has a URL, the app requests its favicon from Google's public icon service (`https://www.google.com/s2/favicons`). This reveals the domain to Google. If this is a concern, leave the URL field empty — the app functions fully without it.

### Security disclaimer

> This software has not undergone a professional third-party security audit. It is provided as-is. Use it at your own risk and in accordance with your personal threat model. For high-value targets, consider a formally audited solution.

---

## Quick start

1. Download `zavetsec-vault.html` (EN) or `zavetsec-vault-ru.html` (RU)
2. Open in any modern browser (Chrome, Firefox, Edge, Safari)
3. Choose **"create vault"** and set a master password (12+ characters recommended)
4. Add entries via **+ entry** or `Ctrl+Shift+A`
5. **Export immediately** — click 💾 and store the `.vault` file somewhere safe

---

## Features

- **Groups** — Personal, Work, Finance, VPN/OPSEC, Email, Development + custom groups
- **Icon picker** — 40 emoji, auto-detected by title and URL (GitHub, Gmail, Telegram, Docker, and more)
- **Password generator** — length 8–64 (default: 16), configurable character sets, ambiguous character exclusion (0/O/1/l/I)
- **Password strength meter** — based on length, entropy, and character diversity
- **Password age indicator** — green → yellow (>6 months) → red (>1 year)
- **Auto-lock** — 15 minutes of inactivity, countdown in status bar
- **Backup reminder** — banner every 7 days without an export
- **Tags** — custom labels for filtering
- **Search** — title, login, URL, notes, tags
- **One-click copy** — login or password instantly to clipboard
- **Session on F5** — no re-entry of master password on reload (cleared on tab close)

---

## Export formats

### 💾 `.vault` — recommended
AES-256-GCM encrypted JSON. Opens only with your master password. Use for backups and cross-device transfer.

### KeePass XML 2.x
**Unencrypted.** Use only for migration. Delete immediately after importing.
- ZavetSec → KeePass: export KeePass XML in ZavetSec → in KeePass `File → Import → KeePass XML (2.x)`
- KeePass → ZavetSec: in KeePass `File → Export → KeePass XML (2.x)` → import in ZavetSec

### CSV
Plain text. Last resort for migrating to third-party systems. Delete after use.

---

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Shift+A` | New entry |
| `Ctrl+Shift+F` | Focus search |
| `Ctrl+Shift+L` | Lock vault |
| `Esc` | Close modal |
| `Enter` | Confirm (password fields) |

---

## Why localStorage and not IndexedDB?

IndexedDB offers more storage capacity and a richer API, but `localStorage` was chosen deliberately: it is **synchronous, universally supported, and trivially auditable** in a single-file context. The encrypted blob stored here is small (passwords compress well), so capacity is not a concern. The tradeoff — data loss on browser clear — is documented explicitly and mitigated by the `.vault` export workflow.

---

## Roadmap

**Features**
- [ ] TOTP / 2FA code display (TOTP secret stored, code generated client-side)
- [ ] Configurable auto-lock timeout
- [ ] Password breach check via HaveIBeenPwned k-anonymity API (opt-in)
- [ ] Dark / light theme toggle
- [ ] Bulk import from CSV

**Security hardening**
- [ ] Argon2id as optional KDF (in addition to PBKDF2)
- [ ] Explicit memory wipe routines on lock (overwrite decrypted state)
- [ ] CSP sandbox mode for stricter extension isolation
- [ ] Professional third-party security audit

---

## Project structure

```
zavetsec-vault.html      ← English interface (default)
zavetsec-vault-ru.html   ← Russian interface
README.md
```

Each file is self-contained: CSS (~220 lines), HTML (~280 lines), JavaScript (~900 lines). No build step. Edit in any text editor.

---

## License

MIT — use, modify, and distribute freely.

---

**ZavetSec** — information security, SOC/DFIR.

> *"Security is not a product, it's a process."*
