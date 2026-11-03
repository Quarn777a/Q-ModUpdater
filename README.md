Q-ModUpdater is a client-side Minecraft mod that installs and updates a modpack from a GitHub release in `.mrpack` format. It is built for packs distributed outside launchers: the player adds the mod to an instance, and the mod downloads the pack, checks every file, and applies it after the game exits.

The mod never changes the running game's mod list and never touches the launcher. It stages the pack in `.qmodupdater/staging/`, then a small helper process applies the staged files once Minecraft has closed and optionally restarts the game. This avoids locked `.jar` files on Windows and removes the need to unpack archives by hand.

## Features

- Install and update from a GitHub release (`.mrpack`).
- Install `overrides/`, `client-overrides/`, and optional files.
- Automatic update checks on startup, a title-screen button, and the `U` key.
- First-launch installer prompt.
- SHA-512/SHA-1 verification and path traversal protection.
- Backups of `config/`, `defaultconfigs/`, and `options.txt`.
- Removal of files that belonged to the previous pack version.
- Resumable and parallel downloads, plus a release cache.
- Loader version check; NeoForge can download the official installer.
- English and Russian UI.
- Built-in settings screen and `config/qmodupdater-client.toml`.
- 30+ options for pack authors.

## Requirements

- Minecraft: use the build that matches your pack (the current 1.3.0 builds target 1.21.1).
- NeoForge 21.1.0+ or Fabric Loader 0.15+ with Fabric API.
- Java 21.
- A GitHub release with an `.mrpack` asset built for the same Minecraft version and loader.

## Configuration

Set the repository in `config/qmodupdater-client.toml`:

```toml
repo = "owner/repo"
```

Leave `asset` empty to use the first `*.mrpack` asset in the release. The same options are available in the in-game settings screen.

## For pack authors

Q-ModUpdater is designed to be configured once and shipped with the pack. More than 30 settings let you:

- lock the settings screen for players (`lockSettings = true`);
- hide the repository name (`hideRepo = true`);
- show the installer on first launch (`promptOnFirstLaunch = true`);
- control update checks, restarts, backups, optional files, client overrides, hash verification, and parallel downloads;
- choose how loader version mismatches are handled.

Ready-to-ship templates: `examples/qmodupdater-client.toml` for NeoForge and `examples/qmodupdater-client-fabric.toml` for Fabric.

## How files are applied

1. The mod reads the GitHub release and downloads the `.mrpack`.
2. Files are prepared in `.qmodupdater/staging/` inside the instance.
3. After Minecraft exits, a small helper applies the staged files and can restart the game.
4. Only files recorded by the previous installation are removed.

The `.mrpack` index supplies the download URLs, so individual files may come from Modrinth CDN or another host chosen by the pack author.

## Safety and transparency

- Every downloaded file is checked against the hashes in the `.mrpack`.
- Paths are confined to the game instance.
- Pack files and the README are never executed.
- Network access is used only to read GitHub releases, download the files referenced by the pack, and fetch the optional README.
- Update checks contact GitHub when `checkOnStartup` is enabled; they can be disabled in the config.
- No telemetry is collected.
- The GitHub token, if configured, is stored as plain text in the config file; do not publish a config that contains a token.

## Limitations

- Client-side only: nothing is installed on a server.
- Works with GitHub releases, not directly with Modrinth or CurseForge.
- The `.mrpack` must be attached to a release and built for the same Minecraft version and loader.
- The mod does not replace the launcher and does not install Java. For NeoForge it can download the official installer, but the player completes the installation.

## License

MIT.

Q-ModUpdater is not affiliated with Mojang Studios, NeoForge, Fabric, or Modrinth.
