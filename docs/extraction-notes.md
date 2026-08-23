# Extraction notes

Everything in this repository was extracted from four TIA Portal project
printouts (`Microsoft: Print To PDF`) plus the course lab guide. This file
records what was parsed, how, and what did not add up.

- Text layer: `pdftotext -layout`, split on the form-feed character.
- Ladder topology: pages re-rendered with `pdftoppm` and read as images.
  Where text and image disagreed, the image won (rule: the text layer loses
  contact polarity and branch nesting).
- Repository images: `pdftoppm -png -r 200`, one crop per network.

## Source files

| PDF | Project title (`pdfinfo`) | Pages | Lab |
|---|---|---|---|
| `Lab1.1_ALL.pdf` | `LAb1.1` | 155 | Lab 1 — Extended Exercise 1.1 |
| `Lab1.2_ALL.pdf` | `LAb1.2` | 161 | Lab 2 — Extended Exercise 1.2 |
| `Lab1.3_ALL.pdf` | `Lab1.3` | 157 | Lab 3 — Extended Exercise 1.3 |
| `Lab1.4_ALL.pdf` | `Lab1.4` | 112 | Lab 4 — Extended Exercise 1.4 |
| `IA_FIle hướng dẫn.pdf` | — | 53 | Course lab guide (requirements) |

Grouping was taken from the file name (`Lab1.1` … `Lab1.4`); the four PDFs are
four separate TIA projects, not four parts of one project. A fifth TIA project
folder, `ET200P`, exists in the source directory but was never printed to PDF,
so it is not documented here.

## Page map

Section headers were located with the regex `^<ProjectTitle> / (.+)$` against
each page of the text layer, cross-checked with the table of contents on
pages 2–3.

### Lab1.1 (155 pages)

| Pages | Section | Action |
|---|---|---|
| 11–12 | `PLC_1 [CPU 1211C AC/DC/Rly] / Program blocks` | **rasterize + text** |
| 13–14 | `… / System blocks / Program resources` | text — Timer1 [DB3], Timer2 [DB4] |
| 17 | `… / PLC tags` | text — 10 tags |
| 19–20 | `… / Watch and force tables` | text — empty force table, Watch table_1 |
| 26–31 | `… / Local modules` | text — article numbers |
| 42 | `HMI_1 / Screens / Root screen` | **rasterize** — hardcopy |
| 67–69 | `HMI_1 / HMI tags` | text — 9 tags |
| 87–142 | `HMI_2 [KTP700 Basic PN]` (all) | **skipped, duplicate device** |

### Lab1.2 (161 pages)

| Pages | Section | Action |
|---|---|---|
| 11–12 | `… / Program blocks` | **rasterize + text** |
| 13–15 | `… / System blocks / Program resources` | text — Timer1 [DB3], Timer2 [DB4], Timer3 [DB1] |
| 18 | `… / PLC tags` | text — 14 tags |
| 20–21 | `… / Watch and force tables` | text |
| 27–32 | `… / Local modules` | text |
| 43 | `HMI_1 / Screens / Root screen` | **rasterize** |
| 70–73 | `HMI_1 / HMI tags` | text — 11 tags |
| 91–149 | `HMI_2 [KTP700 Basic PN]` (all) | **skipped, duplicate device** |

### Lab1.3 (157 pages)

| Pages | Section | Action |
|---|---|---|
| 11–12 | `… / Program blocks` | **rasterize + text** |
| 13–16 | `… / System blocks / Program resources` | text — Timer1 [DB2], Timer2 [DB1], COUNTER [DB3], RESET_DELAY [DB4] |
| 19 | `… / PLC tags` | text — 15 tags |
| 21 | `… / Watch and force tables` | text — **force table only, no watch table** |
| 27–32 | `… / Local modules` | text |
| 43 | `HMI_1 / Screens / Root screen` | **rasterize** |
| 68–70 | `HMI_1 / HMI tags` | text — 10 tags |
| 88–145 | `HMI_2 [KTP700 Basic PN]` (all) | **skipped, duplicate device** |

### Lab1.4 (112 pages)

