# Home Network Recovery Package

Last reviewed: October 19, 2026

This folder contains the troubleshooting and recovery notes created during the Laptop 2 internet outage.

The durable source is the `main` branch of `Sdoof/CROSS-CONNECT` on GitHub. A fresh clone was verified on October 19, 2026. A ZIP copy is also available in the cloud workspace for download.

## Current status

| Computer | Status | Evidence |
|---|---|---|
| Laptop 2 | Network incident resolved | Ethernet and Wi-Fi were both confirmed working after the reset/restart/Windscribe-removal sequence. Windscribe is the leading suspect, but the individual contribution of each step was not isolated. |
| Laptop 1 | Network was working during the Laptop 2 incident | Its network guide was reviewed but has not been run on Laptop 1. Sluggish performance and local session recovery have not been diagnosed or device-tested. |
| Desktop | Network was working during the Laptop 2 incident | Its network guide was reviewed but has not been run on the Desktop. |

## Files

- [Canonical agent instructions](AGENTS.md): shared project rules for Codex and compatible coding agents.
- [Claude Code adapter](CLAUDE.md): imports the canonical instructions without duplicating them.
- [Laptop 2 network troubleshooting](docs/Laptop-2-Network-Troubleshooting.md): the confirmed incident and recovery procedure.
- [Laptop 1 network troubleshooting](docs/Laptop-1-Network-Troubleshooting.md): network procedure plus safe restart, Hibernate, OneDrive, and recovery guidance.
- [Desktop network troubleshooting](docs/Desktop-Network-Troubleshooting.md): the equivalent Windows desktop network procedure.
- [Resume checkpoint](STATUS.md): the file a future Codex session should read first.

## What happens if Laptop 1 logs out or powers off

After a request is accepted and visible in the chat, its computation occurs remotely and may continue if the browser closes or Laptop 1 signs out or shuts down. It can still pause for required input or platform limits. A live command, server, or background process must not be assumed to survive a cloud-environment pause or replacement.

Windows sign-out closes local user applications, and unsent text in a browser or chat box is not a saved checkpoint. Signing out of Codex can also require signing in again before the conversation is available. Files that exist only in `/workspace` are temporary; durable progress must be committed and pushed to GitHub or downloaded elsewhere.

Reopen the same conversation after signing back in. GitHub `main` plus `STATUS.md` provides the verified manual resume path. Automatic restoration in a fresh cloud environment remains unverified until the pending environment draft is published and a later restore check succeeds.

## Keep an offline copy before archiving

Download the package and place it in:

`C:\Users\olivi\OneDrive\Documents\AI STACK`

Wait until OneDrive says **Up to date**. For access during another network outage, right-click the folder in File Explorer and choose **Always keep on this device**. A USB copy or printed copy is also useful.

Do not publish screenshots of `ipconfig /all` without reviewing them first; they can contain the computer name and network adapter addresses.

## Limits

- No OneDrive connector is available in this cloud session, so the files cannot be placed in the Windows OneDrive folder directly.
- A concise cloud resume instruction has been saved in the environment configuration draft. It takes effect for future environments only after the user reviews, saves, and publishes that draft.
- Claude Code, Kilo Code, Cline, Roo, Cursor, and their global configuration are not installed or mounted in this cloud environment. Project-level `AGENTS.md` is canonical; no global cross-drive synchronization has been performed.
- Laptop 1 and Desktop require safe, device-level checks before their procedures can be called tested.
- A full shutdown or forced power-off of Laptop 1 cannot guarantee restoration of unsaved local work. Use Save, completed OneDrive sync, application recovery, and Hibernate for planned pauses.
