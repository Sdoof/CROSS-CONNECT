# Home Network Recovery Package

Last reviewed: October 19, 2026

This folder contains the troubleshooting and recovery notes created during the Laptop 2 internet outage.

## Current status

| Computer | Status | Evidence |
|---|---|---|
| Laptop 2 | Network incident resolved | Ethernet and Wi-Fi were both confirmed working after the reset/restart/Windscribe-removal sequence. Windscribe is the leading suspect, but the individual contribution of each step was not isolated. |
| Laptop 1 | Network was working during the Laptop 2 incident | Its network guide was reviewed but has not been run on Laptop 1. Sluggish performance and local session recovery have not been diagnosed or device-tested. |
| Desktop | Network was working during the Laptop 2 incident | Its network guide was reviewed but has not been run on the Desktop. |

## Files

- `Laptop-2-Network-Troubleshooting.md`: the confirmed incident and recovery procedure.
- `Laptop-1-Network-Troubleshooting.md`: network procedure plus safe restart, Hibernate, OneDrive, and recovery guidance.
- `Desktop-Network-Troubleshooting.md`: the equivalent Windows desktop network procedure.
- `STATUS.md`: the checkpoint a future Codex session should read first.

## What happens if Laptop 1 logs out or powers off

Codex cloud work runs remotely after a request is submitted, so it does not depend on Laptop 1 remaining awake or signed in. A browser disconnect, logout, or local shutdown therefore does not erase work already saved in the conversation or workspace.

A live command, server, or background process should not be assumed to survive a cloud-environment pause or replacement. Durable progress must be written to files and, for long-term retention, committed and pushed to the repository or downloaded to another storage location. `STATUS.md` records the resume point so a later session can continue without reconstructing the history.

Reopen the same conversation after signing back in. If a new environment is created, read `STATUS.md` and the relevant manual before doing more work.

## Keep an offline copy before archiving

Download the package and place it in:

`C:\Users\olivi\OneDrive\Documents\AI STACK`

Wait until OneDrive says **Up to date**. For access during another network outage, right-click the folder in File Explorer and choose **Always keep on this device**. A USB copy or printed copy is also useful.

Do not publish screenshots of `ipconfig /all` without reviewing them first; they can contain the computer name and network adapter addresses.

## Limits

- No OneDrive connector is available in this cloud session, so the files cannot be placed in the Windows OneDrive folder directly.
- Laptop 1 and Desktop require safe, device-level checks before their procedures can be called tested.
- A full shutdown or forced power-off of Laptop 1 cannot guarantee restoration of unsaved local work. Use Save, completed OneDrive sync, application recovery, and Hibernate for planned pauses.