| Pages | Section | Action |
|---|---|---|
| 11–17 | `… / Program blocks` | **rasterize + text** — OB1, FC1–FC4, DB1 |
| 18–20 | `… / System blocks / Program resources` | text — TIMER [DB2], COUNTER_DOWN [DB3], TIMER2 [DB4] |
| 23 | `… / PLC tags` | text — 16 tags (14 are CPU defaults) |
| 25–26 | `… / Watch and force tables` | text |
| 32–37 | `… / Local modules` | text |
| 48 | `HMI_1 / Screens / Root screen` | **rasterize** |
| 78–83 | `HMI_1 / HMI tags` | text — 20 tags |
| — | no second HMI device | — |

### Pages ignored

TIA boilerplate with zero information value, ignored in all four PDFs:
Technology objects, PLC data types (System data types), Traces, Measurements,
Combined measurements, OPC UA communication / Server interfaces, PLC alarm
text lists, HMI alarms (discrete, analog, groups, classes, system events),
Recipes, Historical data (Datalogs, AlarmLogs), Scheduled tasks, Text and
graphic lists, User administration, Screen management, Global screen,
Templates, Connections, Cross-device functions / Project traces, Common data,
Languages & resources / Project texts.

HMI screens ignored because they are stock TIA system screens, identical in
all four projects: `Different jobs`, `Project information`, `SIMATIC PLC
system diagnostics`, `System information`, `System screens`,
`User administration`, `Template_1`. Only `Root screen` carries the student's
own design.

**Ignore count:** the four PDFs total **585 pages**. **80** were read (17 in
Lab1.1, 19 in Lab1.2, 18 in Lab1.3, 26 in Lab1.4); **505 were ignored**
(86 %), of which **173** belong to the duplicate HMI_2 devices.

### The duplicate HMI devices

Labs 1.1, 1.2 and 1.3 each contain a second panel, `HMI_2 [KTP700 Basic PN]`,
on `HMI_Connection_4`. Its tag table was compared against HMI_1 before
skipping:

| Lab | HMI_1 tags | HMI_2 tags | Difference |
|---|---|---|---|
| 1.1 | 9 | 10 | HMI_2 adds `T1_INPUT_RESULT` |
| 1.2 | 11 | 14 | HMI_2 adds the three `*_INPUT_RESULT` tags |
| 1.3 | 10 | 15 | HMI_2 adds `SIG_SCAN`, `RESET_TAG`, `T1_RESULT`, `T2_RESULT`, `START_P` |

Same device type, same `Root screen`, same PLC connection target. HMI_2 is a
working copy that exposes the internal scratch tags as well. This repository
documents HMI_1 only.

## Hardware

Read from `… / Local modules`:

| Item | Value |
|---|---|
| Short designation | CPU 1211C AC/DC/Rly |
| Article number | `6ES7 211-1BE40-0XB0` |
| Firmware | V4.4 |
| Work memory | 50 KB |
| On-board I/O | DI6 × 24 VDC sink/source, DQ4 × relay, AI2 (0..10 V) |
| Digital input addresses | `%I0.0` – `%I0.5` |
| Analog input address | `%IW64` |
| Integration time | 50 Hz (20 ms) |
| HMI | KTP700 Basic PN, `HMI_Connection_1` |

The panel's article number does not appear anywhere in these PDFs — TIA prints
it for the CPU but not for the HMI device. The model designation `KTP700 Basic
PN` identifies the panel unambiguously, so the order number is not recorded in
this repository.

The TIA Portal version is not printed either. It is **V16**, confirmed by the
author and consistent with the `.ap16` project extension.

## Extracted logic

### Lab 1.1 — `Main [OB1]`, LAD, 4 networks

| NW | Rung | Coils |
|---|---|---|
| 1 | `START` %M0.2 (NO) | `S LAMP` %Q0.1, `R LAMP_1` %M0.0 |
| 1 | rail (unconditional) | `MUL Auto(DInt)`: %MD2 × 1000 → %MD6; %MD10 × 1000 → %MD14 |
| 2 | `LAMP` %Q0.1 (NO) → `TON Timer1` [DB3], PT = %MD6 | Q → `R LAMP`, `S LAMP_1` |
| 2 | rail | `DIV Auto(DInt)`: `Timer1.ET` ÷ 1000 → %MD18 `T1_HMI` |
| 3 | `LAMP_1` %M0.0 (NO) → `TON Timer2` [DB4], PT = %MD14 | Q → `S LAMP`, `R LAMP_1` |
| 3 | rail | `DIV Auto(DInt)`: `Timer2.ET` ÷ 1000 → %MD22 `T2_HMI` |
| 4 | `STOP` %M0.3 (NO) | `R LAMP`, `R LAMP_1` |

