# SIMATIC S7-1200 Lab Series — LAD Programming with KTP700 HMI

Four TIA Portal projects for a SIMATIC S7-1200 (CPU 1211C AC/DC/Rly) with a
KTP700 Basic PN panel, built for the Industrial Automation laboratory course.
The series walks from a set/reset latch with two timers up to a mode-switched
water-tank controller split across four function blocks.

`TIA Portal V16` · `S7-1200 · CPU 1211C AC/DC/Rly · FW V4.4` ·
`LAD` · `WinCC Basic / KTP700 Basic PN` · `4 labs`

> Every ladder network, tag table and HMI screen layout in this repository was
> extracted from the TIA Portal project printouts. The runtime captures, the
> test results and the demo clips come from running the four projects on
> S7-PLCSIM with the WinCC RT Simulator; each lab's Testing table marks which
> cases that session covered and which it did not.

## Hardware and software

| Item | Value |
|---|---|
| CPU | SIMATIC S7-1200, CPU 1211C AC/DC/Rly |
| Article number | `6ES7 211-1BE40-0XB0` |
| Firmware | V4.4 |
| Work memory | 50 KB |
| On-board I/O | DI6 × 24 VDC sink/source, DQ4 × relay, AI2 (0..10 V, %IW64) |
| Panel | SIMATIC KTP700 Basic PN, 7", 800 × 480 |
| Engineering software | TIA Portal V16 (STEP 7 Basic/Professional + WinCC Basic) |
| Simulation | S7-PLCSIM (optional; CPU firmware ≥ 4.0 required) |
| Programming language | LAD, all blocks |

## Repository layout

```
.
├── README.md                        this file
├── SCREENSHOT_CHECKLIST.md          capture log: what was shot, what is still open
├── LICENSE                          MIT
├── .gitignore                       TIA-aware; commit .zap, never the .ap16 folder
├── docs/
│   ├── setup.md                     retrieve, compile, download, simulate
│   └── extraction-notes.md          page maps, raw extraction, Findings list
├── labs/
│   ├── 01-alternating-lamp-timers/  Ex. 1.1 — two-timer oscillator
│   ├── 02-sequential-traffic-lights/Ex. 1.2 — three-step light sequence
│   ├── 03-counter-auto-stop/        Ex. 1.3 — CTU on a falling edge, RESET_BF
│   └── 04-water-tank-auto-manual/   Ex. 1.4 — auto/manual, FC split, CTUD
│       ├── README.md                objective, I/O, per-network walkthrough, tests
│       ├── img/                     ladder and HMI images, plus runtime captures
│       └── src/                     archived TIA project (.zap16) — see .gitignore
├── videos/                          one demo clip per lab (MP4)
└── assets/                          shared images
```

## Labs

| Lab | Title | Key concepts | Status | Link | Demo |
|---|---|---|---|---|---|
| 1 | Alternating lamp with adjustable ON/OFF times | `S`/`R` latch, two `TON`, `MUL`/`DIV` scaling, HMI I/O fields | Documented · tested on PLCSIM | [labs/01-…](labs/01-alternating-lamp-timers/README.md) | [16 s](videos/lab-01-demo.mp4) |
| 2 | Sequential three-lamp cycle | Timer chaining, ring sequence, three-lamp reset | Documented · tested on PLCSIM | [labs/02-…](labs/02-sequential-traffic-lights/README.md) | [25 s](videos/lab-02-demo.mp4) |
| 3 | Cycle counter with automatic stop | `N` edge scan, `CTU`, `RESET_BF`, self-resetting counter | Documented · tested on PLCSIM | [labs/03-…](labs/03-counter-auto-stop/README.md) | [32 s](videos/lab-03-demo.mp4) |
| 4 | Water tank — automatic and manual control | `FC` decomposition, non-optimized `DB`, `CTUD`, clock memory | Documented · tested on PLCSIM | [labs/04-…](labs/04-water-tank-auto-manual/README.md) | [85 s](videos/lab-04-demo.mp4) |

[`docs/extraction-notes.md#findings`](docs/extraction-notes.md#findings) lists
what did not add up while cross-checking the projects against the lab guide:
stale watch tables, inverted tag comments, mixed literal types, and one
unfinished "Extra" task in Lab 3.

## Getting started

1. Install TIA Portal (STEP 7 Basic/Professional + WinCC Basic) and, if you
   want to run without hardware, S7-PLCSIM.
2. Clone this repository.
3. In TIA Portal choose **Project → Retrieve**, pick the `.zap16` archive
   under `labs/<lab>/src/`, and select a destination folder.
4. **Compile** the PLC and the HMI (right-click device → Compile → Hardware
   and software (only changes)).
5. **Download to device** — either the physical CPU over PROFINET or PLCSIM.
   Download the HMI separately; choose *Overwrite all* if prompted.
6. Start the HMI runtime (`Start simulation`) or download to the panel.

Full walkthrough, including the IP settings and the PLCSIM caveats, is in
[`docs/setup.md`](docs/setup.md).

## Skills demonstrated

- Ladder logic: series/parallel branch construction, set/reset coils, normally
  open vs. normally closed contact selection, `RESET_BF` bit-field reset.
- Edge detection: positive (`P`) and negative (`N`) scan with an explicit
  edge-memory bit.
- IEC timers: `TON` instance data blocks, preset from a tag rather than a
  literal, elapsed-time read-back via `.ET`.
- IEC counters: `CTU` with a tag-driven preset and self-reset,
  `CTUD` used as a bidirectional integrator.
- Integer arithmetic for unit conversion: seconds ↔ milliseconds with `MUL`
  and `DIV` on `DInt`.
- Program structure: splitting a controller into `FC` blocks by concern
  (mode logic, output mapping, HMI/simulation) called from `OB1`.
- Data blocks: non-optimized `DB` with absolute offsets so HMI tags can bind
  to `%DB1.DBX…` addresses.
- CPU configuration: enabling and using the clock memory byte.
- HMI engineering: WinCC Basic screens, I/O fields bound to PLC tags,
  `SetBit`/`ResetBit`/`InvertBit` button events, symbol library graphics and
  a bar display.

## Author

**Hoàng Minh Khoa** — student ID 10223035
Industrial Automation Lab

## License

Released under the MIT License — see [`LICENSE`](LICENSE).

The lab exercises and the accompanying course guide are the property of the
course; this repository covers the implementation and the documentation
written for it.
