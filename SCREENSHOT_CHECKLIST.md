# Screenshot checklist

Everything the TIA printouts already contain — ladder networks, tag tables,
HMI screen hardcopies — has been rendered into `labs/*/img/` automatically.
This file lists only what the PDFs **cannot** provide: anything that requires
the program to be running.

Save each capture at the exact path in the last column, spelled exactly as
written — the paths are already chosen so that adding

```markdown
![Online monitoring — ON phase](img/online-monitoring.png)
```

to the matching lab README works with no renaming. Nothing in the READMEs
points at these files yet, so an un-captured item leaves no broken image.

## Capture settings

- TIA Portal in the **light** theme.
- Editor zoom **125–150 %** so contact labels stay readable when the image is
  scaled down in a browser.
- **PNG**, cropped tight to the region of interest — no desktop, no taskbar,
  no empty editor margin.
- For online views, wait until the green "connected" bar is steady before
  capturing, so the monitoring colours are unambiguous.

## Lab 1 — Alternating lamp with adjustable ON/OFF times

| # | Lab | What to capture | Where in TIA Portal | Save as |
|---|---|---|---|---|
| 1 | 1 | Networks 2–3 under online monitoring, mid ON phase: `LAMP` energised, `Timer1.ET` counting | `PLC_1 → Program blocks → Main [OB1]`, Monitoring on | `labs/01-alternating-lamp-timers/img/online-monitoring.png` |
| 2 | 1 | HMI runtime with live values: T1 = 5, T2 = 3 entered, one indicator lit, elapsed field counting | `HMI_1 → Start simulation` (or the panel) | `labs/01-alternating-lamp-timers/img/hmi-runtime.png` |
| 3 | 1 | Compile result with 0 errors / 0 warnings **and** "Download completed without error" | Info → Compile tab, then the Load results dialog | `labs/01-alternating-lamp-timers/img/compile-download.png` |
| 4 | 1 | PLCSIM or the physical kit running — lamp visibly on | S7-PLCSIM front panel, or a photo of the trainer | `labs/01-alternating-lamp-timers/img/hardware.png` |

## Lab 2 — Sequential three-lamp cycle

| # | Lab | What to capture | Where in TIA Portal | Save as |
|---|---|---|---|---|
| 5 | 2 | Networks 2–4 online, mid yellow phase: `LAMP_2` energised, `Timer2` counting | `Main [OB1]`, Monitoring on | `labs/02-sequential-traffic-lights/img/online-monitoring.png` |
| 6 | 2 | HMI runtime with T1/T2/T3 entered and exactly one indicator lit — in the phase whose colour is being demonstrated | HMI simulation or the panel | `labs/02-sequential-traffic-lights/img/hmi-runtime.png` |
| 7 | 2 | Compile 0/0 and download completed | Info → Compile, Load results | `labs/02-sequential-traffic-lights/img/compile-download.png` |
| 8 | 2 | PLCSIM or the kit with the three lamps wired to %Q0.1–%Q0.3 | PLCSIM / photo | `labs/02-sequential-traffic-lights/img/hardware.png` |

## Lab 3 — Cycle counter with automatic stop

| # | Lab | What to capture | Where in TIA Portal | Save as |
|---|---|---|---|---|
| 9 | 3 | Network 4 online with a non-zero count: `COUNTER_DISPLAY` part-way to `COUNTER_INPUT` | `Main [OB1]`, Monitoring on | `labs/03-counter-auto-stop/img/online-monitoring.png` |
| 10 | 3 | Network 5 online at the instant of auto-stop: `"COUNTER".QU` true, both lamps cleared | `Main [OB1]`, Monitoring on | `labs/03-counter-auto-stop/img/online-autostop.png` |
| 11 | 3 | HMI runtime showing Counter Input = N and Count climbing | HMI simulation or the panel | `labs/03-counter-auto-stop/img/hmi-runtime.png` |
| 12 | 3 | Compile 0/0 and download completed | Info → Compile, Load results | `labs/03-counter-auto-stop/img/compile-download.png` |
| 13 | 3 | PLCSIM or the kit mid-run | PLCSIM / photo | `labs/03-counter-auto-stop/img/hardware.png` |

> Lab 3 has no watch table in the project. If you build one while testing,
> capture it too and save it as
> `labs/03-counter-auto-stop/img/watch-table.png` — it is the cheapest
> evidence for test rows 4–7.