All contacts are normally open. No NC contact appears anywhere in Lab 1.1.

### Lab 1.2 — `Main [OB1]`, LAD, 5 networks

| NW | Title | Rung | Coils |
|---|---|---|---|
| 1 | Đèn Xanh | `START` %M0.2 (NO) | `S LAMP_1` %Q0.1, `R LAMP_2` %Q0.2, `R LAMP_3` %Q0.3 |
| 1 | | rail | `MUL`: %MD2 × **1000.0** → %MD6; %MD10 × 1000 → %MD14; %MD34 × 1000 → %MD26 |
| 2 | Đèn vàng | `LAMP_1` (NO) → `TON Timer1` [DB3], PT = %MD6 | Q → `R LAMP_1`, `S LAMP_2` |
| 2 | | rail | `DIV`: `Timer1.ET` ÷ 1000 → %MD18 |
| 3 | Đèn đỏ | `LAMP_2` (NO) → `TON Timer2` [DB4], PT = %MD14 | Q → `R LAMP_2`, `S LAMP_3` |
| 3 | | rail | `DIV`: `Timer2.ET` ÷ 1000 → %MD22 |
| 4 | Đèn đỏ | `LAMP_3` (NO) → `TON Timer3` [DB1], PT = %MD26 | Q → `R LAMP_3`, `S LAMP_1` |
| 4 | | rail | `DIV`: `Timer3.ET` ÷ 1000 → %MD30 |
| 5 | Stop | `STOP` %M0.3 (NO) | `R LAMP_1`, `R LAMP_2`, `R LAMP_3` |

All contacts normally open.

### Lab 1.3 — `Main [OB1]`, LAD, 5 networks

| NW | Title | Rung | Coils |
|---|---|---|---|
| 1 | Đèn 1 xanh | `START` %M0.3 **(P edge, mem `START_P` %M5.0)** | `S LAMP_1` %Q0.0, `R LAMP_2` %Q0.1 |
| 1 | | rail | `MUL`: %MD200 × 1000 → %MD6; %MD10 × 1000 → %MD14 |
| 2 | Timer1: L1 → L2 | `LAMP_1` (NO) → `TON Timer1` [DB2], PT = %MD6 | Q → `R LAMP_1`, `S LAMP_2` |
| 2 | | rail | `DIV`: `Timer1.ET` ÷ 1000 → %MD18 |
| 3 | Timer2: L2 tắt → L1 bật lại | `LAMP_2` (NO) → `TON Timer2` [DB1], PT = %MD14 | Q → `R LAMP_2`, `S LAMP_1` |
| 3 | | rail | `DIV`: `Timer2.ET` ÷ 1000 → %MD22 |
| 4 | CTU đếm sườn âm L2 + Auto Stop | `LAMP_2` **(N edge, mem `SIG_SCAN` %M0.2)** → `CTU Int` [DB3 `COUNTER`], R = `RESET_TAG` %M0.1, PV = `COUNTER_INPUT` %MW100, CV = `COUNTER_DISPLAY` %MW26 | Q → `TON RESET_DELAY` [DB4], PT = T#15 ms → `RESET_TAG` %M0.1 (normal coil) |
| 5 | STOP | `"COUNTER".QU` (NO) **OR** `STOP` %M0.0 (NO) | `RESET_BF LAMP_1` %Q0.0, 2 bits |

### Lab 1.4 — `Main [OB1]` + FC1–FC4 + DB1, LAD

`OB1` contains four unconditional calls: `AUTOMATIC_MODE [FC1]`,
`MANUAL_MODE [FC2]`, `MAP OUTPUT [FC3]`, `HMI Display [FC4]`.

**FC1 `AUTOMATIC_MODE`, Network 1** — seven rungs, all operands in
`"DATA_BLOCK"` [DB1]:

