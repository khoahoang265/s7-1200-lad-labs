# Capture log

What was recorded, where it lives, and what a future session should still
shoot. Everything the TIA printouts already contain — ladder networks, tag
tables, HMI screen layouts — is rendered automatically into `labs/*/img/` and
is not listed here.

## Captured

All of it on **S7-PLCSIM** with the **WinCC RT Simulator**, TIA Portal V16.

| Lab | File | Shows |
|---|---|---|
| 1 | `labs/01-alternating-lamp-timers/img/hmi-runtime-on-phase.png` | ON phase — `LAMP` lit, `TIME ELAPSED · T1` = 2 of 5 s |
| 1 | `labs/01-alternating-lamp-timers/img/hmi-runtime-off-phase.png` | OFF phase — indicators swapped, `T2` counting |
| 2 | `labs/02-sequential-traffic-lights/img/hmi-runtime-green.png` | Green phase, with the project tree and PLCSIM in RUN |
| 2 | `labs/02-sequential-traffic-lights/img/hmi-runtime-yellow.png` | Yellow phase |
| 2 | `labs/02-sequential-traffic-lights/img/hmi-runtime-red.png` | Red phase |
| 3 | `labs/03-counter-auto-stop/img/hmi-runtime-lamp1.png` | Lamp 1 phase, `Count` = 0 |
| 3 | `labs/03-counter-auto-stop/img/hmi-runtime-lamp2.png` | Lamp 2 phase, count unchanged |
| 3 | `labs/03-counter-auto-stop/img/hmi-runtime-counting.png` | `Count` = 2 of `Counter Input` = 3 |
| 4 | `labs/04-water-tank-auto-manual/img/hmi-runtime-auto-filling.png` | Automatic mode, pump on, tank filling |
| 4 | `labs/04-water-tank-auto-manual/img/hmi-runtime-auto-dwell.png` | Tank at 100 %, pump off, `TIMER 1` counting |
| 4 | `labs/04-water-tank-auto-manual/img/hmi-runtime-manual.png` | Manual mode, pump by hand, automatic sequence stopped |

| Lab | Clip | Length | Covers |
|---|---|---|---|
| 1 | `videos/lab-01-demo.mp4` | 16 s | start, one full ON/OFF cycle, stop |
| 2 | `videos/lab-02-demo.mp4` | 25 s | start, green → yellow → red → green |
| 3 | `videos/lab-03-demo.mp4` | 32 s | three counted cycles, automatic stop, counter self-reset |
| 4 | `videos/lab-04-demo.mp4` | 85 s | full automatic cycle, then the switch to manual |

The clips are H.264 MP4, 1280 px wide, no audio, so GitHub plays them in the
browser. The original OBS `.mkv` masters are not in the repository.

## Still open

Not blocking anything — the Testing tables already mark these rows *not run*.

| # | Lab | What to capture | Where | Save as |
|---|---|---|---|---|
| 1 | 1–4 | Ladder under online monitoring, so the rungs can be seen carrying power | `Program blocks → <block>`, Monitoring on | `labs/<lab>/img/online-monitoring.png` |
| 2 | 1 | A setpoint changed while the cycle runs, taking effect on the next phase | RT Simulator | `labs/01-alternating-lamp-timers/img/hmi-runtime-setpoint-change.png` |
| 3 | 3 | STOP pressed mid-count, to document what `Count` does (finding F-3.3) | RT Simulator | `labs/03-counter-auto-stop/img/hmi-runtime-manual-stop.png` |
| 4 | 4 | Manual pump held on until the level hits 100 %, showing the interlock drop out | RT Simulator | `labs/04-water-tank-auto-manual/img/hmi-runtime-manual-interlock.png` |
| 5 | 1–4 | The same runs on the physical trainer rather than PLCSIM | lab bench | `labs/<lab>/img/hardware.png` |

Capture settings that matched the existing shots: TIA light theme, PNG,
cropped to the RT Simulator window — or wide enough to keep the PLCSIM panel
in frame when the point is that the CPU is in RUN.

## Reframing note

The four clips are full-desktop OBS recordings, so the HMI occupies a small
part of the frame. If they are ever re-recorded, setting the OBS source to
capture just the RT Simulator window — or cropping it — would make them much
easier to read at the size GitHub renders them.
