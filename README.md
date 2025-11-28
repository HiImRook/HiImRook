# Lightweight Systems, Decentralization, Human Engineering, Mass Psychopathy, and the Way Forward

Hi, I'm Rook. I build small, auditable systems that intently run on low-spec hardware. My work favors clarity, reproducibility, sovereignty, and ergonomics. "Can it run the same in Africa as would in New York or Los Angeles?" is a minimum standard.

Below are the engineering tradeoffs I deliberately make, the projects I'm actively developing (so you know what to expect when cloning), and a bit on the ethos that powers it all.

Why I build this way
- In‑memory procssing: keep the canonical runtime state in memory during development and testing to make behavior explicit, deterministic, and easy to reason about. Snapshot or persist as a clear, pluggable step when needed.
- Auditability, minimal magic: small dependency graphs, explicit serialization, and clear invariants so you can inspect the whole system without a PhD. No more gatekeeping.
- Fast feedback loops: spawn multi‑node tests locally, iterate on consensus rules, and reproduce edge cases with deterministic clocks and seeds.
- Because it's practical: I'm not purist, I'm a worker. I’ll trade theoretical elegance for a working, debuggable implementation when it moves a project forward. Stop supporting fantasy and support real action.
- Because greed kills innovation and affects application. Autonomy, sovereignty, low-to-zero overhead is baked into the code. 

Design principles (Valid)
- Deterministic behavior: canonical serialization and deterministic hashing come first. Consensus breaks fast when nodes disagree on byte layouts.
- Simple concurrency: start with clear ownership (Tokio tasks, Arc<Mutex<...>> where correctness matters), then only optimize with sharding, RwLock, or actor patterns when contention proves problematic.
- Minimal, vetted deps: prefer well‑maintained crates (Tokio, Serde, ed25519-dalek, axum/tower) and avoid plumbing frameworks that hide important behavior.
- Observable and testable: make it straightforward to snapshot state, replay events, and run deterministic integration tests that spawn multiple nodes in one process. 

## Quick start (copy/paste)
```bash
git clone https://github.com/HiImRook/accessible-pos-chain.git
cd accessible-pos-chain
cp config.example.toml config.toml   # edit genesis & validators
cargo build --release
./target/release/pos-chain
```

Technical snapshot (typical patterns I use)
- State model: small in‑memory ChainState (current_slot, tip_hash, accounts: HashMap<Address, u64>, recent blocks buffer). Persistent storage is a snapshot/plug‑in (sled/RocksDB) when needed.
- Mempool: HashMap<TxId, Transaction> + queue for eviction; basic dedup + size caps to keep blocks sane.
- Concurrency: Tokio runtime; network, mempool, and consensus each run in separate tasks. Shared state via Arc<Mutex<...>> early; move to finer-grained locks or actor single-writer when benchmarking shows bottlenecks.
- Consensus testing: deterministic clocks, fixed RNG seeds, and in‑process multi‑node tests to exercise forks and sync behavior.
- Wire format: length‑prefixed binary messages for P2P; JSON‑RPC for admin/wallet UX (simple to integrate with static front-ends).
- Crypto: ed25519 for signing today; plan upgrade path to VRF and Blake3 hashing for blocks and Merkle roots. Current focus.

What I ship (current priorities)
- **The Valid Network**(accessible PoS chain) — compact async Rust node: 10s slots, stake‑weighted leader selection, gossip P2P, mempool, JSON‑RPC. Readme and quickstart in the repo.
- **Local Vault Password Manager** - Local, secure, portable password manager(alpha release)
- **Valid Browser wallet** (prototype) — UX‑focused (initially) JS app for experimenting with seed flows and RPC integration (prototype only; not for real funds).
- **K.E.V.I.N.** — fully autonomous, self-improving AI agent.
- **Valid Browser** - Brave browser hard fork, web3 portal
- Discord bots / small tools — I theorize and isolate it here before the code finds it's home. Used as in-memory experiments, style/UX examples, and for automating smaller tasks. These are mostly experiments for larger use cases. 

Projects (quick links)
- **Node:** https://github.com/HiImRook/accessible-pos-chain
- **Local Vault Password Manager (alpha)**: https://github.com/HiImRook/local-vault-password-manager
- **Wallet (prototype):** https://github.com/HiImRook/Valid-Blockchain-Wallet
- **Bots & tools:** https://github.com/HiImRook (see repositories)

How to try something out quickly
- Clone the node, edit config.example.toml to set genesis/validators, and run it locally to watch slots and gossip across two machines. The node is small enough to read through start to finish if you want to understand how it operates.

If you want to help
- Improve concurrency without losing clarity (benchmarks + deterministic tests)
- Harden crypto paths and canonical serialization
- Add lightweight persistence options (snapshot + restore) without hiding state transitions(you'll probably want to contact me with this)
- Strictly no code bloat
- Join the Discord! Participation of any kind is highly valued.

Notes on trust & usage
- Everything here is MIT unless otherwise noted. Most code is intentionally small so it’s reviewable.
- The wallet prototype is UX‑focused and not secure for production use. Do not store real keys or funds there yet.
- If you run a node and it syncs for you, open an issue

Rook is here
- GitHub: https://github.com/HiImRook
- X/Twitter: https://x.com/WarTimeRook
- Discord: https://discord.gg/2SP383cJs9