| Rung | Series contacts | Coils |
|---|---|---|
| 1 | `AUTOMATIC_START` (NO) ∥ `AUTOMATIC_RUN` (NO) — `AUTOMATIC_STOP` (**NC**) — `AUTOMATIC_MANUAL` (NO) | `AUTOMATIC_RUN` (normal coil) |
| 2 | `AUTOMATIC_RUN` (**P**, mem `POSITIVE_SIGNAL_EDGE`) — `LOW` (**NC**) | `S AUTOMATIC_PUMP`, `R AUTOMATIC_VALVE` |
| 3 | (same P-edge junction) — `LOW` (NO) | `S AUTOMATIC_VALVE`, `R AUTOMATIC_PUMP` |
| 4 | `HIGH` (NO) | `R AUTOMATIC_PUMP` |
| 5 | `AUTOMATIC_RUN` (NO) — `HIGH` (NO) → `TON "TIMER"` [DB2], PT = T#5000 ms | Q → `S AUTOMATIC_VALVE`; `DIV "TIMER".ET ÷ 1000 → TIMER1_DISPLAY` |
| 6 | `LOW` (**NC**) | `R AUTOMATIC_VALVE` |
| 7 | `AUTOMATIC_RUN` (NO) — `LOW` (**NC**) → `TON "TIMER2"` [DB4], PT = T#5000 ms | Q → `S AUTOMATIC_PUMP`; `DIV "TIMER2".ET ÷ 1000 → TIMER2_DISPLAY` |

**FC2 `MANUAL_MODE`, Network 1** — two rungs:

| Rung | Series contacts | Coil |
|---|---|---|
| 1 | `MANUAL_PUMP_START` (NO) ∥ `MANUAL_PUMP` (NO) — `MANUAL_PUMP_STOP` (**NC**) — `AUTOMATIC_MANUAL` (**NC**) — `HIGH` (**NC**) | `MANUAL_PUMP` |
| 2 | `MANUAL_VALVE_START` (NO) ∥ `MANUAL_VALVE` (NO) — `MANUAL_VALVE_STOP` (**NC**) — `AUTOMATIC_MANUAL` (**NC**) — `LOW` (NO) | `MANUAL_VALVE` |

**FC3 `MAP OUTPUT`, Network 1** — two rungs:

| Rung | Logic | Coil |
|---|---|---|
| 1 | (`AUTOMATIC_PUMP` — `AUTOMATIC_RUN`) ∥ `MANUAL_PUMP` | `PUMP` %Q0.0 |
| 2 | (`AUTOMATIC_VALVE` — `AUTOMATIC_RUN`) ∥ `MANUAL_VALVE` | `VALVE` %Q0.1 |

**FC4 `HMI Display`, Network 1** — the tank simulator:

| Rung | Logic | Result |
|---|---|---|
| 1 | `PUMP` %Q0.0 — `Clock_5Hz` %M0.1 — `WATER_LEVEL <= 100 (DInt)` → `CU` of `CTUD Int` [DB3 `COUNTER_DOWN`] | `CV` → `WATER_LEVEL` (DB1.DBD12), PV = 100, R = false, LD = false |
| 2 | `VALVE` %Q0.1 — `Clock_5Hz` — `WATER_LEVEL > 0 (DInt)` → `CD` | same counter |
| 3 | `WATER_LEVEL >= 100 (DInt)` | `HIGH` |
| 4 | `WATER_LEVEL > 0 (DInt)` | `LOW` |

## Tag tables

### Lab 1.1 — `Default tag table [39]`, 10 tags, **no comments**

| Name | Data type | Address |
|---|---|---|
| LAMP | Bool | %Q0.1 |
| LAMP_1 | Bool | %M0.0 |
| START | Bool | %M0.2 |
| STOP | Bool | %M0.3 |
| T1_HMI | DInt | %MD18 |
| T1_INPUT | DInt | %MD2 |
| T1_INPUT_RESULT | DInt | %MD6 |
| T2_HMI | DInt | %MD22 |
| T2_INPUT | DInt | %MD10 |
| T2_INPUT_RESULT | DInt | %MD14 |

### Lab 1.2 — `Default tag table [43]`, 14 tags

| Name | Data type | Address | Comment (as printed) |
|---|---|---|---|
| LAMP_1 | Bool | %Q0.1 | Đèn Xanh |
| LAMP_2 | Bool | %Q0.2 | Đèn Vàng |
| LAMP_3 | Bool | %Q0.3 | Đèn ĐỎ |
| START | Bool | %M0.2 | |
| STOP | Bool | %M0.3 | |
| T1_HMI | DInt | %MD18 | Hiển thị T1 Elapsed |
| T1_INPUT | DInt | %MD2 | Nhập T1 từ HMI |
| T1_INPUT_RESULT | DInt | %MD6 | T1x1000ms |
| T2_HMI | DInt | %MD22 | Hiển thị T2 Elapsed |
| T2_INPUT | DInt | %MD10 | Nhập T2 từ HMI |
| T2_INPUT_RESULT | DInt | %MD14 | T2x1000ms |
| T3_HMI | DInt | %MD30 | Hiển thị T3 Elapsed |
| T3_INPUT | DInt | %MD34 | Nhập T3 từ HMI |
| T3_INPUT_RESULT | DInt | %MD26 | T3x1000ms |