## Lab 4 — Water tank, automatic and manual control

| # | Lab | What to capture | Where in TIA Portal | Save as |
|---|---|---|---|---|
| 14 | 4 | FC1 online during the 5 s dwell at 100 %: `HIGH` true, `"TIMER"` counting, pump off | `PLC_1 → Program blocks → AUTOMATIC_MODE [FC1]`, Monitoring on | `labs/04-water-tank-auto-manual/img/online-monitoring.png` |
| 15 | 4 | FC4 online with the level mid-travel: `WATER_LEVEL` between 0 and 100, one comparator true | `HMI Display [FC4]`, Monitoring on | `labs/04-water-tank-auto-manual/img/online-level.png` |
| 16 | 4 | HMI runtime, automatic mode: tank bar part full, pump lamp on, TIMER 1 field counting | HMI simulation or the panel | `labs/04-water-tank-auto-manual/img/hmi-runtime-auto.png` |
| 17 | 4 | HMI runtime, manual mode: mode switch set to Manual, valve driven by hand | HMI simulation or the panel | `labs/04-water-tank-auto-manual/img/hmi-runtime-manual.png` |
| 18 | 4 | Compile 0/0 and download completed | Info → Compile, Load results | `labs/04-water-tank-auto-manual/img/compile-download.png` |
| 19 | 4 | PLCSIM or the kit mid-cycle | PLCSIM / photo | `labs/04-water-tank-auto-manual/img/hardware.png` |

> For capture 17, remember the polarity: `AUTOMATIC_MANUAL = 0` is manual mode,
> `= 1` is automatic. Frame the shot so both mode lamps are visible — it is the
> clearest single image proving the two modes are mutually exclusive.

## Demo video shot list

Record each lab separately. Keep the whole clip under two minutes; the README
carries the detail, the video only has to prove it runs. Upload unlisted and
paste the URL into the lab's `## Demo` section.

### Lab 1 — target ≈ 1:10

| Time | Shot |
|---|---|
| 0:00–0:10 | Title card: lab number, title, CPU and panel model |
| 0:10–0:25 | TIA project tree, then `Main [OB1]` scrolled through its four networks |
| 0:25–0:40 | Enter T1 = 5 and T2 = 3 on the panel, press Start |
| 0:40–1:00 | Split view: ladder online monitoring beside the panel, through one full ON/OFF cycle |
| 1:00–1:10 | Press Stop; lamp dark, both timers at zero |

### Lab 2 — target ≈ 1:20

| Time | Shot |
|---|---|
| 0:00–0:10 | Title card |
| 0:10–0:20 | Networks 2, 3 and 4 side by side — say the phrase "each timer's coil is the next phase's enable" |
| 0:20–0:35 | Enter T1/T2/T3, press Start |
| 0:35–1:05 | One complete green → yellow → red → green ring on the panel, uncut |
| 1:05–1:20 | Press Stop mid-cycle, then Start again to show it re-enters at green |

### Lab 3 — target ≈ 1:30

| Time | Shot |
|---|---|
| 0:00–0:10 | Title card |
| 0:10–0:25 | Network 4 close-up: the `N` contact and the `CTU`, explaining the falling-edge count |
| 0:25–0:35 | Enter Counter Input = 3 and the two times, press Start |
| 0:35–1:10 | Let all three cycles run; the Count field visibly increments once per cycle |
| 1:10–1:25 | Auto-stop: both lamps clear, count returns to 0. Hold on Network 5 |
| 1:25–1:30 | Restart to show it begins from lamp 1 |

### Lab 4 — target ≈ 1:50

| Time | Shot |
|---|---|
| 0:00–0:10 | Title card |
| 0:10–0:25 | OB1's four FC calls, then one sentence per block on what it owns |
| 0:25–0:35 | Automatic mode, tank empty, press START |
| 0:35–1:00 | Fill to 100 %: bar rising, pump lamp on, then the 5 s dwell counting |
| 1:00–1:20 | Drain to 0 %: valve lamp on, bar falling, then the second dwell — cycle restarts |
| 1:20–1:30 | Press STOP; both actuators drop out |
| 1:30–1:45 | Switch to Manual; drive the pump by hand, show the 100 % interlock dropping it out |
| 1:45–1:50 | Closing card: link back to the repository |
