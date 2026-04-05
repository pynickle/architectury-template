# Example Mod

*A concise Architectury Loom template for building a shared mod with Fabric and NeoForge loaders.*

## Overview

This repository is a minimal multi-loader template built with Architectury Loom. It splits shared code into `common/`, keeps Fabric-specific code in `fabric/`, and keeps NeoForge-specific code in `neoforge/`.

## Project structure

- `common/` — shared code, resources, and mixins
- `fabric/` — Fabric entrypoints and metadata
- `neoforge/` — NeoForge entrypoints and metadata
- `gradle.properties` — shared versions and mod coordinates

## Requirements

- JDK 25
- Gradle Wrapper (`./gradlew` or `gradlew.bat`)

## Getting started

```bash
./gradlew build
```

Use your IDE's generated run configs, or Gradle tasks, to launch Fabric or NeoForge during development.

> [!NOTE]
> This template currently targets Minecraft `26.1.1`, Fabric Loader `0.18.6`, Fabric API `0.145.3+26.1.1`, and NeoForge `26.1.1.6-beta`.

## What to rename first

Before turning this into a real project, update at least:

- `mod_id`, `archives_name`, `maven_group`, and `mod_version` in `gradle.properties`
- package names under `common/src/main/java`, `fabric/src/main/java`, and `neoforge/src/main/java`
- metadata in `fabric/src/main/resources/fabric.mod.json`
- metadata in `neoforge/src/main/resources/META-INF/neoforge.mods.toml`
- resource file names such as `example_mod.mixins.json` and `example_mod.accesswidener`

## Removing access wideners / access transformers

If your mod does **not** need access wideners or access transformers, remove the wiring as a group instead of deleting only one file.

### Remove Access Widener support

Delete these references:

- `common/build.gradle` → `loom { accessWidenerPath = file("src/main/resources/${mod_id}.accesswidener") }`
- `fabric/build.gradle` → `loom { accessWidenerPath = file("src/main/resources/${mod_id}.accesswidener") }`
- `neoforge/build.gradle` → `exclude "${mod_id}.accesswidener"`
- `common/src/main/resources/example_mod.accesswidener`
- `fabric/src/main/resources/example_mod.accesswidener`

### Remove Access Transformer support

Delete these references:

- `neoforge/src/main/resources/META-INF/neoforge.mods.toml` →

```toml
[[accessTransformers]]
file="META-INF/accesstransformer.cfg"
```

- `neoforge/src/main/resources/META-INF/accesstransformer.cfg`

> [!TIP]
> In this template, Fabric does not declare an access widener entry in `fabric.mod.json`, so you do not need to remove anything there.

## Notes

- Shared initialization starts in `common/src/main/java/com/example/example_mod/ExampleMod.java`
- Fabric metadata lives in `fabric/src/main/resources/fabric.mod.json`
- NeoForge metadata lives in `neoforge/src/main/resources/META-INF/neoforge.mods.toml`