### Lab 1.3 — `Default tag table [44]`, 15 tags

| Name | Data type | Address | Comment (as printed) |
|---|---|---|---|
| COUNTER_DISPLAY | Int | %MW26 | Giá trị đếm CV |
| COUNTER_INPUT | Int | %MW100 | Giới hạn N |
| LAMP_1 | Bool | %Q0.0 | Đèn 1 xanh |
| LAMP_2 | Bool | %Q0.1 | Đèn 2 vàng |
| RESET_TAG | Bool | %M0.1 | Tự reset CTU |
| SIG_SCAN | Bool | %M0.2 | Bộ nhớ scan L2 |
| START | Bool | %M0.3 | Nút START |
| START_P | Bool | %M5.0 | |
| STOP | Bool | %M0.0 | Nút STOP |
| T1_HMI | DInt | %MD18 | Hiển thị T1 elapsed |
| T1_INPUT | DInt | %MD200 | Thời gian T1 nhập |
| T1_RESULT | DInt | %MD6 | T1 × 1000 ms |
| T2_HMI | DInt | %MD22 | Hiển thị T2 elapsed |
| T2_INPUT | DInt | %MD10 | Thời gian T2 nhập |
| T2_RESULT | DInt | %MD14 | T2 × 1000 ms |

### Lab 1.4 — `Default tag table [45]`

Only two application tags; the other fourteen are the CPU clock/system byte
defaults (`Clock_Byte` %MB0, `System_Byte` %MB1 and their bits).

| Name | Data type | Address |
|---|---|---|
| PUMP | Bool | %Q0.0 |
| VALVE | Bool | %Q0.1 |

`DATA_BLOCK [DB1]`, non-optimized, 18 static members:

| Name | Data type | Offset |
|---|---|---|
| AUTOMATIC_MANUAL | Bool | 0.0 |
| AUTOMATIC_START | Bool | 0.1 |
| AUTOMATIC_STOP | Bool | 0.2 |
| AUTOMATIC_RUN | Bool | 0.3 |
| AUTOMATIC_PUMP | Bool | 0.4 |
| AUTOMATIC_VALVE | Bool | 0.5 |
| HIGH | Bool | 0.6 |
| LOW | Bool | 0.7 |
| POSITIVE_SIGNAL_EDGE | Bool | 1.0 |
| TIMER1_DISPLAY | DInt | 2.0 |
| TIMER2_DISPLAY | DInt | 6.0 |
| MANUAL_PUMP_START | Bool | 10.0 |
| MANUAL_PUMP_STOP | Bool | 10.1 |
| MANUAL_PUMP | Bool | 10.2 |
| MANUAL_VALVE_START | Bool | 10.3 |
| MANUAL_VALVE_STOP | Bool | 10.4 |
| MANUAL_VALVE | Bool | 10.5 |
| WATER_LEVEL | DInt | 12.0 |

## Watch and force tables

Every force table in all four projects is empty.

**Lab 1.1 — `Watch table_1`**

| Name | Address | Format | Modify value | Comment (as printed) |
|---|---|---|---|---|
| START | %M0.2 | Bool | TRUE | Giả lập nút START |
| STOP | %M0.3 | Bool | | Giả lập nút STOP |
| LAMP | %Q0.1 | Bool | | Xem đèn ON/OFF |
| T1_HMI | %MD18 | DEC+/- | | Nhập thời gian T1 (giây) |
| T2_HMI | %MD22 | DEC+/- | | Nhập thời gian T2 (giây) |
| T1_INPUT | %MD2 | DEC+/- | 5 | Xem thời gian đếm T1 |
| T2_INPUT | %MD10 | DEC+/- | 3 | Xem thời gian đếm T2 |

**Lab 1.2 — `Watch table_1`**: byte-for-byte identical to Lab 1.1's, including
the `"LAMP" %Q0.1` row that no longer matches this project's tag names.

