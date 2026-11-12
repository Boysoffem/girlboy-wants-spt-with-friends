# Girlboy Wants SPT With Friends

Planning notes for hosting an SPT backend on a Linux home server, managing it with Pterodactyl, and connecting multiple clients over a local network. Project Fika is the planned co-op component.

> **Status:** Planning only. This repository contains documentation, not game files, mods, server binaries, or a ready-to-run Pterodactyl egg.

## Goal

Start with a LAN proof of concept where two SPT + Fika clients can use a backend hosted on a Linux rack server. The backend and the raid-hosting game client are separate roles; the first milestone focuses on the backend. A Fika headless client can be evaluated later if raid hosting or AI performance calls for it.

```text
SPT + Fika client ─┐
                   ├── LAN ──> Linux server: SPT/Fika backend
SPT + Fika client ─┘                     │
                                         └── Optional later: Fika headless client
```

## Starting point

The existing single-player install works on Linux. The LAN server should be a separate deployment so the known-working client remains untouched. The current notes focus on the architecture and setup guides; exact versions and settings must be confirmed when implementation begins.

See [Linux client setup notes](docs/linux-client-setup.md) for the install guide summary and [Fika headless guide review](docs/fika-headless-guide-review.md) for analysis of the supplied Docker Compose setup.

## Recommended server platform

**Ubuntu Server 24.04 LTS** is the recommended starting point for the rack server. Pterodactyl officially supports it, and it has mature Docker support. Debian 12 and 13 are also supported options. The client can remain on Linux Mint; client and server do not need to use the same distribution.

## Planned milestones

1. Confirm the exact SPT build and select Fika plugin/server releases compatible with it.
2. Prepare a separate server-side copy; keep the known-working client install intact.
3. Build and document a Pterodactyl egg/container for the native Linux SPT backend and its required runtime.
4. Persist profiles, configuration, and logs across container restarts.
5. Configure the backend to listen on the LAN and advertise the server's LAN address to clients.
6. Confirm Fika's required ports and networking mode before changing firewall or Pterodactyl allocations.
7. Connect one client, then a second client, and try a co-op raid.
8. Consider adding a Fika headless client as a separate service only if needed.

## References

- [SPT installer](https://sp-mod.com/installer)
- [Project Fika overview](https://wiki.project-fika.com/general-information)
- [Project Fika hosting options](https://wiki.project-fika.com/hosting-a-fika-server)
- [Project Fika server configuration](https://wiki.project-fika.com/fika-configuration/server)
- [Pterodactyl supported systems](https://docs.pterodactyl.io/v2/panel/requirements)
- [Pterodactyl custom egg guide](https://docs.pterodactyl.io/v2/guides/egg-creation/creating-custom-egg)
- [Community SPT Linux guide](https://github.com/MadByteDE/SPT-Linux-Guide)
- [Linux SPT installation video (without Fika)](https://www.youtube.com/watch?v=Fqn_N55WT38)

## Scope

This repository is for planning a home-LAN setup. It does not contain or redistribute Escape from Tarkov or SPT game files. It contains no machine paths, account credentials, profile data, local network addresses, or private configuration.
