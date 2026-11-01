# SWAT Launcher: Modpack catalog tutorial

This guide explains how to describe and publish a modpack in the SWAT community catalog. It does **not** explain how to create the individual mods inside the pack. A separate mod-development guide may be published in the future.

## Important concepts

- A **Modded instance** is an independent working copy of an already installed Vanilla game. It is not a game installer.
- A **modpack** is a curated collection of mods intended to be used together.
- A modpack has its own stable ID. That ID identifies the collection, not any individual mod inside it.
- Every individual mod keeps its own `mod.json` ID. Do not replace those IDs with the modpack ID.

## Current launcher support

The SWAT-Modpacks catalog and its metadata template are prepared, but end-to-end modpack installation and instance creation from a modpack are not implemented yet. Publishing an entry makes it part of the catalog; it does not make the launcher able to install it. Do not advertise a pack as one-click installable until that workflow is released.

## 1. Prepare the collection

Before publishing, document:
- The purpose and intended audience of the collection.
- The mod IDs and versions included.
- Compatible SWAT Game build GUIDs, when confirmed.
- Required loaders and dependencies.
- Any known conflicts or installation notes.

The modpack should refer to individual mods by their stable IDs. Each included mod remains independently identifiable and updateable.

## 2. Fill in the catalog entry

Start from [templates/catalog-entry.json](templates/catalog-entry.json).

Use:
- A unique, stable modpack `id`, for example `com.author.examplepack`.
- A clear `title`, `author` and `description`.
- A semantic `version` for the collection.
- Searchable `tags`.
- Confirmed compatible `gameBuildGuids`.
- An HTTPS `downloadUrl` for the pack artifact.
- An optional `repositoryUrl`.
- An ISO 8601 `updatedAt` timestamp.
- `archived: false` while the entry is actively maintained.

The current catalog entry format does not yet define an embedded machine-readable list of member mods. Do not invent a custom field and assume the launcher will process it.

## 3. Publish in SWAT-Modpacks

1. Host the package at a stable HTTPS URL.
2. Add the entry to `catalog.json`.
3. Ensure no other entry uses the same modpack ID.
4. Commit the change to the `main` branch.
5. Keep the catalog entry current when the collection changes.

## 4. Update a modpack

Keep its modpack ID stable when releasing a new version. Update the version, package URL, compatibility information and timestamp. Changing the collection's contents does not turn it into a new modpack identity.

## Avoiding duplicate mods

A modpack ID is not a substitute for the IDs of the mods it contains. If a user already has one of the included mods, the future modpack installer must match that member by its individual mod ID and avoid installing a duplicate. It should also distinguish an already installed matching version from a version that can be updated.

## Future documentation

A later tutorial can cover building the modpack archive, declaring its member mods in a supported manifest, resolving dependencies, and installing it as a Modded instance. Those features require a defined launcher format and implementation first.