**Lab 1.3**: no watch table exists — only an empty force table.

**Lab 1.4 — `Watch table_1`**

| Name | Address | Format | Modify value |
|---|---|---|---|
| "DATA_BLOCK".AUTOMATIC_MANUAL | %DB1.DBX0.0 | Bool | |
| "DATA_BLOCK".AUTOMATIC_START | %DB1.DBX0.1 | Bool | TRUE |
| "DATA_BLOCK".AUTOMATIC_RUN | %DB1.DBX0.3 | Bool | |
| "DATA_BLOCK".AUTOMATIC_STOP | %DB1.DBX0.2 | Bool | FALSE |
| "DATA_BLOCK".AUTOMATIC_PUMP | %DB1.DBX0.4 | Bool | |
| "DATA_BLOCK".AUTOMATIC_VALVE | %DB1.DBX0.5 | Bool | TRUE |
| "DATA_BLOCK".HIGH | %DB1.DBX0.6 | Bool | |
| "DATA_BLOCK".LOW | %DB1.DBX0.7 | Bool | |
| "DATA_BLOCK".WATER_LEVEL | %DB1.DBD12 | DEC+/- | |
| "PUMP" | %Q0.0 | Bool | |
| "VALVE" | %Q0.1 | Bool | |
| "DATA_BLOCK".POSITIVE_SIGNAL_EDGE | %DB1.DBX1.0 | Bool | FALSE |

## Findings

Discrepancies between the projects, the tag/watch comments and the lab guide.
Nothing here has been corrected — this is the fix list.

### Affects every lab

- **F-0.1 — No physical inputs anywhere.** All four projects drive `START`,
  `STOP` and the mode switches from memory bits (`%M`) or DB bits, never from
  `%I`. The lab guide states this explicitly: *"if Memory bits are used,
  especially the inputs, the user won't be able to modify the outputs using
  the physical input switch panel."* Demonstrating on the kit's switch panel
  requires rewiring these tags to `%I0.x` first.
- **F-0.2 — Comment coverage is inconsistent.** Lab 1.1 and Lab 1.4 have no
  PLC tag comments at all; Labs 1.2 and 1.3 are commented in Vietnamese.
- **F-0.3 — Duplicate HMI device** in Labs 1.1–1.3 (see table above). Decide
  whether HMI_2 is intentional; if not, delete it before archiving, because
  it doubles the printout and the compile time.

### Lab 1.1

- **F-1.1 — Watch table comments are inverted.** `T1_HMI`/`T2_HMI` are the
  `DIV` outputs (elapsed time, read-only) but are commented *"Nhập thời gian
  T1/T2 (giây)"* — enter the time. `T1_INPUT`/`T2_INPUT` are the operator
  setpoints but are commented *"Xem thời gian đếm"* — view the elapsed time.
  The two pairs of comments need to be swapped.
- **F-1.2 — `LAMP_1` %M0.0 is not a lamp.** It is the OFF-phase state bit that
  drives Timer2. It is exported to the HMI as if it were an output. Rename to
  something like `LAMP_OFF_PHASE`.
- **F-1.3 — HMI tag list is asymmetric.** HMI_1 exports `T2_INPUT_RESULT` but
  not `T1_INPUT_RESULT`; neither is needed on the panel (both are internal
  millisecond values).
- **F-1.4 — Lab guide asks for a "Counter for time elapsed for each Output".**
  The project implements this with `DIV` on `Timer.ET`, not with a counter
  instruction. Reasonable, but note it if the grading expects a CTU.

### Lab 1.2

- **F-2.1 — Mixed literal types in Network 1.** The first `MUL Auto (DInt)`
  has `IN2 = 1000.0` (a Real literal) while the other two use `1000`. Open the
  network in TIA and confirm it compiles clean; make all three `1000`.
- **F-2.2 — Watch table was never updated from Lab 1.1.** It still watches
  `"LAMP" %Q0.1`, a tag that does not exist in this project (%Q0.1 is
  `LAMP_1` here), and it has no rows for T3.
- **F-2.3 — Address block for T3 breaks the pattern.** T1 and T2 use
  input→result ascending (%MD2→%MD6, %MD10→%MD14) but T3 uses %MD34→%MD26.
  Functional, but a maintenance trap.
