# CDDA.nix

Nix packaging for Cataclysm: Dark Days Ahead that lives outside the game repo. It builds the game and composes all community tilesets from source, driving the project's own Makefile and compose.py instead of reimplementing either. Upstream doesn't seem to want to take Nix packaging in-tree, and they also seem to have an aversion LLMs, as they do not accept contributions from people who use LLMs at all, even when used in minor cases, so it lives here (see CleverRaven/Cataclysm-DDA#87320 and CleverRaven/Cataclysm-DDA#89051)

```sh
nix run github:twstagg/CDDA.nix#tiles-full   # SDL + sound, every tileset baked in
nix build github:twstagg/CDDA.nix#tileset-retrodays
```

The game itself comes from a flake input (`github:CleverRaven/Cataclysm-DDA`), so `nix flake update` moves it forward like any other input. Tileset sources stay pinned in `flake.nix`, with `nix run .#update-tilesets` to bump them. See `nix/README.md` for the full tour: packages, checks, dev shell, pins.

Layout: `flake.nix` (packages, checks, shell, updater apps), `nix/package.nix` (game builds), `nix/tileset.nix` (set composer), `nix/soundpack.nix` (pack isolator), `nix/update-tilesets.sh` plus `nix/test-update-tilesets.sh` and `nix/update-soundpacks.sh` plus `nix/test-update-soundpacks.sh` (pin tooling and its tests). `gameEra` in `flake.nix` picks stable (SDL2, in-tree art) or master (SDL3, composed art); sound (CC-Sounds) ships on both.
