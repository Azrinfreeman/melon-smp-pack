# Validation notes

## Documentation review

- Parsed the tracked pack manifest, index, and 57 indexed mod metadata files as TOML.
- Verified all 62 indexed paths exist and their SHA-256 hashes match the committed file bytes.
- Verified the manifest's index hash matches the committed index.
- Counted 22 client-only and 35 both-side metadata entries; no duplicate output filenames among the metadata files.
- Verified relative documentation links resolve.

Checks use Git blob bytes rather than Windows checkout bytes to avoid newline-conversion differences. They verify local metadata integrity, not remote download availability, mod authenticity, or runtime compatibility.

## Remaining checks

- Confirm Minecraft, Fabric, and Java compatibility in a clean launcher instance.
- Exercise mod downloads and verify downloaded artifacts against each metadata hash.
- Confirm client launch, server connection, voice chat, combat, maps, and world loading.
- Test a server-side installation separately; side labels alone do not prove server compatibility.
- Review both directly bundled JARs alongside their related metadata to confirm intended versions and avoid overlapping mod installations.
- Confirm that bundled server-list data is intended for distribution.
- Confirm the actual pack hosting URL, deployment configuration, and update behaviour.
- Review individual mod licences and redistribution terms for directly bundled files.

No Minecraft client/server, mod JAR, installer, packwiz refresh, external download, or live service was run. The mod list, hashes, server-list data, and hosting files were preserved. Documentation is not indexed into the current pack and does not change its install payload; maintainers should review what a future refresh includes.
