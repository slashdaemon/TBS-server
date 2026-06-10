# Changelog — TBS-Server

All notable changes to the TBS-Server modpack.

## [1.2.0] — 2026-06-09

**StreamCraft Live 0.9.9 → 0.12.1.** Lockstep with TheBlockSurvival 1.2.0. Repins the
cross-side StreamCraft Live mod to **0.12.1+mc26.1.2** (Modrinth `DEgY8AQ8`; the server uses
the standard/Windows jar — it loads no capture natives).

- StreamCraft highlights since 0.9.9: in-world voice chat, native Linux capture (Fedora GNOME
  + Arch KDE), Display Block auto-crop / pixel-precise placement / opaque backdrops, macOS
  capture-stall fixes, and a first-run Terms of Use & age gate. See the TheBlockSurvival
  changelog for the full list.
- No other mod changed. **Requires a redeploy** (the server pack changed).

## [1.1.13] — 2026-05-31

**Removed Simple Voice Chat** (`2.6.17`). Proximity voice is consolidating into
**StreamCraft Live**.

- Removed `mods/simple-voice-chat.pw.toml`. Frees the Simple Voice Chat UDP port on the
  Bloom.host server.
- **StreamCraft Live is once again the only cross-side (`both`) mod.**
- **Requires a redeploy** (the server pack changed). Lockstep release with TheBlockSurvival
  1.1.13.

## [1.1.12] — 2026-05-31

**No server changes** — version aligned with TheBlockSurvival 1.1.12 (lockstep),
which adds Xaero's Minimap + World Map (client-side) so the Open Parties and Claims
map overlay works. Nothing server-side: Xaero's client maps need no server companion,
and OPAC already runs here.

## [1.1.11] — 2026-05-31

