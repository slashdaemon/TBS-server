# Changelog — TBS-Server

All notable changes to the TBS-Server modpack.

## [1.0.0] — 2026-05-20

Initial pack. Minecraft **26.1.2**, Fabric loader **0.19.2**, packwiz format `packwiz:1.1.0`.

26 metadata entries (mods + auto-resolved dependencies). Source = CurseForge unless marked
`(Modrinth)`; Modrinth is used only where no 26.1.2 CurseForge build exists.

### Tier S1 — Performance core
- Fabric API
- Lithium
- Krypton
- FerriteCore *(Modrinth)*
- C2ME (Concurrent Chunk Management Engine)
- Chunky *(Modrinth)*

### Tier S2 — Observability & operations
- Spark
- Connectivity
- Ledger

### Tier S3 — Permissions & admin
- LuckPerms
- Essential Permissions
- WorldEdit

### Tier S4 — Communication
- Text Placeholder API
- Styled Chat

### Tier S5 — Gameplay augmentation (vanilla-packet-only)
- Better Server Sleep *(Modrinth — substitute for the doc's "Better Sleep")*
- Lootr *(Modrinth)*
- FallingTree
- Saplanting
- Universal Bone Meal
- Trade Cycling

### Tier S6 — Discoverability
- BlueMap

### Tier S7 — Cross-side
- StreamCraft Live `0.8.8+mc26.1.2` *(Modrinth)*

### Auto-resolved dependencies
- Cupboard, Fabric Language Kotlin, Forge Config API Port, Puzzles Lib

### Not yet included — no 26.1.2 build available
- **ModernFix** — load-time / memory fixes (S1)
- **Noisium** — worldgen chunk perf (S1)
- **Advanced Backups** — differential world backups (S2)
- **AutoWhitelist** — Discord-role-linked whitelist sync (S2)
- **Gamemode Unrestrictor** — flexible `/gm` (S3)
- **Fabricord** — two-way Discord ↔ in-game chat bridge (S4)
- **NoExpensive** — removes the anvil "Too Expensive" cap (S5)

### Open items
- `luckperms-placeholders` (S3) not found on CurseForge or Modrinth under that name.
  LuckPerms prefix/group data already reaches chat via Styled Chat + Text Placeholder API.
- "Better Sleep" from the doc resolved to **Better Server Sleep** — verify behavior.
