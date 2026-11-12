# Linux install guide notes (SPT without Fika)

These notes summarize a community install guide for SPT 4.1.x on Linux. The guide covers the single-player setup without Fika; it is not a LAN or co-op setup guide.

## Requirements from the guide

- Own the official Escape from Tarkov game.
- Install and update the official game first, and run it to the main menu.
- Copy the official game files into a separate folder before patching. Do not patch the live game folder.

## Installation flow in the pasted guide

1. Copy the contents of the official game directory into a separate folder outside the original game directory.
2. Choose the matching patcher for the intended SPT version (the guide lists 4.0.13 and 4.1.x).
3. Extract and run the patcher in the copied folder, then remove the patcher when done.
4. Download the matching SPT build archive and extract its contents into the copied folder, allowing the intended files to be overwritten.
5. For 4.1.x, mark `SPT_Runtime/SPT.Server.Linux` executable and run it from a terminal once.
6. For 4.1.x, launch `SPT.Launcher.Linux`; set its UMU path to `umu/umu-run` if UMU was manually installed in the prefix. The guide lists `WINEDLLOVERRIDES="winhttp=n,b"` as a launch argument for the game.
7. In Lutris, set the launcher executable and configure the Wine/Proton runner and paths for the install.

The guide also describes a separate 4.0.13 path using the Windows launcher executable in Lutris. That path is not the current 4.1.x setup summarized here.

## Runtime note

The guide's `pacman -S ...` commands are for Arch Linux. Use distribution-specific package instructions for other distributions. For SPT 4.1.x, the guide calls for .NET and ASP.NET Core 10 runtimes; confirm the runtime requirement for the exact server build being used.

## Source

Community installation guide titled “Install instructions for 4.0.13 and 4.1.x (latest).” The [Linux installation video](https://www.youtube.com/watch?v=Fqn_N55WT38) covers SPT without Fika.
