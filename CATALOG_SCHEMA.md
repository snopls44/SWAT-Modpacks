# SWAT Modpacks Catalog

The launcher reads `catalog.json` from the default branch.

## Catalog format

- `schemaVersion`: currently `1`.
- `catalogType`: must be `modpacks`.
- `items`: published modpack entries.

Modpack entries use a stable `id`, title, author, description, version, tags, compatible game build GUIDs, a HTTPS `downloadUrl`, optional `repositoryUrl`, `updatedAt`, `stars`, and `archived` state.

## Identity and duplicates

A modpack is a collection distributed as its own installable package/instance. Its stable ID identifies the pack, not the individual mods inside it. Individual mod IDs remain separate and must not be replaced by the modpack ID.

The launcher must compare a modpack ID against installed modpacks, and compare each contained mod's ID against the local mod library when resolving the pack. Reusing an ID for a different modpack is not allowed.

## Example

```json
{
  "schemaVersion": 1,
  "catalogType": "modpacks",
  "items": []
}
```
