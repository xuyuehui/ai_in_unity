# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**ai_in_unity** — A Unity project that integrates AI capabilities with the Unity Editor via the [MCP for Unity](https://github.com/CoplayDev/unity-mcp) package (`com.coplaydev.unity-mcp` v9.6.8). This bridge allows AI assistants to directly control the Unity Editor through the Model Context Protocol (MCP).

- **Unity Version:** 2022.3.62f1
- **Render Pipeline:** Universal Render Pipeline (URP) 14.0.12
- **IDE:** VS Code with Visual Studio Tools for Unity (`vstuc`)
- **Debugging:** "Attach to Unity" launch config in `.vscode/launch.json`

## MCP for Unity Architecture

The MCP package (`Library/PackageCache/com.coplaydev.unity-mcp@b92c05a258/`) exposes Unity Editor capabilities as MCP tools available to AI clients. Key tool categories:

| Category | Tools |
|---|---|
| Scene | `ManageScene` — create/open scenes, place GameObjects, set transforms |
| Asset | `ManageAsset` — import, create, delete, move assets |
| Script | `ManageScript` — create/modify C# scripts, `ExecuteCode` — run arbitrary C# in editor |
| UI | `ManageUI` — Canvas, UI elements, event systems |
| Material/Shader | `ManageMaterial`, `ManageShader`, `ManageTexture` |
| Build | `ManageBuild` — build settings, build player |
| Package | `ManagePackages` — add/remove Unity packages |
| GameObjects | `ManageComponents` — add/remove/modify components |
| Editor | `ManageEditor` — editor windows, preferences, project settings |
| Animation | `Animation` — clips, controllers, animators |
| Physics, Cameras, Graphics, VFX, ProBuilder, Profiler | Category-specific tools |

Transport modes: HTTP (default) and Stdio (`McpCiBoot.StartStdioForCi()` for CI environments).

## Commands

### Opening the project
```
/Applications/Unity/Hub/Editor/2022.3.62f1/Unity.app/Contents/MacOS/Unity -projectPath .
```

### Running tests (Unity Test Framework)
Tests can be run via the Unity Editor Test Runner window, or through MCP tool `RunTests`.

### Building
Use the MCP `ManageBuild` tool or Build Settings window. Build target defaults to StandaloneWindows based on project settings.

### Package management
- Add packages: via Unity Package Manager window or MCP `ManagePackages`
- Manifest: `Packages/manifest.json`
- Lock file: `Packages/packages-lock.json`

## Project Structure

```
Assets/
  Scenes/
    SampleScene.unity         # Default scene
  Settings/                   # URP pipeline assets and quality profiles
  TutorialInfo/               # Default Unity tutorial readme (can remove)

Packages/
  manifest.json               # Package dependencies
  packages-lock.json

ProjectSettings/              # Unity project settings (editor, graphics, quality, etc.)
```

All C# scripts for game logic go under `Assets/` (create appropriate subdirectories). The `.gitignore` follows the standard Unity template — `Library/`, `Temp/`, `Logs/`, `UserSettings/` and auto-generated `.csproj`/`.sln` files are not tracked.
