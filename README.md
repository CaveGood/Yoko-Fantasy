<div align="center">
  <img src="assets/hero.svg" alt="Yoko-Fantasy — systems-heavy fantasy expansion for Brotato" width="1200" style="max-width: 100%; height: auto;">

  <h1>Yoko-Fantasy</h1>
  <p><strong>A systems-heavy fantasy expansion for Brotato, combining content with mechanics that attach to existing game systems.</strong></p>
  <p>Jobs · Souls · Holy · Erosion · Combat · Enemies · Worlds · UI</p>

  <p>
    <a href="https://github.com/CYoJkoY/Yoko-Fantasy/releases"><img src="https://img.shields.io/github/v/release/CYoJkoY/Yoko-Fantasy?display_name=tag&sort=semver&style=flat-square&label=release" alt="Latest release"></a>
    <a href="https://github.com/CYoJkoY/Yoko-Fantasy/actions/workflows/release.yml"><img src="https://img.shields.io/github/actions/workflow/status/CYoJkoY/Yoko-Fantasy/release.yml?style=flat-square&label=build" alt="Build status"></a>
    <img src="https://img.shields.io/badge/Brotato-1.15.4-478CBF?style=flat-square" alt="Brotato 1.15.4">
    <img src="https://img.shields.io/badge/Mod%20Loader-6.3.0-5965FF?style=flat-square" alt="Mod Loader 6.3.0">
    <a href="LICENSE"><img src="https://img.shields.io/github/license/CYoJkoY/Yoko-Fantasy?style=flat-square" alt="MIT License"></a>
  </p>

  <p><a href="#systems">Systems</a> · <a href="#architecture">Architecture</a> · <a href="#installation">Install</a> · <a href="#development">Develop</a> · <a href="#compatibility">Compatibility</a></p>
</div>

> **Core boundary:** Godot resources define fantasy content, NewContentLoader handles common registration, and focused extensions integrate only the systems that need custom runtime behavior.

## What it is

Yoko-Fantasy is a content and gameplay expansion for Brotato. It is designed for mechanics that need more than a new item or weapon definition but do not justify a second gameplay framework.

```text
Content resources
      │
      ▼
NewContent registration
      │
      ▼
Targeted extensions
      │
      ▼
Brotato runtime systems
```

The result is a mod that can add substantial systems while keeping data inspectable and integrations localized.

## Systems

| System | Scope |
| :--- | :--- |
| Jobs | Job resources, registration, run integration, menu flow, and end-of-run handling |
| Soul | Soul statistics, consumables, drops, and player / run integration |
| Holy | Holy statistics, item interactions, and enemy behavior |
| Erosion | Erosion-focused items and effects |
| Limited Items | Restricted item pools and progression bonuses |
| Combat | Kill progression, reload triggers, weapon switching, lightning chains, hit effects, reflection, and critical-damage interactions |
| Enemies | Custom enemies, cursed behavior, targeting, spawning, healing, and world interactions |
| Entities | Pets, turrets, gardens, wandering bots, and special entities |
| Shop & Waves | Shop interactions, rerolls, synthesis, extra enemies, elites, and wave behavior |
| World | Fantasy zones, backgrounds, title-screen resources, and map-specific behavior |
| UI & Localization | Menu integration, cooperative focus handling, descriptions, and translations |

Representative content includes Holy and Blazing Path weapon variants, Prism Tower, Healing Star, Soul Link, Crow, fantasy pets, plant-themed enemies, and custom attack behavior.

## Architecture

`mod_main.gd` is the main integration point. It installs explicit extensions for the Brotato subsystems used by the expansion, keeping the integration surface visible.

`FantasyNewContent.gd` connects the project to [Yoko-NewContentLoader](https://github.com/CYoJkoY/Yoko-NewContentLoader), registers custom resources, and supplies project-specific content hooks such as Soul consumable selection.

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

These dependencies are declared by `manifest.json`.

### Release installation

1. Install Brotato 1.15.4 and Mod Loader 6.3.0.
2. Install Yoko-NewContentLoader and Yoko-MoreStatsContainer.
3. Download the latest `Fantasy-*.zip` from [Releases](https://github.com/CYoJkoY/Yoko-Fantasy/releases).
4. Place the ZIP in the Mod Loader `mods` directory.
5. Launch Brotato and confirm the dependency chain loads successfully.

The release archive is intended to remain compressed in the normal `mods` directory.

## Development

When adding a mechanic, identify the owning Brotato system first and add the narrowest integration required.

```text
New mechanic
   ├── Content / Resource      → content/
   ├── Reusable behavior       → content/... or extensions/...
   ├── Base-game integration   → extensions/<system>.gd
   └── Localization            → translations/...
```

A practical workflow is: define the resource data, add the smallest runtime extension, register it in `mod_main.gd`, update localization, then test the complete dependency stack.

## Release model

`manifest.json` is the source of truth. Release tags must match its declared version exactly.

```text
manifest.json: 1.1.0
        │
        ├── v1.1.0     → build allowed
        └── v1.2.0     → build rejected
```

The workflow imports Godot resources, checks for import errors, preserves generated `.import` data, packages the Mod Loader ZIP, validates its structure, and verifies the packaged manifest.

## Compatibility

| Component | Version |
| :--- | :--- |
| Brotato | **1.15.4** |
| Godot | 3.x / GDScript |
| Mod Loader | **6.3.0** |
| Yoko-Fantasy | **1.1.0** |
| Dependencies | Yoko-NewContentLoader, Yoko-MoreStatsContainer |
| Authors | CYoJkoY, CaveGood |
| License | MIT |

Compatibility and dependency metadata live in `manifest.json`.

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

Useful contributions add coherent content, fix concrete bugs, improve compatibility, contribute reproducible gameplay feedback, or keep runtime extensions focused.

For bug reports, include the Brotato, Mod Loader, Yoko-Fantasy, and dependency versions together with reproduction steps and relevant logs or screenshots.

## Support

Development support is available through the deployed payment page:

**https://cyojkoy.github.io/Payment/**

## License

Yoko-Fantasy is distributed under the [MIT License](LICENSE).

<div align="center">
  <sub>Yoko-Fantasy · fantasy gameplay systems and content for Brotato</sub>
</div>
