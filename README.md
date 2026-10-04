# Local-first security and systems engineering

Independent software engineer. I build local-first, open-source systems in Rust and JavaScript: a Layer 1 blockchain, an encrypted password manager, and an experimental radar gesture control system. Each one keeps data on the user's own device and works without a third-party server.

Background as a Radioman and in IT and network support for hospital and campus environments, including cryptographic key handling, transmission security, and protection of sensitive records on shared infrastructure.

## Valid Blockchain

A Layer 1 blockchain written from scratch in Rust, with an original consensus mechanism, Three-Party Integrity (TPI). Three randomly selected validators produce each block, and validator standing is earned through block production. Chain state is held in memory, the node ships as a single binary with vendored dependencies, and peer transport runs over TLS 1.3 with hashed, epoch-salted peer identity. In testnet development at v0.8.0.

[Repository](https://github.com/HiImRook/accessible-tpi-chain) · [Whitepaper](https://github.com/HiImRook/accessible-tpi-chain/blob/main/docs/whitepaper.md)

## Valid Vault

A password manager for Chromium browsers and Android. The vault is encrypted on the device with AES-GCM under a single master key, wrapped separately for fingerprint, device PIN, and master password, with no stored password hash and no server. Vaults move between devices over a local QR stream or an encrypted backup file, and browsers on the same computer can share one encrypted vault file. Stores logins, crypto wallet seed phrases, and bookmarks. At v0.7.3.

[Repository](https://github.com/HiImRook/valid-vault-password-manager)

## Valid Symphony

Experimental work in short-range radar sensing as a human interface. A 60 GHz mmWave radar produces a 3D point cloud, which Symphony reduces to a tracked hand position inside a defined control volume. A clutch model separates deliberate input from incidental motion: control engages only when the hand holds still in the volume and releases when it leaves, so a person walking through the field produces no output. The tracked position passes through an adaptive low-pass filter and maps to continuous control, with lighting as the first output.

The tracking core is written in `no_std` Rust so the same code runs on a host computer during development and on the radar chip's embedded processor in later hardware. The radar measures range and motion only, and all processing stays on the local network. Currently in prototype on Texas Instruments 60 GHz radar hardware.

[Repository](https://github.com/HiImRook/valid-symphony)

## Engineering Approach

- User data stays on user hardware.
- Dependencies are minimal and vendored, and builds ship as a single binary or a single file where possible.
- State lives in memory, and nothing is persisted that doesn't need to be.
- Every release is documented with a changelog and roadmap.

## Contact

- Email: [byrook@proton.me](mailto:byrook@proton.me)
- Discord: [Valid community](https://discord.gg/2SP383cJs9)
- Medium: [@HiImRook](https://medium.com/@HiImRook)
