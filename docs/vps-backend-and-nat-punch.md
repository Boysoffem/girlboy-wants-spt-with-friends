# VPS backend and NAT-punch planning

This note records a small-friends-group option for hosting the SPT/Fika backend on a publicly reachable VPS. It is planning guidance, not a tested install recipe. Confirm the exact SPT and Fika release requirements before deploying; do not expose a server with mismatched or unverified settings.

## What runs where

- The VPS runs `SPT.Server` with the Fika server mod. Friends point their SPT Launchers at this shared backend, so they do not need to be on the same Wi-Fi.
- The Fika server mod can also run a NAT-punch service on the VPS. This helps the raid host and joining players establish a direct UDP path through their routers.
- The VPS does **not** run the raid in this setup. The player who starts the raid hosts it on their gaming PC. After connection setup, raid traffic is between the players rather than relayed through the NAT-punch service.
- A Fika headless client is a separate SPT game-client instance used to host raids and offload AI work. Fika says headless does not work on a VPS; use a separate physical machine if you want headless hosting.

## Starting VPS size

For only the SPT/Fika backend and NAT-punch service, start around **2 vCPU, 4 GB RAM, and 50 GB SSD storage**. This is a practical estimate for a small private group, not a published Fika minimum or a benchmarked guarantee. Monitor resource use and resize if needed.

The community Docker guide summarized in [the headless guide review](fika-headless-guide-review.md) lists 4 vCPU, 16 GB RAM, and 100 GB for a deployment that also runs a headless game client and stores its game files. Those figures should not be treated as the backend-only minimum. The Fika wiki separately says the headless client is not supported on a VPS.

Prefer a VPS with a public IPv4 address, SSD storage, reliable network connectivity, and firewall rules you can manage. The Fika self-hosting guide lists these inbound ports for the NAT-punch setup:

- TCP `6969` for the SPT backend.
- UDP `6790` for the NAT-punch service.

Check the current guide and the selected release before opening ports. Do not assume the headless example's UDP `25565` is needed for a backend-only VPS; that port is for the raid host/headless networking path and its use depends on your chosen raid-hosting method.

## NAT-punch behavior and limits

The public or self-hosted NAT-punch server records the clients' public address/port mappings and introduces the peers. Both sides then send UDP packets to create temporary router mappings, allowing a direct raid connection when their NAT behavior supports it. NAT punching does not relay gameplay traffic and does not eliminate the need for an accessible SPT backend.

Fika's current self-hosting guide requires Full-Cone NAT compatibility on the raid host's and joining players' routers. It may fail on incompatible NATs, including symmetric NAT. Fika also offers a public NAT-punch service, which avoids managing the extra VPS service; its availability is not guaranteed and it receives the participants' public IP addresses. If punching fails, use Fika's documented VPN hosting option or normal port forwarding where possible.

For eero specifically, the default “Direct” NAT port mode is described by eero as broadly compatible with peer-to-peer applications, but eero does not label that mode Full-Cone NAT. That setting alone cannot guarantee Fika NAT-punch compatibility. An upstream router, double NAT, or ISP CGNAT may also affect connectivity.

## Linux and Mint notes

The self-hosted NAT-punch instructions are written for Windows. Fika says the Linux principle is the same but directs users to its Linux support channel for platform-specific help. The repository's pasted Docker guide is a community example, not an official Fika deployment file. In particular, its sample `127.0.0.1` address values need review: inside a container, loopback refers to that container, and advertised backend addresses must be reachable by the clients.

Linux Mint is Ubuntu-based, so Arch `pacman` commands do not apply as written. The Docker Compose approach is broadly distro-independent once Docker Engine and Compose are installed, but the exact server image, runtime, networking, and advertised IP settings still need validation against the selected SPT/Fika release. This VPS plan does not require moving the working Linux Mint game client to the VPS.

## Suggested proof of concept

1. Confirm the same compatible SPT and Fika versions for the VPS backend and every client.
2. Start with a temporary or low-cost VPS near the players, using the backend-only sizing above. Skip the headless container.
3. Install a separate server copy, enable the Fika NAT-punch service, and configure the backend to advertise its public VPS address rather than `127.0.0.1`.
4. Open TCP `6969` and UDP `6790` in both the VPS provider firewall and the operating-system firewall, as applicable.
5. Point each SPT Launcher at the VPS backend, then test that clients can see each other before starting a raid.
6. Have one player enable NAT punching and host a raid. The other player joins; the host's PC still runs the raid.
7. If the raid connection fails, verify router/NAT compatibility and try Fika's VPN or port-forwarding method before increasing VPS size.

Keep profiles, configuration, and logs persistent and back them up before changing versions. Do not share profile data or server access beyond the trusted group.

## References

- [Fika general information](https://wiki.project-fika.com/general-information.md)
- [Fika hosting options](https://wiki.project-fika.com/hosting-a-fika-server/choose-your-hosting-method.md)
- [Fika self-hosted NAT-punch server guide](https://wiki.project-fika.com/advanced-features/nat-punching/self-hosted-nat-punch-server.md)
- [Fika public NAT-punch server guide](https://wiki.project-fika.com/advanced-features/nat-punching/fika-nat-punch-server.md)
- [Fika NAT-punching overview](https://wiki.project-fika.com/advanced-features/nat-punching.md)
- [Fika headless client requirements](https://wiki.project-fika.com/advanced-features/headless-client.md)
- [eero NAT port mode](https://www.eero.com/support/articles/what-is-nat-port-mode)
- [Community Linux Docker guide review](fika-headless-guide-review.md)
