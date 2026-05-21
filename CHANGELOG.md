# Changelog — TBS-Server

All notable changes to the TBS-Server modpack.

## [1.0.2] — 2026-05-20

### Changed
- **StreamCraft Live** — switched from the Modrinth `0.8.8` reference to a bundled local
  build, `streamcraft-0.8.22+mc26.1.2.jar`, carried as a loose override in `mods/` and
  extracted by mrpack4server on deploy. 0.8.22 is a work-in-progress build not yet on
  Modrinth; re-pin both packs to the Modrinth version once it is published. TBS-Client
  bundles the identical jar, keeping StreamCraft matched across both packs.

27 entries (26 packwiz metadata + the bundled StreamCraft jar).

## [1.0.1] — 2026-05-20

Adds **Simple Voice Chat** — proximity voice chat. 27 metadata entries (was 26).

### Tier S7 — Cross-side
- **Simple Voice Chat** `2.6.17` *(Modrinth)* — proximity voice chat. Shipped in both packs
  at the same jar version as TBS-Client; optional per player, so a vanilla client without
  it still connects and plays. Needs a UDP port open on the Bloom.host server.

### Notes
- Simple Voice Chat is the second cross-side mod after StreamCraft Live — intentionally
  relaxing the prior "StreamCraft-only" cross-side rule. Keep its jar version identical
  across both packs.
- Modrinth-sourced: CurseForge's `cf install` returns the Bukkit plugin variant for this
  project, and the Fabric file id could not be pinned reliably.

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
- Essential Permissions (Vanilla Permissions) *(Modrinth)*
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
- Essential Permissions is sourced from Modrinth — its CurseForge file blocks third-party
  API distribution, which aborts `packwiz modrinth export`. Same jar
  (`vanilla-permissions-0.3.6+26.1.2.jar`), so no functional difference; the server pack is
  deployed via mrpack4server, not the CurseForge App, so CF sourcing is not required here.
