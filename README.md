# RANDOMART // SSH Key & Text Visualizer

A single self-contained HTML file that turns an SSH key — or any word, name, or phrase — into a deterministic ASCII "randomart" pattern, using the same drunken-bishop algorithm OpenSSH uses for `ssh-keygen -lv`.

No build step, no dependencies, no network calls. Everything runs client-side in the browser.

---

## What it does

1. Takes an input: an SSH public key line, a PEM-wrapped private key, or plain text.
2. Hashes it (MD5 or SHA-256, your choice).
3. Walks the hash bytes through the drunken-bishop algorithm to produce a 17×9 ASCII grid.
4. Displays the art alongside metadata: input type, size, hash algorithm, comment, and fingerprint.

Same input always produces the same art — that's the point. It's how OpenSSH lets you visually eyeball whether two keys match instead of comparing long hex strings.

## Screenshots

**Empty state**

![Empty state](screenshots/01-empty-state.png)

**Generated from plain text (`tree`)**

![Text input generated](screenshots/02-text-input.png)

**Generated from an SSH public key (SHA-256)**

![SSH key input generated](screenshots/03-ssh-key-input.png)

## Usage

Open `randomart.html` in any modern browser. Paste or type into the input field, pick a hash algorithm (MD5 matches classic `ssh-keygen -lv` output; SHA-256 matches the modern default fingerprint basis), and press **Generate**.

Supported input formats:

- `ssh-rsa`, `ssh-ed25519`, `ssh-dss`, `ecdsa-sha2-nistp*` public key lines
- PEM-wrapped private keys (`-----BEGIN ... PRIVATE KEY-----`)
- Any plain text — a name, a word, a passphrase, anything

For SSH public keys, the tool parses the actual key material out of the base64 blob and hashes that. For plain text, it hashes the raw UTF-8 bytes directly.

## Notes on accuracy

- Public key fingerprints and art match real `ssh-keygen -lv` output exactly.
- Private key art is generated from the raw key material in the PEM body rather than a fully parsed private key structure, so it's deterministic and consistent per key, but won't numerically match `ssh-keygen -lv` run against the same private key file.
- MD5 is implemented from scratch since browsers don't expose it natively (only used for legacy-compatible fingerprints). SHA-256 uses the browser's built-in `crypto.subtle`.

## Files

```
randomart.html       — the tool, open directly in a browser
README.md            — this file
screenshots/          — reference screenshots
```
