# nix-cursor

A Nix flake that packages [Cursor](https://cursor.com) — the AI-powered code editor — for NixOS and nix-darwin. Includes an auto-update script to keep the package in sync with the latest upstream release.

## Supported Platforms

- `x86_64-linux`
- `aarch64-linux`

## Usage

### Run without installing

```bash
nix run github:vleeuwenmenno/nix-cursor
```

### Install via `nix profile`

```bash
nix profile install github:vleeuwenmenno/nix-cursor
```

### NixOS / Home Manager (flake)

Add the flake as an input:

```nix
inputs.nix-cursor.url = "github:vleeuwenmenno/nix-cursor";
```

Then include the package in your configuration:

```nix
environment.systemPackages = [
  inputs.nix-cursor.packages.${system}.cursor
];
```

Or via the overlay:

```nix
nixpkgs.overlays = [ inputs.nix-cursor.overlays.default ];
environment.systemPackages = [ pkgs.cursor ];
```

## Updating

The `scripts/update-version.sh` script checks the Cursor API for a new release, downloads the AppImage, computes the Nix SRI hash, and patches `package.nix` automatically.

```bash
# Run from the repository root
bash scripts/update-version.sh
```

**Requirements:** `curl`, `jq`, `nix-prefetch-url`, `nix-hash`

A development shell with all dependencies is provided:

```bash
nix develop
```

## Development

```bash
# Build the package locally
nix build

# Open a shell with the update-script dependencies available
nix develop
```

## License

Cursor itself is proprietary software — see the [Cursor license](https://cursor.com). The Nix packaging in this repository is released under the [MIT License](LICENSE).
