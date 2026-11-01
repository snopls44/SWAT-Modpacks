# SWAT Launcher: Modpack catalog tutorial

This guide covers how to describe and publish a modpack in the SWAT community catalog. It does **not** teach how to program or create the mods inside the collection. A separate mod-development tutorial may be added in the future.

## How the launcher is organized

- **Vanilla installation:** the existing game installation.
- **Modded instance:** an independent copy of an already installed Vanilla game. It is not a game installer.
- **Modpack:** a curated collection of individual mods intended to be used together.
- A modpack has its own stable ID. Each mod inside it keeps its own independent `mod.json` ID.

A modpack is intended to create/configure a Modded instance from mods. It must not download or install the SWAT Game itself.

## Current support status

The SWAT-Modpacks catalog and catalog-entry template are ready. End-to-end modpack package installation and instance creation from a modpack are not implemented yet. Publishing an entry makes it discoverable in the catalog; it does not make the launcher able to install the pack. Do not advertise one-click installation until that workflow exists.

## 1. Describe the collection

Before publishing, prepare:
- The collection's purpose and intended setup.
- The individual mod IDs and versions included.
- Confirmed compatible SWAT Game build GUIDs.
- Required loaders and actual dependencies.
- Known conflicts and installation notes.

A modpack must reference individual mods by their stable IDs. Never replace member mod IDs with the modpack ID. Modded Queue is an optional community integration, not a universal dependency.

## 2. Fill in the catalog entry

Start from [templates/catalog-entry.json](templates/catalog-entry.json).

Fields:
- `id`: unique, stable modpack ID, e.g. `com.author.examplepack`.
- `title`, `author`, `description`: display metadata.
- `version`: version of the collection.
- `tags`: searchable labels.
- `gameBuildGuids`: confirmed compatible game build GUIDs.
- `downloadUrl`: HTTPS URL for the pack archive.
- `repositoryUrl`: optional source/project page.
- `updatedAt`: ISO 8601 timestamp.
- `stars`: catalog popularity value, if used.
- `archived`: set to true when the entry should no longer be installed as a new pack.

The current catalog schema does not yet define a machine-readable member-mod manifest or dependency resolver. Do not add custom fields and assume the launcher will process them.

## 3. Publish in SWAT-Modpacks

1. Host the package at a stable HTTPS URL.
2. Add its entry to the root `catalog.json`.
3. Ensure no other entry uses the same modpack ID.
4. Commit the change to the repository's `main` branch.
5. Keep the catalog entry current as the collection changes.

## 4. Update a modpack

Keep its modpack ID stable when publishing a new collection version. Update the version, package URL, compatibility information and timestamp. A changed member list does not automatically mean it is a new modpack identity.

## Duplicate prevention

The modpack ID identifies only the collection. It must never be used as the ID for the individual mods it contains.

The future modpack installer must compare each member's individual mod ID with the local library and selected instance. If a member mod is already present, it should not install a second copy. If the installed version differs, it should identify an update candidate for that same mod.

## Future tutorial

A later guide can explain the supported modpack archive format, machine-readable member list, dependency resolution, and installation into a Modded instance. Those formats and workflows need to be implemented first; this page intentionally does not invent them.