- **F-2.4 — Lamp colours cannot be verified from the PDF.** The guide asks
  for green/yellow/red; the tag comments agree, but the hardcopy renders three
  identical grey circles because appearance animation is not printed. Confirm
  on the panel.

### Lab 1.3

- **F-3.1 — No watch table at all.** Nothing to seed test setpoints from. The
  Testing table in `labs/03-…/README.md` uses the lab guide's values instead
  and is marked TODO.
- **F-3.2 — Counter data type disagrees with the guide.** The guide says the
  CTU is *"currently DInt"*; the project instantiates `CTU Int` with
  `PV = %MW100` and `CV = %MW26` (both Int). The project is self-consistent;
  the guide text is what is stale.
- **F-3.3 — STOP does not clear the counter.** The guide's "Extra" asks for
  the counter output to be reset after pressing STOP. Network 5 only executes
  `RESET_BF LAMP_1, 2`; `COUNTER` is reset solely by `RESET_TAG`, which is
  pulsed when `QU` goes true. Pressing STOP mid-count leaves `COUNTER_DISPLAY`
  at its current value.
- **F-3.4 — `T1_INPUT` at %MD200** sits far outside the %MD2–%MD26 block used
  by everything else. Almost certainly a typo for %MD2; harmless today but it
  will collide with nothing and confuse everyone.
- **F-3.5 — `RESET_DELAY` PT = T#15 ms** is shorter than a typical scan
  budget. It works as a one-shot because `RESET_TAG` clears `COUNTER` on the
  next scan, but it is timing-fragile. A `P` edge on `"COUNTER".QU` would be
  the intent-revealing construct.

### Lab 1.4

- **F-4.2 — CTUD type vs. tag type.** `COUNTER_DOWN` [DB3] is instantiated as
  `CTUD Int`, but its `CV` output is wired to `"DATA_BLOCK".WATER_LEVEL`,
  declared `DInt` at offset 12.0, and the comparators in the same network
  compare it as `DInt`. Verify this compiles; if it does not, either make the
  CTUD `DInt` or make `WATER_LEVEL` an `Int`.
- **F-4.3 — The water level is simulated, not measured.** `WATER_LEVEL` is
  counted up and down by `Clock_5Hz` while the pump/valve output is on, and
  `HIGH`/`LOW` are derived from it by comparison. There are no sensor inputs.
  The guide permits this ("if sensors aren't available due to software
  limitation, lights can be used instead"), but the README must say so rather
  than imply real instrumentation.
- **F-4.4 — Timer presets are hard-coded.** Both TONs use `T#5000MS` in the
  ladder while `TIMER1_DISPLAY`/`TIMER2_DISPLAY` are exported to the HMI,
  which suggests to the operator that the delay is adjustable. It is not.
- **F-4.5 — `MAP OUTPUT [FC3]` has a space in its name**, unlike every other
  block. Rename to `MAP_OUTPUT`.
- **F-4.6 — Watch table modifies an edge-memory bit.**
  `POSITIVE_SIGNAL_EDGE` is listed with modify value FALSE. Writing an edge
  memory bit by hand is a debugging trick, not a test case; it is carried into
  the Testing table flagged as such.

### Checked and cleared

Things that looked wrong on a first pass and turned out to be correct. Kept
here so nobody re-opens them.

- **F-4.1 (withdrawn) — the mode switch does select a mode.** An early reading
  of this extraction recorded `AUTOMATIC_MANUAL` as a normally closed contact
  in FC1 rung 1, which would have made automatic and manual mutually
  *inclusive*. Re-reading the page render at 200 DPI shows the FC1 contact is
  normally **open** while both FC2 contacts are normally **closed**, so the
  polarities are already opposite and the switch works as intended:

  | `AUTOMATIC_MANUAL` | FC1 rung 1 (NO) | FC2 rungs (NC) | Mode |
  |---|---|---|---|
  | 1 | conducts | blocked | Automatic |
  | 0 | blocked | conducts | Manual |

  This also matches the lab guide, which says `AUTOMATIC_RUN` is energised
  when *"AUTOMATIC_MANUAL is closed (normally open switch)"*. No change is
  needed. The lesson for the extraction procedure: contact polarity must be
  read from a render of at least 200 DPI — at 110 DPI the diagonal stroke of
  an NC contact is a two-pixel artefact and is easy to hallucinate. Every
  contact in Labs 1.1–1.3 was re-checked at 200 DPI at the same time; all of
  them are normally open, as recorded.
