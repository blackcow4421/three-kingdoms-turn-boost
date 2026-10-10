# Three Kingdoms Turn Boost

## v0.1.1 — Battle Loading Hotfix

[Download v0.1.1](https://github.com/blackcow4421/three-kingdoms-turn-boost/releases/tag/v0.1.1). The Stage 5 designated-worker queue restriction bypass was removed after a battle-loading hang. The same battle loaded successfully after restoring this change. Other optimizations and DLLs are unchanged; the optional 190 pack remains v0.1.0. One successful retest does not establish stability for every configuration.

To upgrade from v0.1.0, close the game, extract the new engine ZIP and run `Install.bat`. Preserve `TKTurnBoostBackup/Three_Kingdoms.original.exe`; if it is missing, verify game files through Steam before installation.

A test patch for the Windows Steam edition of **Total War: THREE KINGDOMS**.
It optimizes selected engine queries and repeated processing, including some worker paths.
The optional **190 Expanded Boost** adds a Lua relationship-query optimization.

## Downloads

- **ThreeKingdoms_TurnBoost_v0.1.1.zip** — engine patch; extract everything and run `Install.bat`.
- **190_Expanded_Boost_v0.1.0_PackOnly.zip** — optional 190 Expanded pack; copy the pack to the game's `data` folder.
- **ThreeKingdoms_TurnBoost_v0.1.1_Source.zip** — C++ sources, installer, patch generator, and build instructions.
- **SHA256SUMS_v0.1.1.txt** — download checksums.

Installation instructions in Korean, English, and Simplified Chinese are included in both download packages.
Close the game before installation. The engine installer verifies the supported EXE hash,
backs up the original, and supports restoration through `Restore.bat`.
Enable the optional pack in your mod manager and place 190 Expanded Boost at the very top of the load order.
Only install the 190 pack when using 190 Expanded.

## What this patch does

The engine patch changes selected worker/queue paths and reduces repeated work in
specific internal lookups, string searches, temporary vectors, and score-map construction.
It does not make the entire end turn run on every CPU core, and does not offload it to the GPU.
The optional Lua pack combines friend/rival counting into one read-only relationship traversal.
The previously unstable stage-4 worker-limit modification and Stage 5 queue restriction bypass are excluded.

## Test results and scope

Development system: Ryzen 7 9800X3D (8 cores, 16 threads).

| Configuration | User-measured end-turn time |
| --- | --- |
| 190 Expanded, optional pack only | 32 s |
| 190 Expanded, engine patch only | 32 s, including approximately 2 s skipping an event |
| 190 Expanded, both | 29 s |

MTU+TUP was reported in the teens; the latest internal instrumented segment was **8.37 s**.
Internal segment timing is not the same as a full end-turn stopwatch measurement.
These results were recorded before the v0.1.1 hotfix, come from different turns/campaigns and do not establish a controlled speedup over an unpatched game.
Other CPUs, all mod combinations, and long-term stability have not been verified.

The distribution's installation, reinstall, restore, unsupported-EXE rejection,
Unicode/space paths, and DLL loading from the executable directory have been tested on copies/helper programs.
The final distribution uses DLLs in the game directory instead of the development directory.
Execution of the final release-path configuration was confirmed by fresh DLL logs from the actual game. The user also confirmed successful battle loading after the v0.1.1 change.

## Source and build

The source archive includes C++ implementation files and headers, x64 thunk generation,
patch metadata, the PowerShell installer, and a build script for the optional pack.
See `docs/BUILD.md` in the source archive. Build requirements are Windows x64,
Visual Studio C++ Build Tools, Python, and the listed Python dependencies.
Generating the engine payload requires your own supported original game EXE;
building the optional pack requires your own matching 190 script.
Game executables and the complete original 190 script are not included in the source archive.

## Feedback

Please report your CPU, game/patch version, mod combination, installation result,
and end-turn time. Include event/dialogue delays and whether a crash also occurs without this patch.
