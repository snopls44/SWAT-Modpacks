# SWAT Launcher: Modpack publishing tutorial

This guide covers preparing and publishing a modpack for SWAT Launcher. It does **not** explain how to program or create individual mods. A separate mod-development tutorial may be added in the future.

## Concepts
- **Vanilla installation:** the existing game installation.
- **Modded instance:** an independent copy of an already installed Vanilla game. Creating one does not download or install SWAT Game.
- **Modpack:** a versioned collection of individual mods used to configure a Modded instance.
- A modpack has its own ID; every member keeps its own `mod.json` ID.

A modpack never contains or installs the game itself.

## 1. Prepare the collection
For every member, record its exact stable mod ID, exact version, whether it is required, and whether it should be enabled by default. Confirm game build compatibility and actual loader/dependency requirements. Required member versions must exist in SWAT-Mods. Modded Queue is optional, not a universal dependency.

## 2. Create modpack.json
Start from [templates/modpack.json](templates/modpack.json). The manifest lists member mods by stable ID and pinned version. See [MODPACK_MANIFEST.md](MODPACK_MANIFEST.md) for the format and resolution rules.

## 3. Package
Create a ZIP containing only the root `modpack.json`. Do not include SWAT Game or duplicate member mod ZIPs. The launcher resolves member packages separately through SWAT-Mods.

## 4. Publish
1. Host the ZIP at a stable HTTPS URL.
2. Add an entry to `catalog.json`.
3. Keep the catalog entry `id` and `version` identical to `modpack.json`.
4. Set `downloadUrl` to the direct ZIP URL.
5. List only confirmed compatible game build GUIDs.
6. Update `updatedAt` and commit to `main`.
Use [templates/catalog-entry.json](templates/catalog-entry.json) for the catalog entry.

## 5. Publish an update
Keep the modpack ID stable. Increase its version, update manifest and catalog together, publish a new ZIP, and intentionally update pinned member versions when needed.

## Duplicate and conflict handling
- Same member ID and same version in the selected instance: reuse it.
- Same member ID with a different version: report a conflict and require deliberate resolution.
- Missing required member/version: stop before modifying the instance.
- Missing optional member: allow the player to continue without it.
Never create a duplicate under another ID, silently downgrade a newer mod, or modify the Vanilla source installation.

## Current implementation status
The manifest format and publishing template are defined. End-to-end modpack download, member resolution, version-conflict handling and creation/configuration of a Modded instance from a pack still need implementation and testing. Publishing a pack does not mean those actions are already available.

## Future tutorial
A separate future tutorial may explain how to develop the individual mods. This guide focuses only on packaging and distributing them through the launcher.