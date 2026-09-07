<div align="center">

# Yoko-Fantasy

**A systems-heavy fantasy expansion for Brotato, combining new content with mechanics that hook into existing game systems.**

<p>
  <a href="https://github.com/CYoJkoY/Yoko-Fantasy/releases"><img src="https://img.shields.io/github/v/release/CYoJkoY/Yoko-Fantasy?display_name=tag&sort=semver&style=flat-square&label=release" alt="Latest release"></a>
  <a href="https://github.com/CYoJkoY/Yoko-Fantasy/actions/workflows/release.yml"><img src="https://img.shields.io/github/actions/workflow/status/CYoJkoY/Yoko-Fantasy/release.yml?style=flat-square&label=build" alt="Build status"></a>
  <img src="https://img.shields.io/badge/Brotato-1.15.4-478CBF?style=flat-square" alt="Brotato 1.15.4">
  <img src="https://img.shields.io/badge/Mod%20Loader-6.3.0-5965FF?style=flat-square" alt="Mod Loader 6.3.0">
  <a href="LICENSE"><img src="https://img.shields.io/github/license/CYoJkoY/Yoko-Fantasy?style=flat-square" alt="MIT License"></a>
</p>

<p><a href="#systems">Systems</a> · <a href="#architecture">Architecture</a> · <a href="#installation">Installation</a> · <a href="#development">Development</a> · <a href="#compatibility">Compatibility</a></p>

</div>

## What it is

Yoko-Fantasy is a content and systems expansion for Brotato. It combines resource-driven content with focused script extensions so new mechanics can use Brotato's existing runtime instead of introducing a second gameplay framework.

The project follows a clear boundary:

```text
Content resources
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

This keeps game data inspectable in Godot resources while deeper behavior remains attached to the system that owns it.

## Systems

| System | Scope |
| :--- | :--- |
| Jobs | Job resources, registration, run integration, menu flow, and end-of-run handling |
| Soul | Soul statistics, consumables, drops, and player/run integration |
| Holy | Holy statistics, item interactions, and enemy behavior |
| Erosion | Erosion-focused items and effects |
| Limited Items | Restricted item pools and progression bonuses |
| Combat | Kill progression, reload triggers, weapon switching, lightning chains, hit effects, reflection, and critical-damage interactions |
| Enemies | Custom enemies, cursed behavior, targeting, spawning, healing, and world interactions |
| Entities | Pets, turrets, gardens, wandering bots, and other special entities |
| Shop & Waves | Shop interactions, rerolls, synthesis, extra enemies, elites, and wave behavior |
| World | Fantasy zones, backgrounds, title-screen resources, and map-specific behavior |
| UI & Localization | Menu integration, cooperative focus handling, descriptions, and translations |

Representative content includes Holy and Blazing Path weapon variants, Prism Tower, Healing Star, Soul Link, Crow, fantasy pets, plant-themed enemies, and custom attack behaviors.

## Architecture

### Runtime entry point

`mod_main.gd` is the main integration point. It installs targeted extensions for player/run data, weapons, enemies and neutral entities, spawning, waves, shops, turrets, gardens, wandering bots, music, menus, and other systems used by the expansion.

The extension list is explicit so behavior remains attached to the relevant Brotato subsystem.

### Content integration

`FantasyNewContent.gd` integrates the project with [Yoko-NewContentLoader](https://github.com/CYoJkoY/Yoko-NewContentLoader), registers custom job resources, and provides project-specific content hooks such as Soul consumable selection.

The main content resources are:

| Resource | Role |
| :--- | :--- |
| `NewContentData.tres` | Primary content registration |
| `NewContentDataDLC1.tres` | Additional content registration |
| `FantasyNewContent.gd` | Custom registration and content hooks |

## Installation

### Requirements

- Brotato **1.15.4**
- **Brotato Mod Loader 6.3.0**
- [Yoko-NewContentLoader](https://github.com/CYoJkoY/Yoko-NewContentLoader)
- [Yoko-MoreStatsContainer](https://github.com/CYoJkoY/Yoko-MoreStatsContainer)

These required dependencies are declared by `manifest.json`.

### Release installation

1. Install Brotato 1.15.4 and Mod Loader 6.3.0.
2. Install Yoko-NewContentLoader and Yoko-MoreStatsContainer.
3. Download the latest `Fantasy-*.zip` from [Releases](https://github.com/CYoJkoY/Yoko-Fantasy/releases).
4. Place the ZIP in the Mod Loader `mods` directory.
5. Launch Brotato and verify that the dependency chain loads successfully.

### Development layout

The repository is a Mod Loader source tree rather than a standalone Godot game. The release workflow creates an isolated temporary Godot project when generating import data.

## Development

When adding a mechanic, separate responsibilities:

```text
New mechanic
   ├── Content data / Resource → content/
   ├── Reusable behavior      → content/... or extensions/...
   ├── Base-game integration  → extensions/<system>.gd
   └── Localization           → translations/...
```

A practical workflow is to identify the owning Brotato system first, add the resource data, add only the narrowest required extension, register it in `mod_main.gd`, update localization, and test the complete dependency stack.

## Release pipeline

Releases are driven by semantic version tags. The workflow treats `manifest.json` as authoritative and rejects mismatched versions.

```text
manifest.json: 1.1.0
        │
        ├── tag v1.1.0  → build allowed
        └── tag v1.2.0  → build rejected
```

The pipeline imports Godot resources, checks for import errors, creates the Mod Loader package, preserves generated `.import` data, verifies ZIP structure, and checks the packaged manifest before publishing.

## Compatibility

| Component | Declared target |
| :--- | :--- |
| Game | **Brotato 1.15.4** |
| Engine | Godot 3.x / GDScript |
| Mod Loader | **6.3.0** |
| Mod version | **1.1.0** |
| Dependencies | Yoko-NewContentLoader, Yoko-MoreStatsContainer |
| Authors | CYoJkoY, CaveGood |
| License | MIT |

`manifest.json` is the source of truth for compatibility and dependencies.

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

Useful contributions include concrete bugs, compatibility improvements, gameplay feedback, balance observations, content additions, and focused code changes.

For reproducible reports, include the Brotato version, Mod Loader version, Yoko-Fantasy version, dependency versions, reproduction steps, and relevant logs or screenshots.

## Support

If Yoko-Fantasy saves you time while playing or developing Brotato mods, support is available through the deployed payment page:

**https://cyojkoy.github.io/Payment/**

## License

Yoko-Fantasy is distributed under the [MIT License](LICENSE).

<div align="center">
  <sub>Yoko-Fantasy · fantasy gameplay systems and content for Brotato</sub>
</div>
