# Changelog — TBS-Server

All notable changes to the TBS-Server modpack.

## [1.0.4] — 2026-05-22

Adds **Tier S8 — Worldgen content**: 16 server-only worldgen mods carried over
from the Better Adventures+ `1.7.mrpack` reference pack. Every entry is
`side = "server"`; a vanilla 26.1.2 client still connects and plays. Pack goes
from 27 → 44 metadata entries (16 worldgen + 1 new auto-resolved dep
`cloth-config`).

> **Worldgen seam warning.** Existing worlds will show visible biome/structure
> seams where pre-1.0.4 chunks meet newly generated chunks. Either pair this
> release with a fresh world or pre-run `/chunky` to a clean boundary before
> deploy.

### Tier S8 — Worldgen content (all `side = "server"`)

**Dimension overhauls**
- **Nullscape** `1.2.19` *(Modrinth)* — End rewrite
- **Incendium** `5.4.12` *(Modrinth)* — Nether rewrite
- **Amplified Nether** `1.2.14` *(Modrinth)* — vertical Nether
- **Dungeons Dimensions: Nether** `1.3.2` *(CurseForge)* — MC-Dungeons-style
  Nether content (file: `mcd_d_nether-fabric-26.1.2-1.3.2.jar`)

**Terrain / biome**
- **Geophilic** `3.5` *(Modrinth)* — vanilla-biome terrain overhaul (mod variant,
  not the datapack — `packwiz cf install geophilic` resolves to the datapack
  `.zip` and must be installed via `packwiz mr install geophilic` instead)

**Structures**
- **Explorify** `1.6.5` *(Modrinth)*
- **Dungeons and Taverns** `5.2.0` *(Modrinth)*
- **Structory** `1.3.16` *(Modrinth)*
- **Structory: Towers** `1.0.16` *(Modrinth)*
- **Towns and Towers** `1.13.11` *(CurseForge)*
- **MES — Moog's End Structures** `2.0.3` *(CurseForge)*
- **MVS — Moog's Voyager Structures** `5.0.11` *(CurseForge)*
- **Katters Structures** `2.4` *(Modrinth)*

**Tuning**
- **Sparse Structures** `3.1.2` *(Modrinth)* — structure-spacing knobs so the
  added structure density stays balanced

**Required libs**
- **Cristel Lib** `3.1.4` *(Modrinth)* — required by Structory family
- **Moog's Structure Lib** `2.0.1` *(CurseForge)* — required by MES / MVS
  (also auto-resolved as a dependency of MES)

### Auto-resolved dependencies (new)
- **Cloth Config API** `26.1.154` *(Modrinth)* — pulled in by Towns and Towers
  and Cristel Lib

### Sourcing notes
- Stardust Labs / Redupro family (Nullscape, Incendium, Amplified Nether,
  Geophilic, Structory, Structory: Towers, Explorify, Dungeons and Taverns,
  Katters Structures, Sparse Structures, Cristel Lib) has **no 26.1.2 CurseForge
  build** even though CurseForge pages exist — `packwiz cf install` returns
  "mod not available for the configured Minecraft version" and these were
  installed via Modrinth. CurseForge sourcing should be retried on the next
  pack bump in case 26.1.2 builds are uploaded later.
- CurseForge-sourced entries (Dungeons Dimensions: Nether, Towns and Towers,
  MES, MVS, Moog's Structure Lib) follow the pack's CF-first convention.

### Not yet included — no 26.1.2 build available (new)
- **Biome Replacer** — Fabric build tops out at 1.21.11
- **Take Us Pillage** — original is Forge-only (max 1.20.1); the Fabric
  continuation also stops at 1.21.11

Existing pending entries from v1.0.0 are unchanged
([`docs/MOD_LIST.md` → Pending](docs/MOD_LIST.md)).

44 metadata entries (43 packwiz metadata + the bundled StreamCraft jar).

## [1.0.3] — 2026-05-22

Documentation-only release. No mod additions, removals, or version bumps — the
pack contents are byte-identical to v1.0.2.

### Added
- **`docs/MOD_LIST.md`** — full packwiz-entry reference: every mod grouped by
  tier (S1–S7) with side, source (CurseForge / Modrinth), project-page link,
  and filename. Mirrors the doc added to TBS-Client and makes the pack's
  vanilla-client contract auditable at a glance.

### Changed
- **`README.md`** — replaced the `./packwiz.exe` command examples with the
  cross-platform `packwiz` invocation (matches the bare-binary convention
  now that maintainers can install packwiz natively on macOS / Linux via
  `brew install go && go install github.com/packwiz/packwiz@latest`).
  Added an "Installing packwiz" callout documenting the macOS / Linux install
  path and the still-checked-in `packwiz.exe` for Windows.

27 packwiz metadata entries (unchanged from v1.0.2).

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