**No server mod changes** — version aligned with TheBlockSurvival 1.1.11 (lockstep),
which adds the Open Parties and Claims *client* + a semicolon keybind default. The
server has shipped OPAC since 1.1.10; this entry records the runtime config tuning
applied to the live server this release cycle (the file is world/runtime config, not
pack metadata, so it isn't part of the `.mrpack`):

- **`permissionSystem = "luck_perms"`** (was `prometheus`) — claim/forceload limits
  are now overridable per-rank via LuckPerms meta (`xaero.pac_max_claims`,
  `xaero.pac_max_forceloads`, etc.). Verified in the boot log:
  `Configured OPAC to use the following permission system: luck_perms`.
- **`maxPlayerClaimForceloads = 0`** (was `10`) — forceloading disabled. Claimed
  chunks no longer stay ticking with no player nearby; protects the Bloom.host tick
  budget. Grant specific ranks forceloads later via LuckPerms if wanted.
- **`maxPlayerClaims = 500`** — left at default (claims don't tick; generous is fine).
- Protection exception groups (Controls / Doors / Traders / Livestock) left at OPAC
  defaults (protected — not loosened).

Config path on this host: `/config/openpartiesandclaims-server.toml` (edit with the
server stopped, or it's overwritten on shutdown).

## [1.1.10] — 2026-05-31

**Added — Tier S5 (Gameplay augmentation, vanilla-packet-only):**
- **Open Parties and Claims (OPAC)** `0.26.3` *(CurseForge project `636608`,
  file `8091537`)*, native **26.1.2** build (`open-parties-and-claims-fabric-26.1.2-0.26.3.jar`),
  `side = "both"`. Chunk claims + player parties + chunk forceloading.

  **Why it's here — anti-theft.** The concern on TBS is players *stealing from
  each other's containers*, not block griefing. In a claimed chunk, non-members
  cannot open chests/barrels/shulkers/hoppers/furnaces; the chunk owner grants
  access by adding players to their **party** or defining another party as an
  **ally** with specific access toggles. Container protection is on by default.

  **Vanilla-client contract — path (1), no new client-visible content.** OPAC
  registers no new blocks/items/entities/packets a vanilla client must understand;
  it re-permissions how vanilla container/block interaction behaves, enforced
  **server-side**. The in-game claim UI and Xaero map overlays are purely
  client-side render features a stock client simply never sees. A vanilla 26.1.2
  joiner (no mod) is fully protected *and* fully able to use the system via chat
  commands — `/openpac-claims …`, `/openpac-parties …`, `/opm` (party chat),
  `/openpac …` (player config) — with the mod's server-side text localization.

  **Dependencies — both already in the pack.** Forge Config API Port
  (`ForgeConfigAPIPort-v26.1.4-mc26.1.x-Fabric`) and Fabric API. No new mods added.

  **Scope — server-only for now.** Chat commands cover every player. The OPAC
  *client* mod (claim UI + Xaero minimap/world-map overlay) is a possible future
  TBS-Client fast-follow; it is intentionally not shipped in v1.1.10.

  **Config is world-scoped, tuned post-deploy.** OPAC's main config lives at
  `world/serverconfig/openpartiesandclaims-server.toml` (generated on first run),
  so it can't be baked into the packwiz pack. Defaults are theft-safe out of the
  box; forceload should be capped/disabled on the live server (tick-budget) and
  per-player claim limits set there. Tune via the Bloom.host panel / SFTP while
  the server is stopped.

## [1.1.9] — 2026-05-30

**Added — Tier S5 (Gameplay augmentation, vanilla-packet-only):**
- **Sit Anywhere!** `2.3.4` *(Modrinth `gNPj0UDg`)*, MC range `1.21–26.1.2`,
  `side = "both"`. Right-click stairs/slabs to sit; sit on arbitrary surfaces by
  looking straight down, double-tapping sneak, then right-clicking (or `/trigger sit`).

  **Why Modrinth, not CurseForge (CurseForge-first policy exception):** the
  CurseForge "Sit AnyWhere [Data Pack]" (author *Nico4play*) is a **different,
  stale project** — it tops out at MC 1.20.1 (Aug 2023) and uses a crafted-saddle
  mechanic. `packwiz cf install sit-anywhere` matched that namesake and failed with
  "mod not available for the configured Minecraft version(s)." The actively-maintained
  "Sit Anywhere!" supporting 26.1.2 exists **only on Modrinth**, so this is a
  legitimate Modrinth-only fallback (a name collision, not a missing-file fix).

  **Vanilla-client contract — path (1), no new client-visible content.** Sit
  Anywhere! is datapack logic underneath (shipped as a Fabric wrapper jar so it
  drops into `mods/` and auto-applies to every world). A datapack cannot register
  new entity/block/item types — the seat is an invisible **vanilla** entity the
  player mounts, which stock clients already render and ride. No Polymer layer
  needed; a vanilla 26.1.2 joiner sits down and everyone sees it correctly. No
  dependencies.

  **Verified in-world on production 1.1.9** — right-click stairs/slabs, look-down
  + double-sneak + right-click, and `/trigger sit` all seat and dismount cleanly.

  **Expected harmless log noise:** on boot the server logs, three times during
  pack discovery, `Error reading pack metadata, attempting fallback type … Pack
  version declaration mismatch between supported_formats (from 48) and min_format
  (71.0)`. This is Sit Anywhere's own `pack.mcmeta` (`supported_formats [48, 81]`
  vs `min_format 71` — internally inconsistent). MC loads it via fallback and its
  `v71-1.21.5` overlay supplies the 26.1.2-era functions, so the datapack works
  correctly. Cosmetic only — not a load failure. Do not chase it.

## [1.1.8] — 2026-05-30

**Added:**
- **Just Enough Items (JEI)** `26.1.2-fabric-29.6.2.31` (CurseForge, `side = "both"`).
  Pinned to the identical CurseForge file the TBS-Client pack already ships
  (project `238222`, file `8108850`), so client and server run matching JEI builds.

  Why a recipe viewer on the server: since **Minecraft 1.21.2**, recipes live on
  the server and are no longer sent to the client wholesale. JEI on the client
  alone therefore shows "JEI is missing recipes — please install JEI on the
  server to sync recipes to the client." Installing JEI server-side restores the
  recipe sync. JEI is fully dedicated-server-safe on 1.21.2+ (this is the mod
  author's documented, recommended setup), so it does not break the
  vanilla-client contract — a stock vanilla client still connects and plays.

Synchronized 1.1.8 release with TBS-Client (lockstep policy).

## [1.1.7] — 2026-05-28

**Version-coupling policy change.** TBS-Client and TBS-Server were previously
versioned independently. From this release forward, both packs ship in lockstep —
every future bump to either pack is a synchronized bump of both. This release
jumps the server from `1.0.6` to `1.1.7` to align with TBS-client's current
`1.1.7`. See parent `TBS/CLAUDE.md` for the updated policy.

**Updated:**
- **StreamCraft Live** `0.8.25+mc26.1.2` → `0.9.9+mc26.1.2`. Synchronized with TBS-client
  v1.1.7. Brings in "the Adi branch" rollup covering everything between v0.8.25 and v0.9.9.

  Server-side relevance:
  - `PROTOCOL_VERSION` unchanged at 5 — v0.9.9 server accepts any v0.8.x or v0.9.x client.
  - Late-joining viewers now reliably receive desktop-audio subscription on connect
    (v0.9.5 `SetSubscribedRequest` per pre-existing publication, server-side path).
  - Screen-share token-handshake path is unchanged; the v0.9.8 race fix is client-side.

  Client-visible benefits (TBS-client players) include the new Windows Media Foundation
  webcam shim, Windows Graphics Capture screen-share, Display Block crafting recipe,
  Display Block source-selector fix, and a new in-mod Support + Backers screen.
  See TBS-client `CHANGELOG.md` v1.1.7 for the full list.

No other mod added, removed, or updated.

## [1.0.6] — 2026-05-22

Adds passive chunk pre-generation. Voxy WorldGen runs server-side and
generates chunks ahead of where players are travelling, so the server stops
stuttering when someone walks toward unexplored terrain.

### Added — Tier S1 (Performance core)
- **Voxy WorldGen** `Voxy World Gen V2-26.1.2-2.2.4.jar` *(Modrinth `xT0lnNE9`)*
  — passive chunk pre-generator. `side = "both"` in the pack manifest and
  `environment = "*"` in `fabric.mod.json`, but JAR inspection confirms it
  registers no blocks / items / entities / block-entity types — so the
  Lootr-style registry-sync break (v1.0.5) cannot recur here. Its networking
  surface is Fabric opt-in custom payloads (`HandshakePayload`, `LODDataPayload`)
  on the `voxyworldgenv2` channel; vanilla clients never subscribe so the
  server never sends to them.

### Notes — integration runs in dormant mode
- Voxy WorldGen's headline feature is feeding LOD data to client-side **Voxy**
  for far-distance rendering, which requires Voxy on the server side too.
  Voxy hard-depends on Sodium 0.8.9–0.8.11, and Sodium's
  `environment = "client"` blocks dedicated-server load — so the integration
  chain is mechanically unreachable on stock packwiz/Fabric. We ship
  voxy-worldgen alone; boot log confirms `voxy not present, integration
  disabled`. The mod still provides passive pre-gen value. TBS-Client's
  existing Voxy renders visited far chunks client-side, unchanged.

### Validation
- LocalServer boot test (TBS-server staged into LocalServer via
  `mrpack4server.jar` + JDK 25): reached `Done (0.656s)! For help, type "help"`,
  voxy-worldgen initialized, zero `voxyworldgenv2:`-namespaced registry
  entries observed in boot log.

## [1.0.5] — 2026-05-22

Two hotfixes:

1. **Restores vanilla-client compatibility.** v1.0.4 (and earlier) shipped Lootr
   `1.22.36.109` in Tier S5 under the "vanilla-packet-only" classification from
   `TBS-mod-strategy.md`. That classification turned out to be wrong for this
   Fabric port: Lootr registers ~27 custom registry entries (block-entity types
   for personalised loot chests, container types, etc.) and declares them as
   `side = "both"`. During the login handshake the server transmits its registry
   and a vanilla / TBS-Client connection rejects with **"Received 27 registry
   entries that are unknown to this client. … namespaces may be related: lootr"**.
   Lootr cannot be downgraded to `side = "server"` either — the mod requires
   client-side registration. The only safe fix is removal.
2. **Re-syncs StreamCraft Live with TBS-Client.** Replaces the bundled WIP
   `streamcraft-0.8.22+mc26.1.2.jar` loose override with the published Modrinth
   metafile pinning **0.8.25**, matching TBS-Client v1.1.4. Same canonical
   no-suffix jar (Modrinth version `uZsk67f0`, file
   `streamcraft-0.8.25+mc26.1.2.jar`); on Bloom.host the no-suffix jar is what
   gets deployed.

### Removed
- **Lootr** `1.22.36.109` *(Modrinth `EltpO5cN`)* — broke the vanilla-client
  contract (see above). Personal-loot gameplay feature is lost; a Polymer-based
  per-player-loot alternative would be the right re-add path if one ships for
  26.1.2. `TBS-mod-strategy.md`'s Tier S5 entry for Lootr is now stale.

### Changed
- **StreamCraft Live** — `streamcraft-0.8.22+mc26.1.2.jar` (bundled loose jar)
  → `streamcraft-live.pw.toml` referencing Modrinth `0.8.25`. The pack source
  tree now contains zero loose jars; every entry is packwiz metadata. Matches
  the pattern TBS-Client adopted in v1.1.2.

### Deploy notes
- **Re-import / re-deploy required.** Players who were on v1.0.4 cannot connect
  until the server upgrades to v1.0.5; the registry mismatch is on the wire.
- Any existing world chunks that already contain Lootr block entities will
  read back as missing block-entity types after Lootr is removed. The vanilla
  Minecraft server treats unknown block-entity types as no-ops and the chest
  block underneath stays vanilla — no chunk corruption — but any dungeon chest
  that Lootr had personalised is now a normal first-come-first-served chest.

42 packwiz metadata entries (was 44 in v1.0.4 = 43 metafiles + 1 loose
StreamCraft jar; −1 Lootr, −1 loose jar, +1 StreamCraft metafile).

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
