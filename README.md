<div align="center">

# Yoko-Fantasy

**A systems-heavy fantasy expansion for Brotato**

New characters, jobs, weapons, items, creatures, combat behaviors, progression systems, maps, UI integrations, and localized content — implemented as a Brotato Mod Loader extension rather than a standalone game project.

[![GDScript](https://img.shields.io/badge/GDScript-Godot-478CBF?style=flat-square&logo=godot-engine&logoColor=white)](https://godotengine.org/)
[![Mod Loader](https://img.shields.io/badge/Mod%20Loader-6.0.0-5965FF?style=flat-square)](#compatibility)
[![License](https://img.shields.io/github/license/CYoJkoY/Yoko-Fantasy?style=flat-square)](LICENSE)
[![Workflow](https://img.shields.io/github/actions/workflow/status/CYoJkoY/Yoko-Fantasy/release.yml?style=flat-square&label=release)](.github/workflows/release.yml)

[Overview](#overview) · [Systems](#systems) · [Architecture](#architecture) · [Installation](#installation) · [Development](#development) · [Compatibility](#compatibility)

</div>

---

## Overview

Yoko-Fantasy is a **Brotato Mod Loader** project written in GDScript. Its core purpose is to add a connected set of fantasy-themed gameplay systems and content while extending the base game's existing runtime instead of replacing it.

The project is built around three layers:

- **Content resources** define characters, jobs, weapons, items, entities, enemies, effects, maps, zones, and other game data.
- **Script extensions** patch specific Brotato systems such as player state, run data, weapons, enemies, spawning, shops, waves, entities, and menus.
- **NewContent integration** registers the mod's resources through `Yoko-NewContentLoader` and keeps the content layer separate from the runtime extensions.

The result is a mod architecture where a mechanic can be represented by the combination of a content resource, one or more reusable effects, and a narrowly scoped extension to the base game system that owns the behavior.

## Systems

Yoko-Fantasy is not just a collection of isolated items. Its current source tree connects several gameplay systems across the run lifecycle.

| System | What it covers |
| :--- | :--- |
| **Jobs** | Job resources, job registration, run integration, menu flow, and end-of-run handling |
| **Soul** | Soul statistics, soul consumables, drop logic, and player/run integration |
| **Holy** | Holy-related statistics, item interactions, and enemy behavior |
| **Erosion** | Erosion-focused items and associated effects |
| **Limited Items** | Restricted item pools and progression bonuses tied to limited items |
| **Combat Effects** | Kill progression, reload triggers, weapon switching, lightning chains, hit procs, damage clamping, reflection, and critical-damage interactions |
| **Enemies** | Custom enemies, cursed behavior, targeting logic, special spawning, healing, and world interactions |
| **Companions & Entities** | Pets, turrets, gardens, wandering bots, special entities, and their custom behaviors |
| **Shop & Waves** | Shop curses, reroll interactions, tier-specific upgrades, synthesis, extra enemies, and extra elites |
| **World Content** | Fantasy zones, backgrounds, title-screen resources, special world entities, and map-specific behavior |
| **UI & Localization** | Menu extensions, cooperative focus behavior, localized entity descriptions, and translation resources |

### Representative content

The resource graph includes concrete content such as Holy Battleaxe variants, Blazing Path variants, Prism Tower, Healing Star, Soul Link, Crow, fantasy pets, plant-themed enemies, and custom enemy attack behaviors. These are registered through the same content-loading layer rather than being hard-coded into a single gameplay script.

The repository also contains dedicated attack behaviors for patterns such as lasers, leaves, maples, slow fields, surrounding targets, waving attacks, and improved shooting behavior.

## Architecture

The important architectural boundary is between **data registration** and **base-game behavior**.

```text
                         Brotato
                            │
                            ▼
                    Brotato Mod Loader
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
       mod_main.gd                 FantasyNewContent.gd
             │                             │
             │                    Yoko-NewContentLoader
             │                             │
             │                 ┌───────────┴───────────┐
             │                 │                       │
             ▼                 ▼                       ▼
     Script Extensions    Content Resources       Translations
             │                 │                       │
     ┌───────┼────────┐        │              localized strings
     │       │        │        │
     ▼       ▼        ▼        ▼
   Player  Combat   World   Characters / Jobs
   / Run   / Shop   / UI   Weapons / Items / Entities
```

### `mod_main.gd`

`mod_main.gd` is the runtime integration point. It installs targeted extensions for systems including:

- player and player-run data
- run data and linked stats
- melee and ranged weapons
- enemy and neutral entities
- entity spawning and entity services
- wave management
- shop behavior
- turrets, gardens, and wandering bots
- music management
- gameplay and menu UI
- lootworm behavior

The extension list is deliberately explicit. Each file modifies the game system that owns a particular behavior instead of introducing a second parallel game loop.

### `FantasyNewContent.gd`

`FantasyNewContent.gd` extends `Yoko-NewContentLoader`'s content integration. It registers the mod's job resources and supplies the custom consumable-selection logic used for Souls.

Soul drops are calculated from the project's configured base chance, player luck, Holy statistics, and the mod's additional Soul-drop statistic. The implementation also caps the luck multiplier, keeping the mechanic bounded in code rather than relying on documentation alone.

### Resource-driven content

The main content manifests are:

- `NewContentData.tres` — primary content registration.
- `NewContentDataDLC1.tres` — additional content registration.
- `FantasyNewContent.gd` — custom registration and content hooks.

This allows the content itself to remain inspectable as Godot resources while the extension layer handles interactions with existing Brotato systems.

## Installation

Yoko-Fantasy is intended to run through **Brotato Mod Loader**.

### Requirements

- Brotato
- Brotato Mod Loader **6.0.0**
- [`Yoko-NewContentLoader`](https://github.com/CYoJkoY/Yoko-NewContentLoader)
- [`Yoko-MoreStatsContainer`](https://github.com/CYoJkoY/Yoko-MoreStatsContainer)

These are the dependencies currently declared by `manifest.json`. The manifest does not currently declare a compatible Brotato game-version list, so game-version compatibility should be verified against the installed mod stack.

### Release installation

1. Install Brotato and a compatible Brotato Mod Loader.
2. Install `Yoko-NewContentLoader` and `Yoko-MoreStatsContainer`.
3. Download the Yoko-Fantasy release ZIP.
4. Place the ZIP in the Mod Loader `mods` directory according to your Mod Loader installation.
5. Start Brotato and confirm that the mod and its dependencies are loaded.

The repository's release workflow builds a Mod Loader package under `mods-unpacked/Yoko-Fantasy/`, generates Godot import data when imported assets require it, validates the resulting ZIP, and publishes the package for tagged releases.

### Build from source

```bash
git clone https://github.com/CYoJkoY/Yoko-Fantasy.git
cd Yoko-Fantasy
```

For development, use the same Brotato Mod Loader development layout used by the project. This repository is a **mod source tree**, not a standalone Godot game project, so a normal root `project.godot` is intentionally absent.

## Development

The project uses **Godot 3.x-era GDScript and Godot resource files** together with Brotato Mod Loader's script-extension mechanism.

When adding a mechanic, keep the implementation split according to responsibility:

```text
New mechanic
   │
   ├── Content data / Resource
   │       └── content/...
   │
   ├── Reusable effect / behavior
   │       └── content/... or extensions/...
   │
   ├── Base-game integration
   │       └── extensions/<system>.gd
   │
   └── Player-facing text
           └── translations/...
```

A practical development sequence is:

1. Identify the Brotato system that owns the behavior.
2. Add the content resource or reusable behavior under `content/`.
3. Extend the corresponding base-game script under `extensions/` only when integration is required.
4. Register the extension in `mod_main.gd`.
5. Register the content through the NewContent resource pipeline.
6. Add or update localization resources.
7. Test with the declared Mod Loader and dependency versions.

Keep extensions focused. If a change belongs to `weapon_service.gd`, `wave_manager.gd`, or `base_shop.gd`, avoid moving that responsibility into an unrelated global script.

## Project Structure

```text
Yoko-Fantasy/
├── .github/
│   └── workflows/
│       └── release.yml              # Release packaging and validation
├── assets/                          # Repository-level visual assets
├── content/
│   ├── attack_behaviors/            # Custom attack logic
│   ├── characters/                  # Character resources and effects
│   ├── entities/                    # Enemies, pets, turrets, and entities
│   ├── items/                       # Item resources
│   ├── jobs/                        # Job definitions
│   ├── maps/                        # Background and map resources
│   ├── specials/                    # Special gameplay entities/behaviors
│   ├── weapons/                     # Melee and ranged weapons
│   ├── zones/                       # Fantasy zone data
│   └── ...                          # Additional content resources
├── extensions/                      # Brotato base-game extensions
├── translations/                    # Localization resources
├── FantasyNewContent.gd             # Custom NewContent integration
├── NewContentData.tres              # Main content registration
├── NewContentDataDLC1.tres          # Additional content registration
├── manifest.json                    # Mod metadata and dependencies
├── mod_main.gd                      # Mod Loader entry point
└── LICENSE
```

The exact content tree is intentionally broader than the summary above; `content/` is the data-heavy part of the project, while `extensions/` contains the integration layer.

## Release Pipeline

Releases are automated through `.github/workflows/release.yml`.

The workflow is designed around the actual Mod Loader packaging requirements rather than simply zipping the Git repository:

```text
Git tag vX.Y.Z
      │
      ▼
Validate version tag
      │
      ▼
Read manifest metadata
      │
      ▼
Create temporary Godot project
      │
      ▼
Import assets with Godot
      │
      ▼
Stage mods-unpacked/Yoko-Fantasy/
      │
      ├── include generated .import data when required
      ├── exclude repository-only CI metadata
      └── update manifest version for the release
      │
      ▼
Build + verify ZIP
      │
      ▼
Upload artifact + GitHub Release
```

The workflow validates the `vX.Y.Z` tag format, checks the manifest, imports resources with Godot 3.7-dev1, verifies the ZIP structure, checks the packaged manifest version, and publishes the resulting release artifact.

This distinction matters for this project because imported Godot assets are part of the runtime packaging process even though the generated import directory is not committed to the repository.

## Compatibility

The current `manifest.json` declares the following metadata:

| Component | Declared value |
| :--- | :--- |
| Mod name | `Fantasy` |
| Namespace | `Yoko` |
| Repository | `Yoko-Fantasy` |
| Mod version | `0.0.1` |
| Mod Loader | `6.0.0` |
| Required dependency | `Yoko-NewContentLoader` |
| Required dependency | `Yoko-MoreStatsContainer` |
| Compatible Brotato versions | Not specified |
| Authors | `CYoJkoY`, `CaveGood` |

The manifest is the source of truth for the declared dependency and Mod Loader requirements. Do not interpret an empty `compatible_game_version` array as a claim of universal Brotato compatibility.

## Contributing

Bug reports, gameplay feedback, balance observations, content contributions, and code changes are useful.

When reporting a problem, include:

- Brotato version
- Mod Loader version
- Yoko-Fantasy version
- versions of both required dependencies
- reproduction steps
- relevant logs or screenshots when available

For code changes, preserve the project's separation between content resources and base-game extensions. New behavior should be attached to the narrowest appropriate extension point rather than added as a monolithic system.

## Project Members

The current manifest credits:

- **CYoJkoY**
- **CaveGood**

## License

Yoko-Fantasy is distributed under the **MIT License**. See [`LICENSE`](LICENSE) for the complete text.

## Support the Author

If this project saves you time or improves your Brotato modding workflow, consider supporting its continued development.

<div align="center">
  <a href="https://cyojkoy.github.io/Payment/">
    <img src="https://img.shields.io/badge/Support_the_Author-9E8F7E?style=for-the-badge&logo=buy-me-a-coffee&logoColor=BEB8AE" alt="Support the Author">
  </a>
</div>

---

<div align="center">

**Yoko-Fantasy** · Fantasy gameplay systems and content for Brotato

</div>
