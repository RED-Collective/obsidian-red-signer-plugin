# Obsidian Red Signer (Project R.E.D. Network)

**The official authoring bridge for the Project R.E.D. decentralized knowledge base.**

This plugin allows maintainers to cryptographically sign their Markdown notes with Ed25519 directly inside Obsidian. It automatically generates and updates the network's `manifest.json`, securely storing the file hash, your public key, and your cryptographic signature.

---

## 🦅 The Philosophy: Why We Built This

Project R.E.D. is built on a strict engineering philosophy: **stateless, lightweight, and execution-focused.** When we designed the security grid for the network, we needed a visual interface for maintainers to hash, sign, and manage their guides. The standard industry reaction is to reinvent the wheel—to waste weeks building a bloated, custom desktop application in C++ and Qt6 just to render a file tree and click a "Sign" button. 

We rejected that. 

Instead of building an authoring environment from scratch, we simply made use of one of the best local-first Markdown editor in the world: Obsidian. By turning our signing tool into an Obsidian plugin, we achieved zero context-switching. Maintainers can write their guides, hit a hotkey, and have the Ed25519 cryptography handled completely invisibly in the background. 

We build tools that work. We don't reinvent the wheel.

---

## ⚡ Features

* **Zero-Friction Signing:** Sign your files with one click via the ribbon icon, editor menu, or command palette.
* **The Sovereign Identity Vault:** Automatically generates and stores your permanent `maintainer.key` safely outside your working directory.
* **Network Manifest Injection:** Automatically discovers and updates the `manifest.json` at your vault root, formatting the keys exactly as the Project R.E.D. Go engine requires.
* **Real-Time Security Status:** A status bar indicator that displays `✓ Signed` (green) or `Unsigned` (gray) by dynamically comparing your live file hash against the manifest.
* **Public Key Clipboard:** Instantly copy your public key to add to the server’s `TrustedMaintainers` ring.

---


We need to rewrite the Installation & Setup section of the README to match the updated plugin logic (no renaming required, platform‑specific binary names).
Also, we must provide two clear methods: a simple drag‑and‑drop for inexperienced users, and a lightweight method that avoids downloading the whole zip.

Below is the updated section. Replace the existing ## Installation & Setup in your README.md with this:

🛠️ Installation & Setup
📦 Method 1 – Simple (for most users)
Download the plugin
Go to the Releases page and grab the latest red-signer.zip.

Download the cryptographic engine
From the same page, download the binary that matches your operating system:

OS	File name
Windows	signer-windows-x64.exe
macOS (Intel)	signer-macos-x64
macOS (Apple Silicon)	signer-macos-arm64
Linux	signer-linux-x64
Extract & place

Unzip red-signer.zip – you’ll get a folder named red-signer.

Move the downloaded binary (e.g., signer-linux-x64) inside that red-signer folder.

Now move the whole red-signer folder into your Obsidian vault’s plugin directory:
YourVault/.obsidian/plugins/


Make it executable (macOS/Linux)
open a terminal window and run:

```bash
chmod +x /path/to/your/vault/.obsidian/plugins/red-signer/signer-*
```

- Enable the plugin
- Restart Obsidian (or reload community plugins), then go to Settings → Community plugins and turn on Red Signer.

- That’s it. You’re ready to sign notes.

---

🧩 Method 2 – Lightweight (for advanced users)
If you prefer to fetch only the essential files and avoid the zip:

Create the plugin folder

bash
mkdir -p YourVault/.obsidian/plugins/red-signer
Download the two required files

manifest.json

main.js

Place both inside the red-signer folder.

Download the correct signer binary from the Releases page and place it inside the same folder (no renaming required).

Make it executable (macOS/Linux only):
chmod +x YourVault/.obsidian/plugins/red-signer/signer-*

Reload Obsidian and enable the plugin.

---

## 🚀 The Genesis Workflow (First Use)

1. Open any Markdown guide in your vault.
2. Click the **Signature Icon** (✍️) in the left ribbon.
3. A modal will display your newly generated **Public Key**. Copy this and add it to your Project R.E.D. node's `TrustedMaintainers` map.
4. Click **Sign this note**.  

**What happens under the hood:**
* If you are a new maintainer, the engine generates your private key at `~/.red-network/maintainer.key` (with strict `0600` permissions).  
* It creates or locates the `manifest.json` in your vault root.  
* The guide is cryptographically hashed, signed, and instantly injected into the network ledger.

Once signed, the bottom status bar will glow **`Signed`**. If you modify a single character in that file, the hash changes, and the status will immediately revert to **`Unsigned`** until you authorize the changes.

---

## 🛡️ Security Architecture

* **The Private Key:** Your `maintainer.key` is stored at `~/.red-network/maintainer.key` (owner read/write only). **Never upload this to GitHub or share it.**
* **The Verification:** The Obsidian plugin only signs. Verification happens strictly on the Project R.E.D. Go server side, ensuring no compromised files are ever rendered to the end-user.

---

## 💻 Building from Source (For Core Contributors)

If you want to audit or modify the plugin's TypeScript architecture:

```bash
git clone [https://github.com/StandardCodebase/obsidian-red-signer](https://github.com/StandardCodebase/obsidian-red-signer)
cd obsidian-red-signer
npm install
npm run build   # or `npx tsc`
