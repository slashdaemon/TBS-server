# TBS-Server

**The Block Survival — Server modpack**

Fabric modpack for **Minecraft 26.2**, built with [packwiz](https://packwiz.infra.link/).
Deployed to the game server via [mrpack4server](https://github.com/Patbox/mrpack4server).

TBS-Server makes the server fast, observable, well-administered, and gameplay-rich **while
staying joinable by any stock vanilla 26.2 client**. Players need no mods; the server sends one
small resource pack for its terrain slabs, which they accept on join.

## The vanilla-client contract

The base server is vanilla 26.2. Every server mod must either:

1. **Add no new client-visible content.** It changes how vanilla mechanics or world generation
   behave (sleep voting, tree-felling, trade restocking, new biomes and structures built from
   vanilla blocks) without adding new blocks, items, entities or packets.
2. **Translate new content into vanilla packets with Polymer.** Since 2.0.0 three mods do this:
   **SlashSlabs** (terrain slabs), **SlashRails** (the Track Smoother) and **StreamCraft Live**
   (the Display Block, with `allow_vanilla_clients=true` in `config/streamcraft/config.properties`).
   Polymer builds one server resource pack for all of them; `config/polymer/auto-host.json`
   serves it and marks it required.

So the contract is: **a server resource pack is required; no client mod is.** Players who
decline the pack are disconnected.

Two mods are shared with TBS-Client and shipped at the same version on both sides:
**StreamCraft Live** and **SlashRails**. Both are optional per player: a vanilla client without
them still connects and plays (no stream video; smoothed rails ride smoothly but look like vanilla
rails). See `docs/TBS-mod-strategy.md` for the full design.

### Bedrock crossplay

**Bedrock Edition clients can join** through **Geyser** (Bedrock↔Java protocol bridge) and
**Floodgate** (Bedrock players join the online-mode server without a Java account). Both are
`side = "server"` mods; Java clients are unaffected. A Bedrock player is treated like a vanilla
Java joiner: full gameplay, no StreamCraft voice. The host must expose a Bedrock UDP port: UDP
`19132` is open on Bloom.host (see `config/Geyser-Fabric/config.yml`).

Geyser 2.11.x supports Java 26.2, which is why the 2.0.0 move to 26.2 brought Bedrock back
(Geyser's last 26.1.2 build topped out at Bedrock 26.33, below what Bedrock clients run).

Bedrock doesn't read Java resource packs, so `config/Geyser-Fabric/` ships custom mappings and a
Bedrock pack generated with Rainbow: Bedrock players see the terrain slabs and the Track Smoother
instead of the vanilla states Polymer borrows. **Regenerate them whenever the Polymer mod set
changes**; see `docs/BEDROCK-MAPPINGS.md`. StreamCraft's Display Block still shows as a sculk
sensor on Bedrock (it draws through display entities, which Geyser doesn't support).

## Deploy

One-click deploy from the TBS project root via `server-config.py`:

```bash
python ../server-config.py deploy        # export -> SFTP upload -> restart Bloom.host
python ../server-config.py               # interactive menu (deploy, power, backups)
```

It exports this pack, uploads the `.mrpack` to Bloom.host as `local.mrpack`, removes any
remote `modpack-info.json` so mrpack4server uses the local file, uploads the staged secret files
(SoulCraft's `config/soulcraft/config.json`, from devenv `SoulCraft-TBS`), then restarts the
server. Needs `TBS/.env` (copy from `TBS/.env.example`) and `~/.deploy/tbs` populated by
`devenv pull SoulCraft-TBS`.

Manual equivalent:

```bash
packwiz modrinth export            # produce TBS-Server-X.Y.Z.mrpack
# -> upload as local.mrpack to the host running mrpack4server
```

## Build / maintenance

```bash
packwiz cf install <mod-slug> -y   # add a mod from CurseForge (preferred)
packwiz mr install <mod-slug> -y   # Modrinth fallback (no CF 26.2 build)
packwiz update --all               # update every mod
packwiz refresh                     # rebuild index.toml after manual edits
```

> **Installing packwiz.** Requires the [packwiz](https://packwiz.infra.link/) CLI on `PATH`.
> macOS / Linux: `brew install go && go install github.com/packwiz/packwiz@latest`
> (ensure `~/go/bin` is on `PATH`). Windows: a prebuilt `packwiz.exe` is checked in at the
> repo root — invoke it as `./packwiz.exe …` in place of `packwiz …` below.

CurseForge is always tried first; Modrinth is used only when a mod has no CurseForge build
for 26.2. One jar is bundled directly in `mods/` instead of being pinned from a store: SoulCraft Light
(unpublished, server-only).

## Mod tiers

See `CHANGELOG.md` for the exact resolved state of every mod and `docs/TBS-mod-strategy.md`
for the full design rationale.

- **Tier S1 — Performance core:** Fabric API, Lithium, Krypton, FerriteCore, C2ME, Chunky,
  Voxy WorldGen
- **Tier S2 — Observability & operations:** Spark, Connectivity, Ledger
- **Tier S3 — Permissions & admin:** LuckPerms, Vanilla Permissions, WorldEdit
- **Tier S4 — Communication:** Text Placeholder API, Styled Chat
- **Tier S5 — Gameplay augmentation:** Better Server Sleep, FallingTree, Saplanting,
  Universal Bone Meal, Trade Cycling, Sit Anywhere!, Open Parties and Claims (chunk
  claims/parties, server-enforced)
- **Tier S6 — Discoverability:** BlueMap
- **Tier S7 — Cross-side:** StreamCraft Live, SlashRails
- **Tier S8 — Worldgen (server-only):**
  - Terrain: Tectonic (landforms), Terralith (biomes), Lithostitched (their shared library),
    SlashSlabs (half-slab steps on one-block rises; Polymer)
  - Structures: Repurposed Structures (needs MidnightLib), Tidal Towns, Explorify, Dungeons and
    Taverns, Structory, Structory: Towers, Towns and Towers, Moog's Voyager Structures,
    Moog's End Structures, Katters Structures (libs: Cristel Lib, Moog's Structure Lib)
  - Density: Sparse Structures, set to `spreadFactor` 0.75 (`config/sparsestructures.json5`),
    denser than vanilla spacing
  - Nether and End: Incendium, Amplified Nether, Nullscape
- **Tier S9 — Client recipe sync:** Just Enough Items (JEI). Since MC 1.21.2 recipes are held
  server-side, so JEI runs on the server to sync them to JEI on the client. A stock vanilla
  client is unaffected.
- **Tier S10 — Crossplay:** Geyser, Floodgate
- **Tier S11 — Companion:** SoulCraft Light (Lena), server-only. Only the owner controls her
  (`access.controllers` in `config/soulcraft/tuning.json`); everyone else can talk with her.
- **Libraries:** Cloth Config API, Cupboard, Fabric Language Kotlin, Forge Config API Port,
  Puzzles Lib

## Pending mods

These mods from the strategy doc aren't in the pack. Per the strategy doc, the architecture
absorbs server-side gaps cleanly — they can be added later with no TBS-Client change:

- **Advanced Backups** — differential world backups (S2). Not yet checked for 26.2.
- **AutoWhitelist** — Discord-role-linked whitelist sync (S2). Has a 26.2 build; left out of
  the 2.0.0 reset by decision.
- **Gamemode Unrestrictor** — flexible `/gm` (S3). Not yet checked for 26.2.
- **NoExpensive** — removes the anvil "Too Expensive" cap (S5). Not yet checked for 26.2.
- **Biome Replacer** — worldgen biome remap (S8). On hold: add only if some Terralith biomes turn
  out unwanted.

Dropped from the list in 2.0.0: ModernFix and Noisium (no 26.x builds), Take Us Pillage (never
reached 26.x on Fabric; Towns and Towers covers outposts), Fabricord (the Discord chat relay now
runs in the theblockacademy backend).

`luckperms-placeholders` (S3) was not found on CurseForge or Modrinth under that name —
LuckPerms group/prefix data is exposed to chat through Styled Chat + Text Placeholder API,
which are installed.

## Version coupling

TBS-Client and TBS-Server ship in **lockstep** (since v1.1.7) — every bump to either pack
is a synchronized bump of both, same version number. The mods whose jar version must match
across both packs are **StreamCraft Live** and **SlashRails**.
