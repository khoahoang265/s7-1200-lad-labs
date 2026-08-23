# Archived TIA project

`LAb1.2.zap16` — the TIA Portal V16 archive for Lab 2 — Sequential three-lamp cycle, produced by
**Project → Archive** with *Discard restorable data* enabled. It contains the
`.ap16` project file and the full project tree in one file.

## Restoring it

1. In TIA Portal V16: **Project → Retrieve**
2. Select `LAb1.2.zap16`
3. Pick a short local destination folder, e.g. `C:\TIA\`

TIA opens the retrieved project automatically. Compile the PLC and the HMI
before downloading — see [`docs/setup.md`](../../../docs/setup.md).

A newer TIA Portal will open this archive but upgrades the project
irreversibly; it can then no longer be retrieved on V16.

## Updating it

After changing anything in TIA, run **Project → Archive** again and overwrite
this file, then commit it. Never commit the live project folder (`*.ap16` and
its subdirectories) — see the comment block at the top of `.gitignore`.
