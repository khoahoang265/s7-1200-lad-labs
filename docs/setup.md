# Setup

How to get any of the four labs running, on real hardware or in simulation.

## 1. Software

| Component | Needed for |
|---|---|
| TIA Portal — STEP 7 Basic or Professional | the PLC program |
| TIA Portal — WinCC Basic | the KTP700 panel (WinCC Basic covers Basic Panels) |
| S7-PLCSIM | optional; running without a CPU |

> ⚠️ TODO — confirm the TIA Portal version. The projects on disk use the
> `.ap16` extension, which is TIA Portal V16. A project can be opened by a
> newer TIA Portal (it will upgrade it, irreversibly) but never by an older
> one, so record the version you used.

PLCSIM only works with S7-1200 CPUs whose firmware is **V4.0 or newer**. The
CPU in these projects is FW V4.4, so simulation is available.

## 2. Retrieve the project

The repository stores each lab as a TIA archive, not as a live project folder
(see the comment block at the top of `.gitignore` for why).

1. **Project → Retrieve**
2. Choose `labs/<lab>/src/<name>.zap16`
3. Choose a target directory — somewhere short and local, e.g.
   `C:\TIA\<lab>`. Deep paths under OneDrive or a network share cause
   intermittent compile failures.
4. TIA opens the retrieved project automatically.

## 3. Check the hardware configuration

Open **Device configuration** for `PLC_1` and confirm the CPU matches the one
physically in front of you:

| Property | Value in these projects |
|---|---|
| Model | CPU 1211C AC/DC/Rly |
| Article number | `6ES7 211-1BE40-0XB0` |
| Firmware | V4.4 |

If the trainer has a different CPU, right-click the device → **Change device**
and pick the correct order number. Do not simply lower the firmware version —
the I/O addresses may shift.

**Lab 4 additionally needs clock memory.** Open `PLC_1` → Properties →
**General → System and clock memory** and confirm *Enable the use of clock
memory byte* is ticked with `%MB0`. Without it `Clock_5Hz` never toggles and
the tank level never moves.

**Lab 4 also needs a non-optimized data block.** `DATA_BLOCK [DB1]` →
Properties → Attributes → *Optimized block access* must be **unticked**, so
its members keep absolute addresses like `%DB1.DBX0.3` that the HMI can bind
to. If you re-create the DB, do this before adding any members.

## 4. Set the network addresses

Both devices talk PROFINET over the same subnet.

1. `PLC_1` → Properties → **PROFINET interface [X1] → Ethernet addresses** —
   set an IP such as `192.168.0.1 / 255.255.255.0`.
2. `HMI_1` → the same dialog — set e.g. `192.168.0.2 / 255.255.255.0`.
3. Open **Devices & networks** and confirm the green `HMI_Connection_1` link
   between the panel and the CPU is present.
4. Set your PC's Ethernet adapter to a third address on that subnet, e.g.
   `192.168.0.100`.

## 5. Compile

Right-click `PLC_1` → **Compile → Hardware and software (only changes)**, then
do the same for `HMI_1`. Both must finish with 0 errors before downloading.

Two of these projects have known compile-time risks — check the Info →
Compile tab carefully:

- **Lab 2** has `IN2 = 1000.0` on one `MUL Auto (DInt)` box (finding F-2.1).
- **Lab 4** wires an `Int` `CTUD` output to a `DInt` tag (finding F-4.2).

## 6. Download to the CPU

1. Put the CPU in **STOP** (or let TIA do it when it prompts).
2. Right-click `PLC_1` → **Download to device → Hardware and software**.
3. Choose *PN/IE* as the interface, pick your adapter, **Start search**,
   select the CPU, **Load**.
4. In the Load results dialog set the action to *Start module*, then **Finish**.

## 7. Download to the panel

Right-click `HMI_1` → **Download to device**. Choose **Overwrite all** if
asked. On a Basic Panel the first download also transfers the runtime, which
takes noticeably longer than later ones.

To run without the panel: right-click `HMI_1` → **Start simulation**. The
simulated runtime connects to whatever the connection points at — a real CPU
or PLCSIM.

## 8. Running under PLCSIM instead

1. Right-click `PLC_1` → **Start simulation**. TIA opens S7-PLCSIM and asks to
   compile.
2. Download to the simulated CPU exactly as in step 6 — the interface will be
   `PLCSIM` rather than your Ethernet adapter.
3. Set the simulated CPU to **RUN**.
4. Start the HMI simulation. It will bind to PLCSIM automatically.

## 9. Driving the program

All four projects command the PLC from memory bits or DB bits, not from `%I`
inputs (finding F-0.1). That means:

- The trainer's physical input switch panel **will not** start or stop these
  programs as they stand.
- Use the HMI (real or simulated), or a watch table with *Modify value*.
- To use the physical switches, rewire `START`/`STOP` — and, in Lab 4, the
  mode and manual buttons — to `%I0.0`–`%I0.5` and download again.

To drive from a watch table: open `PLC_1` → **Watch and force tables →
Watch table_1**, click *Monitor all*, then type into the *Modify value* column
and press the "Modify now" button. Labs 1, 2 and 4 ship with a watch table;
Lab 3 does not (finding F-3.1) — create one from the tag list in that lab's
README.

## 10. Archiving your changes back

**Project → Archive**, tick *Discard restorable data*, and save the `.zap16`
over the one in `labs/<lab>/src/`. Commit that single file. Never `git add`
the retrieved project folder — `.gitignore` blocks it, and if you force it
past the filter the repository becomes unusable within a few commits.
