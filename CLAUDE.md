# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Related Paths

- **This repo** (`~/repos/TheBoglers`) — packwiz pack source, managed with git
- **Production server** (`~/repos/mc-server/bogler`) — SSHFS-mounted; pulls from `main` branch on startup via packwiz-installer
- **Test server** (`~/repos/mc-server/testbogler`) — SSHFS-mounted; pulls from `testing` branch on startup via packwiz-installer

### Server startup flow

Both servers run `run.sh` which:
1. Runs `packwiz-installer-bootstrap.jar` — pulls the latest pack from GitHub (`main` or `testing` branch) and syncs mods
2. Launches NeoForge with `user_jvm_args.txt` (both servers: `-Xms20G -Xmx20G -XX:+UseZGC -XX:+ZGenerational`)

This means **pushing to `main` or `testing` on GitHub is what deploys mod changes to the servers** — no manual file copying needed.

### Server directory layout (both servers share this structure)

- `mods/` — installed mod JARs (managed by packwiz-installer, do not edit manually)
- `config/` — mod config files (78 entries; safe to edit directly)
- `server.properties` — server settings (port 25565, hard difficulty, survival, whitelist off)
- `world/` — world data and datapacks

## Overview

This is a [packwiz](https://packwiz.infra.link/) modpack repository for **The Boglers** — a Minecraft 1.21.1 NeoForge modpack centered around Cobblemon (Pokémon in Minecraft) with Create, tech, and exploration mods.

- **Minecraft**: 1.21.1
- **Mod loader**: NeoForge 21.1.221
- **Pack format**: packwiz 1.1.0

## Packwiz CLI Commands

```bash
# Add a mod from Modrinth
packwiz modrinth add <slug-or-url>

# Add a mod from CurseForge
packwiz curseforge add <slug-or-url>

# Update all mods
packwiz update --all

# Update a specific mod
packwiz update <mod-name>

# Refresh index.toml hashes after manual edits
packwiz refresh

# Export to a .mrpack (Modrinth modpack format)
packwiz modrinth export

# Install pack locally (for testing)
packwiz install
```

## Repository Structure

- `pack.toml` — pack metadata (name, MC version, loader version)
- `index.toml` — auto-generated manifest of all files with SHA256 hashes; do not edit manually
- `mods/*.pw.toml` — one file per mod; each references Modrinth/CurseForge download URLs and hashes
- `resourcepacks/*.pw.toml` — resource packs managed by packwiz
- `world/datapacks/*.pw.toml` — datapacks bundled with the default world
- `packwizignore.txt` — files excluded from pack exports (currently excludes `README.md`)

## How Mod Files Work

Each `.pw.toml` file follows this structure:

```toml
name = "Mod Name"
filename = "mod-file.jar"
side = "both"  # or "client" or "server"

[download]
url = "https://cdn.modrinth.com/..."
hash-format = "sha512"
hash = "..."

[update]
[update.modrinth]
mod-id = "..."
version = "..."
```

After adding or editing any `.pw.toml` file manually, always run `packwiz refresh` to regenerate `index.toml`.

## Key Mods / Theme

The pack is Cobblemon-focused with a heavy integration layer:
- **Cobblemon** + many sidemods (capture XP, mega evolution, move inspector, PokeNav, spawn notifications, TMs/TRs, MissingMons, etc.)
- **Create** + Create Deco, CreateAddition, Slice & Dice (for automation + Farmers Delight integration)
- **AE2** (Applied Energistics 2) + **Immersive Engineering** for tech progression
- **YUNG's** structure suite for improved world generation
- **Sodium** + Iris + performance mods (Lithium, FerriteCore, ImmediatelyFast, EntityCulling) for client optimization
- **EMI** as the recipe viewer with enchanting, ore, and Cobblemon recipe addons
