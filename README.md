# Example Mod

*A concise Architectury Loom template for building a shared mod with Fabric and NeoForge loaders.*

## Overview

This repository is a minimal multi-loader template built with Architectury Loom. It splits shared code into `common/`, keeps Fabric-specific code in `fabric/`, and keeps NeoForge-specific code in `neoforge/`.

## Project structure

- `common/` — shared code, resources, mixins, and the Access Widener
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
> This template currently targets Minecraft `26.1.2`, Fabric Loader `0.19.3`, Fabric API `0.155.2+26.1.2`, and NeoForge `26.1.2.95`.

## What to rename first

Before turning this into a real project, update at least:

- `mod_id`, `archives_name`, `maven_group`, and `mod_version` in `gradle.properties`
- package names under `common/src/main/java`, `fabric/src/main/java`, and `neoforge/src/main/java`
- metadata in `fabric/src/main/resources/fabric.mod.json`
- metadata in `neoforge/src/main/resources/META-INF/neoforge.mods.toml`
- resource file names such as `example_mod.mixins.json` and `example_mod.accesswidener`

## Access wideners

This template keeps a single Access Widener in `common/src/main/resources/${mod_id}.accesswidener` and wires both loaders to that file.

- Fabric copies it into the jar (`processResources`) and injects it with Loom (`loom.injectAccessWidener`). `fabric.mod.json` declares the same file as `"accessWidener"`.
- NeoForge reads the same file through `loom.accessWidenerPath` and converts it to `META-INF/accesstransformer.cfg` when building the remapped jar (`loom.neoForge.convertAccessWideners`). There is no hand-written Access Transformer source file.

### Removing access widener support

If your mod does **not** need access wideners, remove the wiring as a group instead of deleting only one file.

- `common/build.gradle` → `loom { accessWidenerPath = file("src/main/resources/${mod_id}.accesswidener") }`
- `common/src/main/resources/example_mod.accesswidener`
- `fabric/build.gradle` → `commonAccessWidener`, `loom.accessWidenerPath`, `processResources { from(commonAccessWidener) }`, `shadowJar { exclude "${mod_id}.accesswidener" }`, and `loom.injectAccessWidener(...)`
- `fabric/src/main/resources/fabric.mod.json` → `"accessWidener": "example_mod.accesswidener"`
- `neoforge/build.gradle` → `commonAccessWidener`, `loom.accessWidenerPath`, and `loom.neoForge { convertAccessWideners(...) }`
- `neoforge/src/main/resources/META-INF/neoforge.mods.toml` →

```toml
[[accessTransformers]]
file="META-INF/accesstransformer.cfg"
```

> [!TIP]
> NeoForge still lists `META-INF/accesstransformer.cfg` in `neoforge.mods.toml` because Loom generates that file from the common Access Widener at jar time. Do not add `neoforge/src/main/resources/META-INF/accesstransformer.cfg` unless you are leaving this shared-widener setup.

## Notes

- Shared initialization starts in `common/src/main/java/com/example/example_mod/ExampleMod.java`
- Fabric metadata lives in `fabric/src/main/resources/fabric.mod.json`
- NeoForge metadata lives in `neoforge/src/main/resources/META-INF/neoforge.mods.toml`
