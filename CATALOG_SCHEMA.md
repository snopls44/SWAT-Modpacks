# SWAT Modpacks Catalog

The launcher reads `catalog.json` from the default branch.

- `schemaVersion`: currently `1`.
- `catalogType`: must be `modpacks`.
- `items`: published modpack entries.

Each entry contains a stable `id`, `title`, `author`, `description`, `version`, `tags`, compatible `gameBuildGuids`, HTTPS `downloadUrl` to a ZIP containing `modpack.json`, optional `repositoryUrl`, ISO 8601 `updatedAt`, `stars`, and `archived`.

The catalog entry ID and version must match the root manifest ID and version. The catalog ID identifies the collection, not its member mods. Each member retains its own stable ID from `mod.json`.

See [MODPACK_MANIFEST.md](MODPACK_MANIFEST.md) for the manifest and member resolution rules.