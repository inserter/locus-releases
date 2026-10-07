# LOCUS

Fewer addresses to remember. Less upkeep for your start page.

Find and open your NAS, home servers, and self-hosted apps from one page you host yourself.

[简体中文](README.zh-CN.md) · [Website](https://locus.casa/en/) · [Download](https://github.com/inserter/locus-releases/releases/latest) · [Feedback](https://github.com/inserter/locus-releases/issues)

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://locus.casa/shots/home-en-dark-1x.webp">
  <img src="https://locus.casa/shots/home-en-light-1x.webp" alt="LOCUS in an example homelab: pinned apps and websites, recent entries, and services grouped by device." width="760">
</picture>

*Example environment. Device names and service data are for demonstration.*

## At home

A NAS has a file share, an admin page, and perhaps SSH. A home server runs several apps, each on its own port. Some links are bookmarked; others survive only in browser history.

LOCUS gathers the services it can discover onto one page, grouped by device. Add the ones that stay quiet, then pin what you use. You do not need to write a complete dashboard configuration before getting started.

## Find your services. Open what you need.

- **Discover:** find services advertised through mDNS, SSDP/UPnP, and WS-Discovery. Discovery depends on what your devices announce and what the host can receive.
- **Search:** type a few letters, such as `nas` or `ssh nas`, to narrow the entries. Web pages open in a new tab.
- **Keep the familiar close:** pin, order, and group entries on the launchpad. Recent entries help you return to what you just opened. Missing pinned entries fade rather than vanish.
- **Add services that stay quiet:** paste an address, check the connection, and use the page title as a starting name. Associate it with a device, or add an external website separately.
- **Review candidates:** optional port probing suggests entries for you to accept or ignore. Candidate probing and scheduled scans are off by default.
- **Make it yours:** English and Chinese interfaces, light and dark modes, themes, icons, and backgrounds.

One device can have several ways in. LOCUS does not automatically discover every self-hosted application, and it does not sign you in to the services it opens.

## Install

Run LOCUS on a compatible Linux machine on your trusted home network, then open `http://<host-ip>:3033`.

### Native Linux

```sh
curl -fsSL https://locus.casa/releases/install | sh
```

The installer asks for a language, checks the machine, and guides you through the listen address, port, and data directory. It needs root or sudo to install a dedicated `locus` user and a systemd or OpenRC service.

Current native releases support **x86_64 and ARM64 (aarch64), with glibc 2.34 or newer**. Not every Linux-based NAS meets those requirements. ARMv7 is not ARM64; musl/Alpine native binaries and ARMv7 packages are not currently advertised as supported.

For manual downloads and checksum instructions, see the [installation guide](https://locus.casa/en/#manual) and [Releases](https://github.com/inserter/locus-releases/releases).

### Docker on Linux

```sh
curl -fsSL https://locus.casa/releases/latest/docker-linux-amd64.tar.gz | docker load
docker run -d --name locus --network host --restart unless-stopped -v locus-data:/data locus:latest
```

On ARM64, replace `amd64` with `arm64` in the download URL. Run these commands as a user allowed to use Docker. Host networking is required for discovery; there is no port mapping. The `locus-data` volume keeps your settings and records.

To use another port, append `--bind 0.0.0.0:<port>` to the `docker run` command. A NAS needs a compatible, working Docker environment; Docker is not a workaround for an unsupported CPU architecture. LOCUS does not manage containers.

## Before you start

- **Trusted LAN only.** LOCUS has no built-in login. Anyone who can reach it can change settings and use available device controls. Host and same-origin checks are browser protections, not authentication. Do not expose it directly to the public internet.
- **Discovery has boundaries.** It does not automatically cross isolated VLANs or VPNs. Services need suitable advertisements, network forwarding, or manual entries.
- **Non-web links need a client.** SSH, SMB, VNC, and similar links depend on your browser and operating system having a compatible URL protocol handler. LOCUS does not include a terminal, file manager, or remote desktop.
- **Status is the last check, not a promise.** Reachability and discovery presence are separate. HTTP 5xx is shown as a service error, not a healthy response.
- **Address following is conditional.** Stable identity and current neighbor records can help an eligible manual entry follow a changing IP. Ambiguous bindings need confirmation; this is not dynamic DNS.
- **Active probing is optional.** Enable scans only on networks you are authorized to check. They generate traffic and can trigger security alerts; start with a small scope.

Device control is limited: supported UPnP media players can expose playback and volume actions; IPP printers and ONVIF cameras provide read-only information and relevant links. Controls are on by default where supported. Restrict control clients with `LOCUS_CONTROL_CLIENTS` (`none` allows nobody), or disable control per device in the Situation panel. LOCUS is not a general smart-home or monitoring console.

## Your data, on your machine

Entries, discovery records, settings, launchpad layout, usage history, and uploaded assets live in the data directory: `/var/lib/locus` with the installer by default, or the `/data` volume under Docker.

No LOCUS cloud account is required. Local storage does **not** mean no network requests: discovery, connection checks, title/icon fetching, and device controls contact their targets. Configured external sites can cause internet requests. Release builds also check for updates; set `LOCUS_UPDATE_CHECK=off` to disable those checks.

Before upgrading, stop LOCUS and back up the **whole data directory**. Do not downgrade using only an older executable. An in-app Docker upgrade writes the new program into `/data`; it does not replace the Docker image.

See the [website](https://locus.casa/en/#before) for deployment boundaries and network behavior.

## Feedback

This repository holds official binary releases, release notes, and user feedback. **LOCUS is closed source; this is not its source repository.**

If something is missing or confusing, [open an issue](https://github.com/inserter/locus-releases/issues). Include the version, OS and CPU architecture, installation method, expected result, and steps to reproduce. For discovery problems, describe network isolation and whether the service advertises itself.

Redact screenshots and logs. Do not upload passwords, tokens, private keys, or your database.

## License

You may run unmodified copies free of charge on machines you control for personal or internal use. Redistribution, modification, and other uses are restricted by the [LOCUS license](https://locus.casa/license.txt). Third-party components retain their own licenses; see [third-party notices](https://locus.casa/third-party-notices.txt). Docker system-component information is included in the image as `SYSTEM_COMPONENTS.txt`.

LOCUS stands for *Local Object & Capability Unified Space*.
