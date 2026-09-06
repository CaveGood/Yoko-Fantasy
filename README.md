<div align="center">

# Yoko-Fantasy

**A systems-heavy fantasy expansion for Brotato.**

Custom characters, jobs, weapons, items, enemies, combat effects, progression systems, maps, UI integrations, and localization — built as a Brotato Mod Loader extension.

[![Latest Release](https://img.shields.io/github/v/release/CYoJkoY/Yoko-Fantasy?display_name=tag&sort=semver&style=flat-square)](https://github.com/CYoJkoY/Yoko-Fantasy/releases)
[![Build](https://img.shields.io/github/actions/workflow/status/CYoJkoY/Yoko-Fantasy/release.yml?style=flat-square&label=build)](https://github.com/CYoJkoY/Yoko-Fantasy/actions/workflows/release.yml)
[![Mod Loader](https://img.shields.io/badge/Mod%20Loader-6.3.0-5965FF?style=flat-square)](#compatibility)
[![Godot](https://img.shields.io/badge/Godot-3.x-478CBF?style=flat-square&logo=godot-engine&logoColor=white)](https://godotengine.org/)
[![License](https://img.shields.io/github/license/CYoJkoY/Yoko-Fantasy?style=flat-square)](LICENSE)

[Overview](#overview) · [Systems](#systems) · [Architecture](#architecture) · [Installation](#installation) · [Development](#development)

</div>

---

## Overview

Yoko-Fantasy is a content and systems expansion for Brotato. The project combines resource-driven content with focused script extensions so new mechanics can use Brotato's existing runtime instead of introducing a second gameplay framework.

The architecture is split into three layers:

```text
Content resources
     │
     ├── Characters / Jobs / Weapons / Items / Entities / Maps
     │
     ▼
NewContent registration
     │
     ▼
Targeted script extensions
     │
     ▼
Brotato runtime systems
```

This keeps data inspectable in Godot resources while behavior that needs deeper integration remains isolated in `extensions/`.

## Systems

| System | Scope |
| :--- | :--- |
| **Jobs** | Job resources, registration, run integration, menu flow, and end-of-run handling |
| **Soul** | Soul statistics, consumables, drops, and player/run integration |
| **Holy** | Holy-related statistics, item interactions, and enemy behavior |
| **Erosion** | Erosion-focused items and effects |
| **Limited Items** | Restricted item pools and progression bonuses |
| **Combat** | Kill progression, reload triggers, weapon switching, lightning chains, hit effects, reflection, and critical-damage interactions |
| **Enemies** | Custom enemies, cursed behavior, targeting, spawning, healing, and world interactions |
| **Entities** | Pets, turrets, gardens, wandering bots, and other special entities |
| **Shop & Waves** | Shop interactions, rerolls, synthesis, extra enemies, elites, and wave behavior |
| **World** | Fantasy zones, backgrounds, title-screen resources, and map-specific behavior |
| **UI & Localization** | Menu integration, cooperative focus handling, descriptions, and translations |

Representative content includes Holy and Blazing Path weapon variants, Prism Tower, Healing Star, Soul Link, Crow, fantasy pets, plant-themed enemies, and custom attack behaviors.

## Architecture

### `mod_main.gd`

`mod_main.gd` is the main runtime integration point. It registers targeted extensions for player/run data, weapons, enemies and neutral entities, spawning, waves, shops, turrets, gardens, wandering bots, music, menus, and other systems used by the expansion.

The extension list is explicit so each behavior remains attached to the Brotato system that owns it.

### `FantasyNewContent.gd`

`FantasyNewContent.gd` integrates the project with [Yoko-NewContentLoader](https://github.com/CYoJkoY/Yoko-NewContentLoader). It registers custom job resources and supplies project-specific content hooks such as Soul consumable selection.

### Resource registration

The main content resources are:

- `NewContentData.tres` — primary content registration.
- `NewContentDataDLC1.tres` — additional content registration.
- `FantasyNewContent.gd` — custom registration and content hooks.

## Installation

### Requirements

- Brotato **1.15.4**
- **Brotato Mod Loader 6.3.0**
- [Yoko-NewContentLoader](https://github.com/CYoJkoY/Yoko-NewContentLoader)
- [Yoko-MoreStatsContainer](https://github.com/CYoJkoY/Yoko-MoreStatsContainer)

These are the required dependencies declared by `manifest.json`.

### Release installation

1. Install Brotato 1.15.4 and Mod Loader 6.3.0.
2. Install Yoko-NewContentLoader and Yoko-MoreStatsContainer.
3. Download the latest `Fantasy-*.zip` from [Releases](https://github.com/CYoJkoY/Yoko-Fantasy/releases).
4. Place the ZIP in the Mod Loader `mods` directory.
5. Launch Brotato and verify that the dependency chain loads successfully.

### Development

The repository is a mod source tree rather than a standalone Godot game project. Its release workflow creates an isolated temporary Godot project when generating the import cache.

## Development

When adding a mechanic, keep responsibility separated:

```text
New mechanic
   │
   ├── Content data / Resource
   │      └── content/...
   │
   ├── Reusable behavior
   │      └── content/... or extensions/...
   │
   ├── Base-game integration
   │      └── extensions/<system>.gd
   │
   └── Localization
          └── translations/...
```

A practical workflow is to identify the owning Brotato system first, add the resource data, add only the narrowest required extension, register it in `mod_main.gd`, then update localization and test the complete dependency stack.

## Release pipeline

Releases are driven by semantic version tags. The workflow treats `manifest.json` as authoritative and refuses to package mismatched versions:

```text
manifest.json: 1.1.0
        │
        ├── tag v1.1.0  → build allowed
        └── tag v1.2.0  → build rejected
```

The pipeline also imports Godot resources, checks for import errors, creates the Mod Loader package, preserves generated `.import` data, verifies ZIP structure, and checks the packaged manifest before publishing.

## Compatibility

| Component | Declared target |
| :--- | :--- |
| Engine | Godot 3.x / GDScript |
| Mod Loader | **6.3.0** |
| Mod version | **1.1.0** |
| Dependencies | Yoko-NewContentLoader, Yoko-MoreStatsContainer |
| Brotato game version | **1.15.4** |
| Authors | CYoJkoY, CaveGood |
| License | MIT |

The manifest is the source of truth for declared compatibility.

## Project structure

```text
Yoko-Fantasy/
├── .github/workflows/release.yml
├── content/
│   ├── attack_behaviors/
│   ├── characters/
│   ├── entities/
│   ├── items/
│   ├── jobs/
│   ├── maps/
│   ├── specials/
│   ├── weapons/
│   ├── zones/
│   └── ...
├── extensions/
├── translations/
├── FantasyNewContent.gd
├── NewContentData.tres
├── NewContentDataDLC1.tres
├── manifest.json
├── mod_main.gd
├── README.md
└── LICENSE
```

## Contributing

Bug reports, gameplay feedback, balance observations, content contributions, and code changes are useful.

For a reproducible report, include the Brotato version, Mod Loader version, Yoko-Fantasy version, dependency versions, reproduction steps, and relevant logs or screenshots.

## License

Yoko-Fantasy is distributed under the [MIT License](LICENSE).

## Support the Author

If this project saves you time while playing or developing Brotato mods, consider supporting its continued development.

<div align="center">
  <a href="https://cyojkoy.github.io/Payment/">
    <img src="https://img.shields.io/badge/Support_the_Author-9E8F7E?style=for-the-badge&logo=buy-me-a-coffee&logoColor=BEB8AE" alt="Support the Author">
  </a>
</div>

---

<div align="center">
  <sub>Yoko-Fantasy · Fantasy gameplay systems and content for Brotato</sub>
</div>
