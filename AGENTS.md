# Project instructions

This repository contains durable, beginner-friendly Windows network recovery documentation. It has no build, dependency-installation, or service-startup step.

- Use the existing checkout. At the start, run `git status --short --branch`, then read `STATUS.md`, `README.md`, and only the manual relevant to the task.
- Preserve existing work. Separate confirmed facts, suspected causes, and untested guidance. Do not call the Laptop 1 or Desktop procedures tested until device evidence confirms them.
- Prefer reversible diagnostics before resets, uninstalls, or driver changes. Give exact novice-safe steps and warn before restarts or changes that can interrupt access.
- Keep credentials, Wi-Fi passwords, and unnecessary personal or device identifiers out of files and output.
- Treat `/workspace` and live processes as temporary. When state changes, update `STATUS.md`; before stopping, persist authorized work to Git or a downloadable artifact and report anything uncommitted or unpushed.
- On resume, inspect Git and `STATUS.md` before repeating an action. Do not assume a process survived logout, shutdown, or environment replacement.
