# Fika Docker guide review

Summary of the Fika setup guide pasted by the user. This is a reference only; no Docker containers or game files have been changed.

## What the guide sets up

The guide is for SPT 4.1.x and describes a Docker Compose deployment with two services:

1. `fika-server`, using `ghcr.io/sp-tushonka/server-csharp:latest`, with persistent server user data under `./server` and TCP port 6969.
2. `fika-headless`, using `ghcr.io/zhliau/fika-headless-docker:latest`, with a full SPT client install under `./headless` and UDP port 25565. It uses a generated headless profile ID from the Fika server configuration.

The guide's stated minimum host specs are 4 vCPU cores, 16 GB RAM, and 100 GB storage. Its headless client step uploads roughly 60 GB of game files. It asks for Fika 1.5.x for SPT 4.1.x and Fika server 2.4.x.

## Difference from the planned Pterodactyl proof of concept

The current plan is to put the SPT/Fika backend on the Linux rack server and manage it through Pterodactyl. This guide uses Docker Compose directly and also adds a headless game client for AI/raid hosting. The headless service is optional for the first backend-only milestone and can be evaluated later.

## Addressing details to verify before adapting

- The sample backend settings use `SPT_IP=127.0.0.1` and `SPT_BACKEND_IP=127.0.0.1`. Those values are loopback addresses and are not suitable as the advertised backend address for separate LAN clients.
- The sample Compose file puts the backend and headless client in separate containers, while `SERVER_URL=127.0.0.1` is also shown for the headless service. In separate containers, loopback points to the current container itself. Verify the intended service DNS name/address and current Fika instructions before using this configuration.
- For LAN clients, the backend must listen on an interface reachable from the LAN and advertise the rack server's LAN address (or another correct reachable address). Confirm the expected protocol and port allocations for the selected Fika release before configuring Pterodactyl or firewall rules.
- The guide's 25565/UDP allocation belongs to the headless service example; do not assume it is needed for a backend-only setup without confirming the selected networking mode and Fika version.

## Security and operations notes

- The guide adds the `fika` account to `sudo` and to the `docker` group. Membership in the Docker group grants broad control over the host, so these steps need deliberate review when adapting the setup.
- It uses `latest` image tags. For a reproducible POC, pin compatible image versions/digests once the exact SPT and Fika releases are confirmed.
- Treat `./server` and generated user/profile data as persistent state; back it up before upgrades or experiments.

## Proposed adaptation

Use a separate server-side deployment for the Pterodactyl setup. Keep any known-working client install unchanged.

## Source

Community guide titled “Basic Linux experience. it's better for the both of us Specs needed (will be updated at random times)”. It contains a Docker Compose example plus steps for generating a headless profile. Validate version compatibility and configuration against the exact Fika release chosen later.
