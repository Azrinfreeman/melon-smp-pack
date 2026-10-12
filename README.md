# Melon SMP

A Fabric Minecraft modpack maintained with packwiz. The repository stores versioned mod download metadata and integrity hashes alongside a small number of directly bundled files, making the pack's composition reviewable in Git.

## Recorded versions

| Component | Manifest value |
| --- | --- |
| Pack | Melon SMP 1.0.0 |
| Minecraft | 26.1.2 |
| Fabric Loader | 0.19.5 |
| Pack format | packwiz:1.1.0 |

These values come from [pack.toml](pack.toml). They describe this revision; compatibility and launch success have not been tested in this documentation review.

## Pack contents

The [file index](index.toml) contains 62 entries, including 57 mod metadata files: 22 marked client-only and 35 marked for both sides. It also includes two directly bundled JARs, `servers.dat`, and the hosting files.

Examples of declared mods include [Better Combat](mods/better-combat.pw.toml), [Archers](mods/archers.pw.toml), [Simple Voice Chat](mods/simple-voice-chat.pw.toml), [Sodium](mods/sodium.pw.toml), and [Xaero's World Map](mods/xaeros-world-map.pw.toml). Their metadata records filenames, download hashes, and update identifiers. Inclusion does not establish that every combination has been playtested.

## Install and maintain

1. Use a separate launcher instance configured for the Minecraft and Fabric versions above, with a compatible Java runtime.
2. Follow the official [packwiz-installer guide](https://packwiz.infra.link/tutorials/installing/packwiz-installer/) to connect that instance to the owner's hosted `pack.toml` URL. A hosted URL and working installer bundle have not been verified here.
3. Let the installer retrieve the indexed files, then check the launch log before joining a server. Back up existing worlds before changing a pack installation.
4. For server deployments, follow the guide's server-side installation instructions; client-only mods should be filtered by side metadata.

For maintainers, use the official [packwiz workflow](https://packwiz.infra.link/tutorials/creating/getting-started/) to manage mods and refresh the index after pack changes. Keep the hash in `pack.toml` synchronized with `index.toml`. Review generated changes before publishing an update.

## Repository layout

- `pack.toml`: pack identity, loader versions, and index hash.
- `index.toml`: indexed file paths and integrity hashes.
- `mods/*.pw.toml`: mod download and update metadata.
- `index.html` and `.nojekyll`: minimal hosting content; their presence alone does not confirm an active deployment.
- `servers.dat`: bundled server-list data; confirm intended distribution before updates.

See [validation notes](docs/VALIDATION.md) for the checks performed and remaining release work.

## Attribution

Mods belong to their respective authors and retain their individual licences. Metadata references third-party downloads; the directly bundled JARs need their own redistribution-rights review. This README does not grant rights over Minecraft or included mods.
