# SWAT Modpack Manifest (modpack.json)

The modpack manifest describes a collection of mods that SWAT Launcher can resolve from the **SWAT-Mods** catalog. It does not contain the game and does not replace individual mod manifests.

## Identity rules

- `id` uniquely identifies this modpack and remains stable across releases.
- Every item in `mods[]` references an individual mod by its exact stable mod ID.
- Never use the modpack ID as a member mod ID.
- Pin each member to an explicit version. This makes the collection reproducible and prevents silently changing its contents when a mod author publishes an update.

## Fields

| Field | Required | Meaning |
|---|---:|---|
| `schemaVersion` | Yes | Manifest format version; currently 1. |
| `id` | Yes | Stable modpack identity. |
| `title` | Yes | Display title. |
| `author.name` | Yes | Author/maintainer display name. |
| `description` | Yes | Purpose of the collection. |
| `version` | Yes | Modpack release version. |
| `game.id` | Yes | Must be `swat-game`. |
| `game.buildGuids` | No | Confirmed compatible game build GUIDs. Empty means compatibility is unspecified, not universal. |
| `loader` | No | Loader requirement for the collection. |
| `mods` | Yes | At least one member mod. |
| `mods[].id` | Yes | Exact ID from the member mod's `mod.json`. |
| `mods[].version` | Yes | Exact version requested by this pack. |
| `mods[].required` | No | Defaults to true. If false, the member is optional. |
| `mods[].enabledByDefault` | No | Defaults to true. Whether the mod should be enabled in the created instance. |
| `tags` | No | Search labels. |
| `notes` | No | Player-facing setup notes. |

## Package layout

The modpack ZIP contains the manifest at its root:

```text
ExamplePack.zip
└── modpack.json
```

The ZIP does not bundle copies of member mods. The launcher resolves each `mods[].id` and exact version against the SWAT-Mods catalog, then downloads the individual mod package from that catalog. This keeps each mod's own identity, version history and update path intact.

## Duplicate handling

When resolving a pack against an instance:
- If the same mod ID and requested version are already installed, reuse it; do not install a second copy.
- If the same ID exists at a different version, report a version conflict/update decision. Never install a duplicate under another ID.
- If a required member is missing from the catalog or its exact version is unavailable, stop and explain which member cannot be resolved.
- If an optional member is unavailable, let the user continue without it.
- Do not silently replace a newer installed version with an older pinned version.

The launcher must validate every resolved mod package's internal `mod.json` ID and version against the manifest/catalog before applying it.

## Compatibility

Only list build GUIDs that have been tested. The launcher should compare the selected Vanilla installation's build GUID with the modpack's compatibility list and clearly warn or block according to its compatibility policy. An empty list is unknown compatibility.

## Publishing

1. Create the `modpack.json` using [templates/modpack.json](templates/modpack.json).
2. Verify every member ID and pinned version exists in **SWAT-Mods**.
3. Put `modpack.json` at the root of a ZIP.
4. Host the ZIP at a stable HTTPS URL.
5. Add/update the matching entry in the SWAT-Modpacks `catalog.json`.
6. Keep the catalog entry ID and version identical to the manifest.
