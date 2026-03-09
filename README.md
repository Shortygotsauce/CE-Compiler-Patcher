# Community Launcher: Step-by-Step Tutorial

Welcome! This guide will walk you through seting up your environment and building the Minecraft Legacy Console Edition (Oct 2014) source code using the Community Edition Launcher.

## 1. Prerequisites

Before you start, ensure you have the following tools installed:

- **Git**: Required for applying patches. [Download here](https://git-scm.com/)
- **CMake**: Required for modern builds. [Download here](https://cmake.org/)
- **Python**: Required for internal scripts. [Download here](https://www.python.org/)
- **Visual Studio 2022**: Required for modern builds. Community, Professional, or Enterprise editions are all supported.
- **Visual Studio 2012**: Required for **Legacy Builds** (raw Oct 2014 source).
- **Java (JRE)**: Required if you want to inject custom titles using FFDec. [Download here](https://www.java.com/)

### ⚠️ VS2012 Warning

If you previously had **Visual Studio 2010** installed, your VS2012 installation might be missing critical header files.

- **The Symptom**: Compilation fails with "file not found" errors for standard C++ headers.
- **The Fix**: Copy the `VC/include` folder from a working VS2012 installation to your own.

---

## 2. Using the Launcher

### Step 1: Environment Audit

When you open the compiler, it will scan for your tools.

- If a tool is marked as **Missing**, click "Browse manually" to point the launcher to the correct executable or installation folder.

### Step 2: Source Selection

Select the **mc-console-oct2014.zip** file. The launcher will verify its integrity.

### Step 3: Extraction

Click **Extract**. This will unpack the source code into a temporary directory (`build-temp`).

### Step 4: Patch Selection

Choose which patches you want to apply.

- **Mandatory**: If you are using VS2022, you MUST apply the "MSVC 2022 Compatibility" patch.
- **Optional**: Add features like Keyboard & Mouse support, LAN play, or custom logos.

### Step 5: Build

Select your **Build Profile** (Release is recommended for playing) and click **Start Build**.

- You can watch the real-time build progress in the terminal window.

### Step 6: Launch & Launcher Menu

Once the build is complete, you can click **Launch** to start the game immediately.

Alternatively, go to the **Launcher** menu from the Welcome screen:

- **Compiled Versions**: See a list of all your successful builds.
- **Add Build Manually**: If you have an existing `.exe` from a previous build, you can add it to the list.
- **Settings Overlay**: Click the **Settings** button on a build card to set your player name (`-name`) or enable **Headless Server** mode (`-server`).
- **Multiplayer Servers**: Click the **Servers** button to add server entries to the build's `servers.txt` file automatically.
- **Play**: Click **PLAY** to launch the game with your configured arguments.

---

## 3. Creating Your Own Patches

If you've made changes to the source code and want to share them:

1. Go to the **Create Patch** section in the launcher.
2. Select your **Original Source** (the unpatched ZIP).
3. Select your **Modified Source** folder.
4. Fill in the name, author, and description.
5. Click **Create Patch**. The launcher will generate a `.patch` file and its metadata for you to share!

---

## 4. Troubleshooting

- **Build Fails**: Check the terminal output. Most errors are due to missing tools or the VS2012 header issue mentioned above.
- **Patch Application Fails**: Ensure you are applying patches to the correct version of the source code.
