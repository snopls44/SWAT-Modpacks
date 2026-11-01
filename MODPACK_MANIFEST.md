# SWAT Modpack Manifest (modpack.json)

A modpack manifest describes a collection of mods resolved through the SWAT-Mods catalog. It does not contain or install SWAT Game.

## Identity
- `id` is the stable identity of the collection and must match the SWAT-Modpacks catalog entry.
- Every item in `mods[]` uses the individual mod's stable ID from its `mod.json`.
- Pin each member to an explicit version so the pack is reproducible.

## Fields
- `schemaVersion`: required, currently 1.
- `id`, `title`, `version`: required pack identity and release metadata.
- `author.name`, `description`: required display metadata.
- `game.id`: required, must be `swat-game`.
- `game.buildGuids`: confirmed compatible game build GUIDs; empty means unknown, not universal.
- `loader`: optional loader requirement.
- `mods`: required list of member mods.
- `mods[].id`: exact ID from the member's `mod.json`.
- `mods[].version`: exact required version.
- `mods[].required`: defaults to true.
- `mods[].enabledByDefault`: defaults to true.
- `tags`, `notes`: optional search tags and player-facing notes.

## Package layout
The ZIP contains `modpack.json` at its root. It does not bundle member mod ZIPs. The launcher resolves each member ID and pinned version against SWAT-Mods and downloads each mod's own package.

## Resolution and duplicate rules
- Same member ID and version already installed in the selected instance: reuse it; do not duplicate.
- Same ID at a different version: report a conflict; never silently downgrade or overwrite a newer version.
- Missing required member/version: stop before changing the instance and identify the missing item.
- Missing optional member: allow the player to continue without it.
- Validate the downloaded package's internal `mod.json` ID and version before applying it.
- Compare IDs, never display names or ZIP filenames.

## Compatibility
Only list tested game build GUIDs. Empty means compatibility is unspecified. Check the selected Vanilla installation's build GUID before configuring a Modded instance.

## Publishing
1. Fill `templates/modpack.json`.
2. Verify every member ID and pinned version exists in SWAT-Mods.
3. ZIP the root `modpack.json`.
4. Host the ZIP at a stable HTTPS URL.
5. Add a catalog entry with the same modpack ID and version.