# Community Launcher: Patch Developer Guide

This guide explains how the patching system works and how to create patches for the community.

## The Problem with Traditional Modding

Traditionally, modifying closed-source games or engines meant creating entirely new codebases. Modders fork a project, add their unique features like multiplayer, new minigames, UI elements, or custom graphics and release an entirely standalone version of the game.

This approach creates **community splintering**:

- Players are forced to choose between Client A's improved netcode or Client B's vastly superior UI.
- Updates are rarely cross-compatible.
- A fix made in one fork must be manually ported to dozens of others, or it is lost.
- Over time, the community fragments into isolated, incompatible camps.

## The Solution: A Universal Base with Modular Patches

Because the community now has access to the original source code, we don't need to distribute monolithic forks. We distribute **Features**,**Improvements** or **Updates** via **Patches**.

**The Modular Vision is built on 4 pillars:**

1. **The Source is the Canvas:** Everyone begins with the exact same, source code as the unalterable baseline.
2. **Features over Forks:** All development is done strictly on top of this baseline. When changes are made, they are distributed as lightweight, readable `.patch` files rather than full repository forks or pre-compiled binaries.
3. **True Mix-and-Match:** Because changes are localized, players can stack them. A user can download a patch that adds online multiplayer, and apply it seamlessly with a completely separate patch that modifies the UI. A patch that modifies the UI has zero impact on a patch that updates network protocols.
4. **Universal Availability:** Community Edition: Compiler & Patcher aims to streamline this process. They take the raw source, dynamically apply only the community patches you select, cleanly compile the engine on the fly, and launch a truly customized experience.

By committing to a modular source patching philosophy rather than distributing hard-forked executables, our development efforts compound rather than compete. When someone fixes an engine bug, everyone benefits.

The source code belongs to everyone, and by sharing our changes modularly, the community stays united.

---

## 1. Patch Types

The launcher supports two types of patches:

### Standard Unified Patches (.patch)

These are standard Git patches generated via `git diff`. They are the most powerful and can modify any number of files at once.

- **Binary Support**: The launcher supports binary assets in patches using the `--binary` flag.
- **Sparse Support**: We use `--diff-filter=d` to ensure that "missing" files in sparse fork directories aren't treated as deletions.

### JSON Find-Replace (.json)

For simple text-based changes (like disabling a flag or changing a constant), you can use a JSON patch:

```json
{
  "file": "Minecraft.Client/Common/App.cpp",
  "find": "DEBUG_ENABLED = true;",
  "replace": "DEBUG_ENABLED = false;"
}
```

---

## 2. Creating a Patch via Command Line

While the Launcher UI has a built-in creator, you can also generate patches manually:

```bash
git diff --no-index --binary --diff-filter=d path/to/vanilla_src path/to/your_modded_src > my_mod.patch
```

### Why these flags?

- `--no-index`: Allows diffing two local folders that aren't git repositories.
- `--binary`: Encodes binary modifications (images, archives) safely into the patch.
- `--diff-filter=d`: **CRITICAL**. Since forks only contain changed files, Git would otherwise think you deleted every unmodified file in the original source, creating a multi-gigabyte patch file.

---

## 3. Metadata (.meta.json)

Every `.patch` or `.json` file should have a companion `.meta.json` file. This tells the launcher how to display and handle the patch.

```json
{
  "name": "My Cool Mod",
  "version": "1.0.0",
  "author": "YourName",
  "description": "Adds cool features to the game.",
  "features": ["Feature A", "Feature B"],
  "requires": ["base_fix.patch"],
  "conflicts": ["incompatible_mod.patch"],
  "modifiedFiles": ["Minecraft.Client/UI/Menu.cpp"],
  "created": "2026-03-07"
}
```

- **requires**: A list of filenames that must be applied *before* this patch.
- **conflicts**: A list of filenames that cannot be present when this patch is applied.
- **modifiedFiles**: (Optional) A list of files modified by this patch to help the launcher detect overlaps.

---

## 4. Best Practices

1. **Keep it focused**: Patches should be focused. If you have multiple features, create multiple patches.
2. **Include Metadata**: Always provide a `.meta.json` so users know what they are installing.
3. **Avoid Binary Bloat**: Only include binary assets if absolutely necessary.
