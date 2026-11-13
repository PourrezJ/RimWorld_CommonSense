# Common Sense — RimWorld 1.7 port

This is a fork of [catgirlfighter/RimWorld_CommonSense](https://github.com/catgirlfighter/RimWorld_CommonSense), the source for the [Common Sense Steam Workshop mod](https://steamcommunity.com/sharedfiles/filedetails/?id=1561769193). Credit for the original mod goes to avilmask and the upstream contributors.

## RimWorld 1.7 support

The `rimworld-1.7` branch includes:

- A distinct `1.7/` folder with the XML resources and a prebuilt .NET 10 `CommonSense.dll`.
- A separate `Source/CommonSense17` SDK-style project targeting `net10.0`, using RimWorld 1.7's `Assembly-CSharp.dll`, `UnityStubs.dll`, and `Waylith.dll`.
- The final code from [upstream PR #70](https://github.com/catgirlfighter/RimWorld_CommonSense/pull/70), including the doctor triage patch. **Note:** the last PR commit reverted the cleaning/ingestion stability changes described in the PR body; they are *not* part of the PR's final diff or this port.
- `About/About.xml` declaring support for RimWorld 1.7.

Existing 1.0–1.6 assets and sources remain unchanged on this branch.

### Installation

1. Download the `rimworld-1.7` branch ZIP and extract it, preserving the `About`, `1.7`, `Languages` and `Textures` directories.
2. Put the extracted mod folder into RimWorld's local `Mods` directory.
3. Enable [Harmony](https://steamcommunity.com/sharedfiles/filedetails/?id=2009463077) before Common Sense. Disable any other installed copy of Common Sense to avoid duplicate `avilmask.CommonSense` package IDs.

### Building from source

Requires .NET 10 SDK, a local RimWorld 1.7 installation, and NuGet access for `Lib.Harmony`. The bundled game assemblies are **not** redistributed in this repository.

```bash
# Linux example; use an absolute path for your own RimWorld installation.
~/.dotnet/dotnet build Source/CommonSense17/CommonSense/CommonSense.csproj \
  --configuration Release \
  -p:RimWorldDir="$HOME/.steam/steam/steamapps/common/RimWorld"
```

Build output `CommonSense.dll` is copied into `1.7/Assemblies/`. Runtime Harmony is supplied by the Harmony mod, **not** bundled in the Common Sense mod package.

### Validation

- Compiled against **RimWorld 1.7.5005 rev1082**, .NET SDK 10.0.112: **0 errors, 0 warnings**.
- All 1.7 mod XMLs and About metadata parse successfully.
- All 117 Common Sense assembly types can be loaded in a standalone reflection check using the 1.7 game binaries and the installed Harmony 3 runtime.
- The `WorkGiver_Scanner.Prioritized` getter and `GetPriority(Pawn, TargetInfo)` override points used by PR #70 still exist in 1.7.

**Limitations:** this is build/API validation, *not* an in-game gameplay regression test. Other Harmony patch behavior and mod-to-mod interactions still need testing in RimWorld 1.7.
