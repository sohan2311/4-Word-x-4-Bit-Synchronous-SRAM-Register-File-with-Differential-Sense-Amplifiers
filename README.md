# 4-Word x 4-Bit Synchronous SRAM Register File with Differential Sense Amplifiers

[![Report](https://img.shields.io/badge/Report-sram__calculations.pdf%20(10%20Pages)-blueviolet.svg)](./sram_calculations.pdf)
[![Cadence Virtuoso](https://img.shields.io/badge/EDA-Cadence%20Virtuoso%20IC6.1-blue.svg)](https://www.cadence.com)
[![PDK](https://img.shields.io/badge/PDK-GPDK%20180nm-green.svg)](https://www.cadence.com)
[![Supply Voltage](https://img.shields.io/badge/VDD-1.8V-orange.svg)]()
[![Transistor Count](https://img.shields.io/badge/Transistors-208%20Total%20(114%20NMOS%20%2F%2094%20PMOS)-blueviolet.svg)]()
[![DRC](https://img.shields.io/badge/Assura%20DRC-Clean%20(0%20Violations)-success.svg)]()
[![LVS](https://img.shields.io/badge/Assura%20LVS-100%25%20Matched-success.svg)]()
[![Parasitic Extraction](https://img.shields.io/badge/RC%20Extraction-AV%20Extracted%20Clean-success.svg)]()
[![GDSII Stream File](https://img.shields.io/badge/GDSII-sram__final__4__bit.gds%20(30.7KB)-blue.svg)](./sram_final_4_bit.gds)
[![Read Delay](https://img.shields.io/badge/tpd(read)-185.9%20ps-yellow.svg)]()
[![Total Power](https://img.shields.io/badge/Pavg-79.71%20%CE%BCW%20%40%20100MHz-red.svg)]()
[![Status](https://img.shields.io/badge/Verification-Fully%20Verified%20(Pre%20%26%20Post--Layout)-brightgreen.svg)]()

A high-performance, full custom **4-Word x 4-Bit (16-Bit) Synchronous Static Random-Access Memory (SRAM) Register File** designed and physically implemented in **Cadence Virtuoso IC6.1.x** using the **Generic 180nm CMOS Process Design Kit (gpdk180)** operating at a nominal supply voltage of **$V_{DD} = 1.8\text{ V}$**. 

The design incorporates balanced 6-Transistor (6T) SRAM bitcells, a low-latency 2-to-4 row address decoder, high-drive tri-state write drivers, a bitline precharge/equalization network, and fast regenerative differential latch-type sense amplifiers to achieve low read latency, high noise immunity, and energy-efficient memory operations. The full custom layout is signed off with **Assura DRC (Design Rule Checking)**, **Assura LVS (Layout Versus Schematic)** matching with 100% equivalence, and **Assura RCX / QRC Parasitic RC Extraction** generating clean `av_extracted` post-layout views and finally exported tapeout-ready **[GDSII Stream File (sram_final_4_bit.gds)](./sram_final_4_bit.gds)** and **[Log Files (strmOut.log)](./strmOut.log)** with 0 errors and 0 warnings.

---

## Table of Contents
1. [Project Highlights & Key Specifications](#project-highlights--key-specifications)
2. [Transistor Count & Component Breakdown](#transistor-count--component-breakdown)
3. [Architectural Overview & System Block Diagram](#architectural-overview--system-block-diagram)
4. [Theory of Operation & Timing Cycle](#theory-of-operation--timing-cycle)
   - [Phase 1: Precharge & Bitline Equalization](#phase-1-precharge--bitline-equalization)
   - [Phase 2: Synchronous Write Operation](#phase-2-synchronous-write-operation)
   - [Phase 3: Synchronous Read Operation](#phase-3-synchronous-read-operation)
   - [Phase 4: Differential Sense Amplification](#phase-4-differential-sense-amplification)
   - [Stability Criteria & Sizing Margins](#stability-criteria--sizing-margins)
5. [Hierarchical Cell-by-Cell Design, Simulation & Layout](#hierarchical-cell-by-cell-design-simulation--layout)
   - [1. CMOS Inverter (NOT Gate)](#1-cmos-inverter-not-gate)
   - [2. 2-Input CMOS NOR Gate](#2-2-input-cmos-nor-gate)
   - [3. 2-to-4 Row Address Decoder](#3-2-to-4-row-address-decoder)
   - [4. Standard 6T SRAM Bitcell](#4-standard-6t-sram-bitcell)
   - [5. Bitline Precharge & Equalization Unit](#5-bitline-precharge--equalization-unit)
   - [6. High-Drive Write Driver Circuit](#6-high-drive-write-driver-circuit)
   - [7. Latch-Based Differential Sense Amplifier](#7-latch-based-differential-sense-amplifier)
   - [8. 4-Word x 1-Bit SRAM Core Column Array](#8-4-word--1-bit-sram-core-column-array)
   - [9. Top-Level 4-Word x 4-Bit Synchronous SRAM Macro](#9-top-level-4-word--4-bit-synchronous-sram-macro)
6. [Top-Level Full-Chip Verification & Waveform Analysis](#top-level-full-chip-verification--waveform-analysis)
7. [Comprehensive Quantitative Timing, Power, Resistance-Capacitance (RC) & Energy Calculations](#comprehensive-quantitative-timing-power-resistance-capacitance-rc--energy-calculations)
   - [7.1 Process Technology Parameters & Analytical Framework](#71-process-technology-parameters--analytical-framework)
   - [7.2 Fundamental Equations and Formal Definitions](#72-fundamental-equations-and-formal-definitions)
   - [7.3 Step-by-Step Derivations and Numerical Calculations for Sub-Blocks](#73-step-by-step-derivations-and-numerical-calculations-for-sub-blocks)
   - [7.4 Top-Level 4-Word x 4-Bit Synchronous SRAM System Calculations](#74-top-level-4-word-x-4-bit-synchronous-sram-system-calculations)
   - [7.5 Master Performance Summary Table (Cell-by-Cell & Full-Chip)](#75-master-performance-summary-table-cell-by-cell--full-chip)
   - [7.6 Pre-Layout vs. Post-Layout (RC Extracted) Verification Table](#76-pre-layout-vs-post-layout-rc-extracted-verification-table)
   - [7.7 Cadence Virtuoso ADE Calculator Syntax & Verification Equations](#77-cadence-virtuoso-ade-calculator-syntax--verification-equations)
8. [Physical Design & Sign-Off Verification (DRC, LVS, PEX & GDSII Stream)](#physical-design--sign-off-verification-drc-lvs-pex--gdsii-stream)
   - [8.1 Design Rule Checking (DRC)](#81-design-rule-checking-drc)
   - [8.2 Layout Versus Schematic (LVS)](#82-layout-versus-schematic-lvs)
   - [8.3 Parasitic Extraction (PEX / RCX)](#83-parasitic-extraction-pex--rcx)
   - [8.4 GDSII Stream Export & File Distribution](#84-gdsii-stream-export--file-distribution)
9. [Repository File Hierarchy](#repository-file-hierarchy)
10. [Cadence Virtuoso Simulation & Replication Guide](#cadence-virtuoso-simulation--replication-guide)
11. [Acknowledgements](#acknowledgements)
12. [Author Information](#author-information)

---

## Project Highlights & Key Specifications

| Parameter | Specification / Value |
| :--- | :--- |
| **Technology Node** | Cadence GPDK 180nm CMOS (gpdk180) |
| **Nominal Power Supply ($V_{DD}$)** | $1.8\text{ V}$ |
| **Total Memory Capacity** | 16 Bits (4 Words x 4 Bits/Word) |
| **Word Organization** | 4 Addressable Words (Word 0 to Word 3) |
| **Data Word Width** | 4-Bit Parallel I/O ($DIN[3:0]$, $DOUT[3:0]$) |
| **Address Bus Width** | 2-Bit Binary Decoded ($A_0, A_1$) |
| **Total Transistor Count** | **208 Transistors** (114 NMOS / 94 PMOS) |
| **Read Access Delay ($t_{pd,read}$)** | **$185.90\text{ ps}$** (Address Decode to Sense Amp Latch Output) |
| **Write Access Delay ($t_{pd,write}$)** | **$162.80\text{ ps}$** (Address Decode to Internal Cell Flip) |
| **Contamination Delay ($t_{cd,macro}$)** | **$98.90\text{ ps}$** (Minimum propagation delay / Hold-time safety) |
| **Total Average Power ($P_{avg}$)** | **$79.71\,\mu\text{W}$** (Nominal $100\text{ MHz}$ continuous cycling) |
| **Static Standby Power ($P_{static}$)** | **$84.60\text{ nW}$** (Leakage across 208 MOSFETs in retention) |
| **Power-Delay Product (PDP)** | **$14.82\text{ fJ}$** ($1.482 \times 10^{-14}\text{ J}$) |
| **Energy-Delay Product (EDP)** | **$2754.7\text{ fJ}\cdot\text{ps}$** ($2.755 \times 10^{-24}\text{ J}\cdot\text{s}$) |
| **Physical Sign-Off: DRC** | **Clean (0 Errors / 0 Violations)** via Assura DRC |
| **Physical Sign-Off: LVS** | **100% Schematic-to-Layout Match (0 Net/Instance Discrepancies)** |
| **Parasitic Extraction** | **Assura RCX / QRC Parasitic RC Extraction (`av_extracted` view)** |
| **Tapeout GDSII Export** | **[sram_final_4_bit.gds (30.7 KB)](./sram_final_4_bit.gds)** (Cleanly Streamed Out, 0 Errors / 0 Warnings) |

---

## Transistor Count & Component Breakdown

The complete 4-Word x 4-Bit SRAM Register File macro integrates **29 sub-blocks** consisting of exactly **208 MOSFETs** (114 NMOS and 94 PMOS transistors):

| Subsystem / Functional Block | Instances in Macro | NMOS / Unit | PMOS / Unit | Total / Unit | Cumulative NMOS | Cumulative PMOS | Cumulative Transistors |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **2-to-4 Row Address Decoder** | 1 | 10 | 10 | 20 | 10 | 10 | 20 |
| **6T SRAM Bitcell** | 16 | 4 | 2 | 6 | 64 | 32 | 96 |
| **Bitline Precharge Unit** | 4 | 0 | 5 | 5 | 0 | 20 | 20 |
| **Write Driver Circuit** | 4 | 4 | 3 | 7 | 16 | 12 | 28 |
| **Differential Sense Amplifier** | 4 | 6 | 5 | 11 | 24 | 20 | 44 |
| **FULL-CHIP SRAM MACRO** | **29 Blocks** | — | — | — | **114 NMOS** | **94 PMOS** | **208 Transistors** |

* **NMOS Contribution**: $10\text{ (Decoder)} + 64\text{ (Core Array)} + 0\text{ (Precharge)} + 16\text{ (Write Drivers)} + 24\text{ (Sense Amps)} = 114\text{ NMOS}$ (54.8%)
* **PMOS Contribution**: $10\text{ (Decoder)} + 32\text{ (Core Array)} + 20\text{ (Precharge)} + 12\text{ (Write Drivers)} + 20\text{ (Sense Amps)} = 94\text{ PMOS}$ (45.2%)

---

## Architectural Overview & System Block Diagram

The synchronous SRAM register file is structured into a modular, column-and-row regular matrix. A 2-bit binary address bus ($A_0, A_1$) drives the **2-to-4 Row Address Decoder**, which selectively raises one of four mutually exclusive wordlines ($WL_0, WL_1, WL_2, WL_3$). 

Each column represents one bit of the 4-bit data word and consists of:
1. **Precharge & Equalization Unit**: Pulls both differential bitlines ($BL_j, \overline{BL_j}$) to $V_{DD}$ prior to evaluation and balances their initial voltages.
2. **6T SRAM Bitcell Column**: Four vertically stacked 6T cells sharing the same bitline pair.
3. **Write Driver**: Translates input data $DIN_j$ into complementary rail-to-rail signals on $(BL_j, \overline{BL_j})$ during write cycles ($WE = 1$).
4. **Differential Latch Sense Amplifier**: Detects small differential discharge on $(BL_j, \overline{BL_j})$ during read cycles, rapidly resolving it to a digital logic level at $DOUT_j$ when strobed by $SAE$.

### System Architecture Diagram

[![System Architecture Diagram](images/System_Architecture.png)](SRAM_Register_File)

> 📂 **Design Library Source**: [Explore Cadence Design Library (`SRAM_Register_File`)](SRAM_Register_File)

---

## Theory of Operation & Timing Cycle

The SRAM operation is segmented into synchronous non-overlapping phases governed by the precharge clock ($\overline{PRE}$), write enable ($WE$), address bus ($A_0, A_1$), and sense amplifier enable ($SAE$).

```
Timing Sequence:
Clock / PRE_bar:  ___/\______/\______/\______/\______
Wordline (WL):    ______/\___________/\___________
Write Enable (WE):______/\_________________________ (Write Cycle)
SAE (Sense Strobe):___________________/\___________ (Read Cycle)
Bitlines:         =====[ Precharge ]==[ Differential ]==
```

### Phase 1: Precharge & Bitline Equalization
* **Conditions**: $\overline{PRE} = 0\text{ V}$ (active LOW), $WL_i = 0\text{ V}$, $WE = 0\text{ V}$, $SAE = 0\text{ V}$.
* **Mechanism**: The precharge unit turns ON two pull-up PMOS transistors and one equalization PMOS transistor across each bitline pair.
* Both $BL$ and $\overline{BL}$ are charged solidly to $V_{DD} = 1.8\text{ V}$.
* The equalization transistor shorts $BL$ and $\overline{BL}$, eliminating any residual differential voltage ($\Delta V_{BL} = 0\text{ V}$) from prior operations, establishing symmetric initial states.

### Phase 2: Synchronous Write Operation
* **Conditions**: $\overline{PRE} = 1.8\text{ V}$ (OFF), $WE = 1.8\text{ V}$ (active HIGH), $WL_k = 1.8\text{ V}$ (selected by $A_0, A_1$), $SAE = 0\text{ V}$.
* **Mechanism**: 
  1. The write driver receives single-ended input data $DIN$. It forcefully drives one bitline to solid ground ($0\text{ V}$) and holds the complementary bitline at $V_{DD}$ ($1.8\text{ V}$).
  2. The row decoder asserts the targeted wordline $WL_k$, turning ON the access NMOS transistors of all four bitcells in that row.
  3. The bitline pulled to ground discharges the internal node storing "1" through the access transistor. Because the access transistor is sized stronger than the internal PMOS pull-up load transistor (satisfying the **Pull-up Ratio / Writeability condition**), the internal node drops below the switching threshold ($V_M$) of the inverter, causing the regenerative latch to flip instantaneously.

### Phase 3: Synchronous Read Operation
* **Conditions**: $\overline{PRE} = 1.8\text{ V}$ (OFF), $WE = 0\text{ V}$ (write drivers tri-stated/isolated), $WL_k = 1.8\text{ V}$ (selected row), $SAE = 0\text{ V} \to 1.8\text{ V}$.
* **Mechanism**:
  1. With bitlines precharged to $V_{DD}$ and floating, $WL_k$ is pulsed HIGH.
  2. The access transistors connect the internal storage nodes ($Q$ and $\overline{Q}$) to $BL$ and $\overline{BL}$.
  3. The storage node at "0" draws current through its pull-down driver NMOS and access NMOS, gradually discharging that bitline:
     $$\Delta V_{BL}(t) = \frac{I_{read} \cdot \Delta t}{C_{BL}}$$
  4. The opposite bitline remains tied to the node at "1" and stays at $V_{DD}$.
  5. Sizing constraints ensure that the voltage rise at the "0" node does not exceed the threshold voltage ($V_{tn}$) of the companion NMOS pull-down, preserving **Read Stability**.

### Phase 4: Differential Sense Amplification
* Once a small, reliable differential voltage ($\Delta V_{BL} \approx 100\text{ mV} - 200\text{ mV}$) is established across the bitline pair, the strobe signal **$SAE$** is driven HIGH.
* The differential latch sense amplifier activates its cross-coupled inverter pair through an NMOS tail transistor.
* The regenerative positive feedback of the latch rapidly magnifies the small differential analog signal to full rail-to-rail digital logic levels ($0\text{ V}$ / $1.8\text{ V}$) at $DOUT$.
* **Power Advantage**: The memory does not need to discharge the heavy bitline capacitance all the way to ground during reads. Sensing at $\Delta V_{BL} \ll V_{DD}$ saves substantial dynamic active power ($P_{dyn} = C_{BL} \cdot V_{DD} \cdot \Delta V \cdot f$).

### Stability Criteria & Sizing Margins

The robust operation of the 6T SRAM cell relies on two critical transistor sizing ratios:

$$\text{Cell Ratio } (\beta) = \frac{(W/L)_{driver}}{(W/L)_{access}} = \frac{(W/L)_{M1,M2}}{(W/L)_{M5,M6}} \ge 1.5 - 2.0 \quad \text{[Guarantees Read Stability]}$$

$$\text{Pull-up Ratio } (\gamma) = \frac{(W/L)_{load}}{(W/L)_{access}} = \frac{(W/L)_{M3,M4}}{(W/L)_{M5,M6}} < 1.0 \quad \text{[Guarantees Writeability]}$$

---

## Hierarchical Cell-by-Cell Design, Simulation & Layout

Following the structured bottom-up custom IC design methodology, each sub-block is presented in strict hierarchical sequence: **Circuit Theory & Sizing**, **Schematic Diagram**, **Testbench Circuit**, **Simulation Waveforms**, and **Physical Layout Design** (both dark-background and white-background views), concluding with physical sign-off verification. 

Every image is clickable to navigate directly into its original design cellview folder in `SRAM_Register_File/`.

---

### 1. CMOS Inverter (NOT Gate)

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/inverter/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/inverter/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/inverter/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/inverter_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/inverter)

#### Circuit Theory & Design
The static CMOS inverter serves as the elemental building block for memory latches, address complementary line generation, and write driver logic. 
* **Transistor Count**: 2 Transistors (1 NMOS, 1 PMOS).
* Designed in GPDK 180nm with balanced PMOS-to-NMOS width ratio ($W_p/W_n \approx 2.0 - 2.5$) to compensate for the hole mobility deficit ($\mu_n / \mu_p \approx 2.5$), ensuring symmetrical rise/fall propagation delays ($t_{pLH} = t_{pHL}$) and a midpoint switching threshold $V_M \approx V_{DD}/2 = 0.9\text{ V}$.

#### Schematic Diagram
The transistor-level schematic features PMOS connected between $V_{DD}$ and Output, and NMOS between Output and $GND$, sharing a common gate input.

[![Inverter Schematic](images/01_inverter/Inverter%20Schematic.png)](./SRAM_Register_File/inverter/schematic)

> 📂 **Design Original Folder**: [Open Inverter Schematic View (`SRAM_Register_File/inverter/schematic`)](./SRAM_Register_File/inverter/schematic)

#### Testbench Setup
Transient pulse stimuli applied with rise/fall times of $50\text{ ps}$ and $1.8\text{ V}$ amplitude to evaluate transfer curves, noise margins, and propagation delay.

[![Inverter Testbench](images/01_inverter/Inverter%20Testbench.png)](./SRAM_Register_File/inverter_tb/schematic)

> 📂 **Design Original Folder**: [Open Inverter Testbench View (`SRAM_Register_File/inverter_tb`)](./SRAM_Register_File/inverter_tb/schematic)

#### Simulation Output
Full rail-to-rail swing ($0\text{ V}$ to $1.8\text{ V}$) with sharp switching characteristics and sub-nanosecond delay.

[![Inverter Output](images/01_inverter/Inverter%20Output.png)](./SRAM_Register_File/inverter)

> 📂 **Design Original Folder**: [Open Inverter Cell Directory (`SRAM_Register_File/inverter`)](./SRAM_Register_File/inverter)

#### Physical Layout Design
Custom layout with matched poly routing, guard rings, $N$-well substrate taps, and minimum parasitic area.

##### Physical Layout (Standard Dark Background)
[![Inverter Layout Dark](images/01_inverter/Inverter%20Layout.png)](./SRAM_Register_File/inverter/layout)

##### Physical Layout (White Background View)
[![Inverter Layout White](images/01_inverter/Inverter%20White%20Background%20Layout.png)](./SRAM_Register_File/inverter/layout)

> 📂 **Design Original Folder**: [Open Inverter Layout View (`SRAM_Register_File/inverter/layout`)](./SRAM_Register_File/inverter/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 2. 2-Input CMOS NOR Gate

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/2_input_NOR/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/2_input_NOR/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/2_input_NOR/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/2_input_NOR_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/2_input_NOR)

#### Circuit Theory & Design
The 2-input NOR gate ($Y = \overline{A + B}$) forms the fundamental decoding gate in the 2-to-4 row address decoder.
* **Transistor Count**: 4 Transistors (2 NMOS, 2 PMOS).
* **Pull-Up Network**: Two series-connected PMOS devices. To compensate for the doubled series channel on-resistance ($R_{p,eq} = 2 \cdot R_p$), the PMOS widths are upscaled ($W_p \ge 4 \cdot W_n$) to maintain balanced rise times.
* **Pull-Down Network**: Two parallel NMOS devices offering low discharge resistance to ground.

#### Schematic Diagram
Transistor implementation showing series PMOS pull-up and parallel NMOS pull-down branches.

[![2-Input NOR Schematic](images/02_nor_gate/2%20input%20NOR%20gate%20Schematic.png)](./SRAM_Register_File/2_input_NOR/schematic)

> 📂 **Design Original Folder**: [Open 2-Input NOR Schematic View (`SRAM_Register_File/2_input_NOR/schematic`)](./SRAM_Register_File/2_input_NOR/schematic)

#### Testbench Setup
Exhaustive 2-bit digital sequence applied across inputs $A$ and $B$ (00, 01, 10, 11) to verify truth table compliance.

[![2-Input NOR Testbench](images/02_nor_gate/2%20input%20NOR%20Test%20Bench.png)](./SRAM_Register_File/2_input_NOR_tb/schematic)

> 📂 **Design Original Folder**: [Open 2-Input NOR Testbench View (`SRAM_Register_File/2_input_NOR_tb`)](./SRAM_Register_File/2_input_NOR_tb/schematic)

#### Simulation Output
Clean transient verification showing output HIGH ($1.8\text{ V}$) strictly when $A=0$ and $B=0$, and LOW ($0\text{ V}$) for all other input combinations.

[![2-Input NOR Output](images/02_nor_gate/NOR%20gate%20output.png)](./SRAM_Register_File/2_input_NOR)

> 📂 **Design Original Folder**: [Open 2-Input NOR Cell Directory (`SRAM_Register_File/2_input_NOR`)](./SRAM_Register_File/2_input_NOR)

#### Physical Layout Design
Symmetric poly gates, compact active diffusion sharing to minimize drain junction capacitance, and continuous power rails.

##### Physical Layout (Standard Dark Background)
[![NOR Gate Layout Dark](images/02_nor_gate/2%20input%20NOR%20gate%20Layout.png)](./SRAM_Register_File/2_input_NOR/layout)

##### Physical Layout (White Background View)
[![NOR Gate Layout White](images/02_nor_gate/2%20input%20NOR%20Gate%20White%20Background%20Layout.png)](./SRAM_Register_File/2_input_NOR/layout)

> 📂 **Design Original Folder**: [Open 2-Input NOR Layout View (`SRAM_Register_File/2_input_NOR/layout`)](./SRAM_Register_File/2_input_NOR/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 3. 2-to-4 Row Address Decoder

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/row_decoder_2to4/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/row_decoder_2to4/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/row_decoder_2to4/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/row_decoder_2to4_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/row_decoder_2to4)

#### Circuit Theory & Design
The row address decoder converts the 2-bit binary memory address ($A_0, A_1$) into four one-hot active-HIGH wordline signals ($WL_0, WL_1, WL_2, WL_3$).
* **Transistor Count**: 20 Transistors (10 NMOS, 10 PMOS).
* Constructed using 2 CMOS inverters to generate internal complementary signals ($\overline{A_0}, \overline{A_1}$) and 4 NOR gates configured to implement:
  - $WL_0 = \overline{A_1 + A_0} = \overline{A_1} \cdot \overline{A_0} \implies \text{Word 0}$
  - $WL_1 = \overline{A_1 + \overline{A_0}} = \overline{A_1} \cdot A_0 \implies \text{Word 1}$
  - $WL_2 = \overline{\overline{A_1} + A_0} = A_1 \cdot \overline{A_0} \implies \text{Word 2}$
  - $WL_3 = \overline{\overline{A_1} + \overline{A_0}} = A_1 \cdot A_0 \implies \text{Word 3}$
* Mutual exclusivity is maintained across all timing corners, preventing multi-wordline activation and bus contention.

#### Schematic Diagram
Schematic showing address inverter buffers driving the four NOR decoding gates.

[![Row Decoder Schematic](images/03_row_decoder/Row%20Decoder%202%20to%204%20Schematic.png)](./SRAM_Register_File/row_decoder_2to4/schematic)

> 📂 **Design Original Folder**: [Open Row Decoder Schematic View (`SRAM_Register_File/row_decoder_2to4/schematic`)](./SRAM_Register_File/row_decoder_2to4/schematic)

#### Testbench Setup
Binary address sweep from $00_2$ to $11_2$ with synchronous transition edges.

[![Row Decoder Testbench](images/03_row_decoder/Row%20Decoder%202%20to%204%20Testbench.png)](./SRAM_Register_File/row_decoder_2to4_tb/schematic)

> 📂 **Design Original Folder**: [Open Row Decoder Testbench View (`SRAM_Register_File/row_decoder_2to4_tb`)](./SRAM_Register_File/row_decoder_2to4_tb/schematic)

#### Simulation Output
Waveforms confirm exact 1-of-4 decoding with fast propagation delay from address change to wordline transition.

[![Row Decoder Output](images/03_row_decoder/2%20to%204%20decoder%20output.png)](./SRAM_Register_File/row_decoder_2to4)

> 📂 **Design Original Folder**: [Open Row Decoder Cell Directory (`SRAM_Register_File/row_decoder_2to4`)](./SRAM_Register_File/row_decoder_2to4)

#### Physical Layout Design
Pitch-matched layout aligned with the row pitch of the SRAM core matrix to ensure direct horizontal wordline routing in Metal2/Metal3 without dogleg detours.

##### Physical Layout (Standard Dark Background)
[![Decoder Layout Dark](images/03_row_decoder/Decoder%202%20to%204%20Layout.png)](./SRAM_Register_File/row_decoder_2to4/layout)

##### Physical Layout (White Background View)
[![Decoder Layout White](images/03_row_decoder/Decoder%202%20to%204%20White%20Background%20Layout.png)](./SRAM_Register_File/row_decoder_2to4/layout)

> 📂 **Design Original Folder**: [Open Row Decoder Layout View (`SRAM_Register_File/row_decoder_2to4/layout`)](./SRAM_Register_File/row_decoder_2to4/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 4. Standard 6T SRAM Bitcell

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/sram_cell_6t/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/sram_cell_6t/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/sram_cell_6t/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/sram_cell_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/sram_cell_6t)

#### Circuit Theory & Design
The standard 6-Transistor static RAM cell comprises two cross-coupled CMOS inverters ($M_1-M_4$) and two NMOS pass-transistor access gates ($M_5, M_6$).
* **Transistor Count**: 6 Transistors (4 NMOS, 2 PMOS).
* **Read Stability**: Designed with Cell Ratio $\beta = (W/L)_{M1,M2} / (W/L)_{M5,M6} \approx 1.8$, preventing the read current from elevating node voltage $V_Q$ above the NMOS threshold voltage $V_{tn}$.
* **Writeability**: Designed with Pull-Up Ratio $\gamma = (W/L)_{M3,M4} / (W/L)_{M5,M6} \approx 0.6 < 1.0$, allowing the write driver to pull down the node storing "1" past $V_M$ to guarantee write success.
* **Storage Nodes**: Symmetrical bistable state storage ($Q$ and $\overline{Q}$).

#### Schematic Diagram
Complete transistor schematic showing the cross-coupled inverter core and wordline-gated access transistors.

[![6T SRAM Cell Schematic](images/04_sram_cell_6t/6T%20RAM%20Cell%20Schematic.png)](./SRAM_Register_File/sram_cell_6t/schematic)

> 📂 **Design Original Folder**: [Open 6T SRAM Cell Schematic View (`SRAM_Register_File/sram_cell_6t/schematic`)](./SRAM_Register_File/sram_cell_6t/schematic)

#### Testbench Circuit Setups
Multiple testbenches were constructed to verify: (1) Transient hold and read stability, (2) Multi-cycle read and write operations, and (3) Read discharge behavior under bitline load.

##### Static & Transient Testbench
[![SRAM Cell TB 1](images/04_sram_cell_6t/6%20T%20SRAM%20Cell%20Testbench.png)](./SRAM_Register_File/sram_cell_tb/schematic)

##### Read & Write Operation Testbench
[![SRAM Cell TB 2](images/04_sram_cell_6t/Testbench%20to%20Test%20SRAM%20Read%20Write%20Operation.png)](./SRAM_Register_File/sram_cell_tb/schematic)

##### Read Operation Loaded Circuit
[![SRAM Cell TB 3](images/04_sram_cell_6t/Read%20operation%20SRAM%20Cell%20Circuit.png)](./SRAM_Register_File/sram_cell_tb/schematic)

> 📂 **Design Original Folder**: [Open 6T SRAM Cell Testbench View (`SRAM_Register_File/sram_cell_tb`)](./SRAM_Register_File/sram_cell_tb/schematic)

#### Simulation Outputs
* **Dynamic Read & Write Cycles**: Demonstrating successful non-destructive read operations and state flipping during writes.
* **Transient Stability**: Preserving stored data indefinitely in hold mode with zero static gate leakage dissipation.

##### Transient 6T Cell Response Waveform
[![SRAM Cell Output 1](images/04_sram_cell_6t/SRAM%20Cell%20Output.png)](./SRAM_Register_File/sram_cell_6t)

##### Read and Write Operation Dynamic Waveforms
[![SRAM Cell Output 2](images/04_sram_cell_6t/Read%20and%20Write%20Operation%20for%20the%206T%20SRAM%20Cell%20.png)](./SRAM_Register_File/sram_cell_6t)

> 📂 **Design Original Folder**: [Open 6T SRAM Cell Directory (`SRAM_Register_File/sram_cell_6t`)](./SRAM_Register_File/sram_cell_6t)

#### Physical Layout Design
Highly optimized symmetrical layout featuring thin-cell topology, shared contact diffusion for $V_{DD}$ and $GND$, and vertical bitline / horizontal wordline routing tracks.

##### Physical Layout (Standard Dark Background)
[![6T Cell Layout Dark](images/04_sram_cell_6t/6%20T%20SRAM%20Cell%20Layout.png)](./SRAM_Register_File/sram_cell_6t/layout)

##### Physical Layout (White Background View)
[![6T Cell Layout White](images/04_sram_cell_6t/SRAM%20Cell%206T%20White%20Background%20Layout.png)](./SRAM_Register_File/sram_cell_6t/layout)

> 📂 **Design Original Folder**: [Open 6T SRAM Cell Layout View (`SRAM_Register_File/sram_cell_6t/layout`)](./SRAM_Register_File/sram_cell_6t/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 5. Bitline Precharge & Equalization Unit

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/precharge_unit/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/precharge_unit/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/precharge_unit/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/precharge_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/precharge_unit)

#### Circuit Theory & Design
The precharge and equalization circuit prepares the high-capacitance bitline pairs for upcoming read and write operations.
* **Transistor Count**: 5 Transistors (0 NMOS, 5 PMOS).
* Contains dual pull-up PMOS transistors connected between $V_{DD}$ and the bitlines, plus one bridging PMOS equalization transistor connected between $BL$ and $\overline{BL}$.
* Controlled by active-low signal $\overline{PRE}$:
  - When $\overline{PRE} = 0\text{ V}$, both bitlines are pulled up to $V_{DD}$ and equalized to eliminate residual voltage offsets.
  - When $\overline{PRE} = 1.8\text{ V}$, the circuit turns OFF completely, allowing bitlines to float during read differential discharge or be pulled down by write drivers without DC supply contention.

#### Schematic Diagram
Five-transistor PMOS network providing simultaneous pull-up and cross-equalization.

[![Precharge Schematic](images/05_precharge_unit/Pre_Charge_Unit_Schematic.png)](./SRAM_Register_File/precharge_unit/schematic)

> 📂 **Design Original Folder**: [Open Precharge Unit Schematic View (`SRAM_Register_File/precharge_unit/schematic`)](./SRAM_Register_File/precharge_unit/schematic)

#### Testbench Setup
Testbench evaluating precharge recovery time and equalization speed across loaded bitlines.

[![Precharge Testbench](images/05_precharge_unit/Pre%20Charge%20Unit%20Test%20Bench.png)](./SRAM_Register_File/precharge_tb/schematic)

> 📂 **Design Original Folder**: [Open Precharge Testbench View (`SRAM_Register_File/precharge_tb`)](./SRAM_Register_File/precharge_tb/schematic)

#### Simulation Outputs
* Rapid bitline pull-up recovery from $0\text{ V}$ to $1.8\text{ V}$ within hundreds of picoseconds.
* Full integration waveforms showing periodic precharge assertion between read access events.

##### Standalone Precharge Characteristic Response
[![Precharge Output](images/05_precharge_unit/Pre%20Charge%20Unit.png)](./SRAM_Register_File/precharge_unit)

##### Read Operation Circuit with 6T Cell & Precharge Unit
[![Read Precharge Circuit](images/05_precharge_unit/Read%20operation%20using%20SRAM%20Cell%20and%20PreCharge%20unit.png)](./SRAM_Register_File/precharge_unit)

##### Full Read Operation Waveforms with Precharge Cycling
[![Read Full Waveforms](images/05_precharge_unit/Read%20operation%20full%20with%20SRAM%20Cell%20and%20Pre%20CHarge%20Unit.png)](./SRAM_Register_File/precharge_unit)

> 📂 **Design Original Folder**: [Open Precharge Unit Cell Directory (`SRAM_Register_File/precharge_unit`)](./SRAM_Register_File/precharge_unit)

#### Physical Layout Design
Pitch-matched layout placed at the top boundary of each bitline column.

##### Physical Layout (Standard Dark Background)
[![Precharge Layout Dark](images/05_precharge_unit/Precharge%20Unit%20Layout.png)](./SRAM_Register_File/precharge_unit/layout)

##### Physical Layout (White Background View)
[![Precharge Layout White](images/05_precharge_unit/Pre%20Charge%20Unit%20White%20Background%20Layout.png)](./SRAM_Register_File/precharge_unit/layout)

> 📂 **Design Original Folder**: [Open Precharge Unit Layout View (`SRAM_Register_File/precharge_unit/layout`)](./SRAM_Register_File/precharge_unit/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 6. High-Drive Write Driver Circuit

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/write_driver/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/write_driver/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/write_driver/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/write_driver_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/write_driver)

#### Circuit Theory & Design
The write driver overrides the stored state of the selected 6T cell by driving one bitline fully to $0\text{ V}$ while keeping the other at $V_{DD}$.
* **Transistor Count**: 7 Transistors (4 NMOS, 3 PMOS).
* Enabled by the active-HIGH **Write Enable ($WE$)** signal.
* When $WE = 1$, high-drive NMOS pull-down transistors discharge the appropriate bitline based on input data $DIN$.
* When $WE = 0$, both bitline drive paths enter high-impedance (tri-state) mode, completely isolating the write circuitry from the bitlines during read and precharge phases.

#### Schematic Diagram
Write driver schematic incorporating tri-state gating, data complementary drive, and strong pull-down transistors.

[![Write Driver Schematic](images/06_write_driver/write_driver_schematic.png)](./SRAM_Register_File/write_driver/schematic)

> 📂 **Design Original Folder**: [Open Write Driver Schematic View (`SRAM_Register_File/write_driver/schematic`)](./SRAM_Register_File/write_driver/schematic)

#### Testbench Setup
Testbench applying alternating $DIN$ patterns ($0 \to 1 \to 0$) under synchronized $WE$ strobe pulses.

[![Write Driver Testbench](images/06_write_driver/Write%20Driver%20Test%20Bench.png)](./SRAM_Register_File/write_driver_tb/schematic)

> 📂 **Design Original Folder**: [Open Write Driver Testbench View (`SRAM_Register_File/write_driver_tb`)](./SRAM_Register_File/write_driver_tb/schematic)

#### Simulation Output
Clear rail-to-rail complementary bitline switching when $WE$ is asserted, and high-impedance isolation when $WE$ is de-asserted.

[![Write Driver Output](images/06_write_driver/Wire%20Driver%20Output.png)](./SRAM_Register_File/write_driver)

> 📂 **Design Original Folder**: [Open Write Driver Cell Directory (`SRAM_Register_File/write_driver`)](./SRAM_Register_File/write_driver)

#### Physical Layout Design
Compact layout featuring wide NMOS pull-down transistors for minimal ground resistance.

##### Physical Layout (Standard Dark Background)
[![Write Driver Layout Dark](images/06_write_driver/Write%20Driver%20Layout.png)](./SRAM_Register_File/write_driver/layout)

##### Physical Layout (White Background View)
[![Write Driver Layout White](images/06_write_driver/Wire%20Driver%20white%20background%20Layout.png)](./SRAM_Register_File/write_driver/layout)

> 📂 **Design Original Folder**: [Open Write Driver Layout View (`SRAM_Register_File/write_driver/layout`)](./SRAM_Register_File/write_driver/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 7. Latch-Based Differential Sense Amplifier

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/sense_amp/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/sense_amp/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/sense_amp/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/sense_amp_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/sense_amp)

#### Circuit Theory & Design
The sense amplifier is a strobed differential voltage latch that amplifies small differential voltages ($\Delta V_{BL}$) on the bitlines into full CMOS logic levels.
* **Transistor Count**: 11 Transistors (6 NMOS, 5 PMOS).
* **Topology**: Cross-coupled CMOS inverter pair with NMOS tail transistors activated by **Sense Amplifier Enable ($SAE$)**.
* **High Sensitivity**: Because it uses regenerative positive feedback, once enabled, the cross-coupled pair resolves an input voltage difference as small as $50\text{ mV} - 100\text{ mV}$ within picoseconds.
* **Energy Efficiency**: Restricting the bitline swing to a small fraction of $V_{DD}$ dramatically cuts dynamic charging and discharging energy.

#### Schematic Diagram
Cross-coupled latch schematic with differential bitline inputs, strobed tail current path, and output buffers.

[![Sense Amplifier Schematic](images/07_sense_amplifier/Sense%20Amplifier%20Schematic.png)](./SRAM_Register_File/sense_amp/schematic)

> 📂 **Design Original Folder**: [Open Sense Amplifier Schematic View (`SRAM_Register_File/sense_amp/schematic`)](./SRAM_Register_File/sense_amp/schematic)

#### Testbench Setup
Testbench applying small differential input voltages to characterize gain, strobe response time, and output settling.

[![Sense Amplifier Testbench](images/07_sense_amplifier/Sense%20Amplifier%20Testbench.png)](./SRAM_Register_File/sense_amp_tb/schematic)

> 📂 **Design Original Folder**: [Open Sense Amplifier Testbench View (`SRAM_Register_File/sense_amp_tb`)](./SRAM_Register_File/sense_amp_tb/schematic)

#### Simulation Output
Transient waveforms showing sharp output latching to $1.8\text{ V}$ or $0\text{ V}$ immediately following the $SAE$ trigger edge.

[![Sense Amplifier Output](images/07_sense_amplifier/Sense%20Amplifier%20Output.png)](./SRAM_Register_File/sense_amp)

> 📂 **Design Original Folder**: [Open Sense Amplifier Cell Directory (`SRAM_Register_File/sense_amp`)](./SRAM_Register_File/sense_amp)

#### Physical Layout Design
Carefully mirrored and interdigitated layout to minimize transistor mismatch and reduce input-referred DC offset voltage.

##### Physical Layout (Standard Dark Background)
[![Sense Amp Layout Dark](images/07_sense_amplifier/Sense%20Amplifier%20Layout.png)](./SRAM_Register_File/sense_amp/layout)

##### Physical Layout (White Background View)
[![Sense Amp Layout White](images/07_sense_amplifier/Sense%20Amplifier%20White%20Background%20Layout.png)](./SRAM_Register_File/sense_amp/layout)

> 📂 **Design Original Folder**: [Open Sense Amplifier Layout View (`SRAM_Register_File/sense_amp/layout`)](./SRAM_Register_File/sense_amp/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 8. 4-Word x 1-Bit SRAM Core Column Array

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/sram_core_array/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/sram_core_array/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/sram_core_array/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/sram_core_array_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/sram_core_array)

#### Circuit Theory & Design
The 4-Word x 1-Bit array forms one complete vertical slice of the memory.
* **Transistor Count**: 24 Transistors (16 NMOS, 8 PMOS) spanning four 6T SRAM cells.
* Integrates 4 stacked 6T SRAM cells along a continuous vertical differential bitline pair ($BL, \overline{BL}$).
* Connected to four independent horizontal wordlines ($WL_0, WL_1, WL_2, WL_3$).
* Verifies column-level pitch matching, continuous power rail distribution, and inter-cell parasitic capacitance.

#### Schematic Diagram
Array schematic showing 4 6T cells tied to common differential bitlines with distinct wordline controls.

[![1-Bit Array Schematic](images/08_sram_core_1bit_column/1%20bit%20SRAM%20Array.png)](./SRAM_Register_File/sram_core_array/schematic)

> 📂 **Design Original Folder**: [Open 1-Bit Core Array Schematic View (`SRAM_Register_File/sram_core_array/schematic`)](./SRAM_Register_File/sram_core_array/schematic)

#### Testbench Setup
Multi-word sequential read and write testbench verifying that activating one wordline does not disturb unselected cells sharing the same column.

[![1-Bit Array Testbench](images/08_sram_core_1bit_column/SRAM%20Core%20Array%20Testbench.png)](./SRAM_Register_File/sram_core_array_tb/schematic)

> 📂 **Design Original Folder**: [Open 1-Bit Core Array Testbench View (`SRAM_Register_File/sram_core_array_tb`)](./SRAM_Register_File/sram_core_array_tb/schematic)

#### Simulation Output
Multi-wordline read/write verification waveforms across all 4 words.

[![1-Bit Array Output](images/08_sram_core_1bit_column/1%20bit%20SRAM%20Core%204%20-%20word%20Line.png)](./SRAM_Register_File/sram_core_array)

> 📂 **Design Original Folder**: [Open 1-Bit Core Array Cell Directory (`SRAM_Register_File/sram_core_array`)](./SRAM_Register_File/sram_core_array)

#### Physical Layout Design
Vertical abutment of 4 6T cells sharing continuous $V_{DD}$, $GND$, and $N$-well regions for maximum silicon area density.

##### Physical Layout (Standard Dark Background)
[![1-Bit Array Layout Dark](images/08_sram_core_1bit_column/sram%201%20bit%20core%20array.png)](./SRAM_Register_File/sram_core_array/layout)

##### Physical Layout (White Background View)
[![1-Bit Array Layout White](images/08_sram_core_1bit_column/4%20word%20x%201%20bit%20SRAM%20Core%20Array%20White%20Background%20Layout.png)](./SRAM_Register_File/sram_core_array/layout)

> 📂 **Design Original Folder**: [Open 1-Bit Core Array Layout View (`SRAM_Register_File/sram_core_array/layout`)](./SRAM_Register_File/sram_core_array/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

### 9. Top-Level 4-Word x 4-Bit Synchronous SRAM Macro

> 📂 **Cadence OpenAccess Design Files on GitHub**:
> [📄 Schematic (sch.oa)](./SRAM_Register_File/sram_final_4_bit/schematic) • [🔣 Symbol (symbol.oa)](./SRAM_Register_File/sram_final_4_bit/symbol) • [📐 Layout (layout.oa)](./SRAM_Register_File/sram_final_4_bit/layout) • [🧪 Testbench (sch.oa)](./SRAM_Register_File/sram_final_4_bit_tb/schematic) • [📁 Cell Directory](./SRAM_Register_File/sram_final_4_bit)

#### Circuit Theory & Design
The top-level memory macro integrates four 4-Word x 1-Bit columns with the 2-to-4 row address decoder, four precharge units, four write drivers, and four differential sense amplifiers to form the complete **16-bit Synchronous SRAM Register File**.
* **Total Transistor Count**: **208 Transistors** (114 NMOS, 94 PMOS) across 29 sub-blocks.
* Parallel 4-bit data input bus ($DIN[3:0]$) and 4-bit data output bus ($DOUT[3:0]$ / $Out[3:0]$).
* Uniform power grid distribution, clock distribution for $\overline{PRE}$ and $SAE$, and global $WE$ control.

#### Schematic Diagram
Top-level structural schematic illustrating complete integration of row decoder, 4×4 core matrix, precharge blocks, write drivers, and differential sense amplifiers.

[![Top Level SRAM Macro Schematic](images/09_top_sram_4x4_register_file/4%20bit%20SRAM%20Array.png)](./SRAM_Register_File/sram_final_4_bit/schematic)

> 📂 **Design Original Folder**: [Open Top-Level SRAM Macro Schematic View (`SRAM_Register_File/sram_final_4_bit/schematic`)](./SRAM_Register_File/sram_final_4_bit/schematic)

#### Testbench Setup
Comprehensive system-level testbench generating multi-cycle read and write access sequences across all four words with distinct data patterns.

[![Top Level SRAM Testbench](images/09_top_sram_4x4_register_file/SRAM%20final%204%20bit%20x%204%20word%20testbench.png)](./SRAM_Register_File/sram_final_4_bit_tb/schematic)

> 📂 **Design Original Folder**: [Open Top-Level SRAM Macro Testbench View (`SRAM_Register_File/sram_final_4_bit_tb`)](./SRAM_Register_File/sram_final_4_bit_tb/schematic)

#### Simulation Outputs & Multi-Cycle Waveforms
Transient simulation results over full multi-word write/read cycles:
* **Output 1**: Extended multi-cycle transient response demonstrating successive write operations to all four addresses followed by non-destructive read operations with differential sensing.
* **Output 2**: Focused verification showing exact timing correlation between address decoding, precharge de-assertion, write drive, and sense amplifier latch output.
* **Waveform Detail**: Input data ($DIN$), Address ($A_0, A_1$), and Valid Output ($DOUT$) timing alignment.

##### Full Multi-Cycle Read/Write Transient Waveform (Run 1)
[![Top SRAM Output 1](images/09_top_sram_4x4_register_file/4%20bit%20SRAM%20Output%201.png)](./SRAM_Register_File/sram_final_4_bit)

##### Multi-Cycle Read/Write Transient Waveform (Run 2)
[![Top SRAM Output 2](images/09_top_sram_4x4_register_file/4%20Bit%20SRAM%20Out%202.png)](./SRAM_Register_File/sram_final_4_bit)

##### Detailed Address Bus, DIN & DOUT Timing Correlation
[![Top SRAM DIN DOUT Detail](images/09_top_sram_4x4_register_file/A%20DIN%20OUT%20Output%20Waveform.png)](./SRAM_Register_File/sram_final_4_bit)

> 📂 **Design Original Folder**: [Open Top-Level SRAM Macro Cell Directory (`SRAM_Register_File/sram_final_4_bit`)](./SRAM_Register_File/sram_final_4_bit)

#### ADE ViVA Direct Simulation Reports
Direct simulation printouts from Cadence Virtuoso Analog Design Environment (ADE) ViVA waveform viewer documenting transient signal dynamics ($V_{DD} = 1.8\text{ V}$, $0\text{ to }1.0\,\mu\text{s}$):

##### ADE ViVA Simulation Waveform Report 1
[![ADE Report 1](images/09_top_sram_4x4_register_file/4_bit_SRAM_Register_File_Output_page_1.png)](./SRAM_Register_File/sram_final_4_bit_tb/schematic)

##### ADE ViVA Simulation Waveform Report 2
[![ADE Report 2](images/09_top_sram_4x4_register_file/4_bit_SRAM_Register_File_Output_Final_page_1.png)](./SRAM_Register_File/sram_final_4_bit_tb/schematic)

##### ADE ViVA Simulation Waveform Report 3
[![ADE Report 3](images/09_top_sram_4x4_register_file/4_bit_SRAM_Register_File_Output_Final2_page_1.png)](./SRAM_Register_File/sram_final_4_bit_tb/schematic)

*(Original PDF format documentation available: [`Output Report 1`](images/09_top_sram_4x4_register_file/4_bit_SRAM_Register_File_Output.pdf), [`Output Report 2`](images/09_top_sram_4x4_register_file/4_bit_SRAM_Register_File_Output_Final.pdf), [`Output Report 3`](images/09_top_sram_4x4_register_file/4_bit_SRAM_Register_File_Output_Final2.pdf))*

> 📂 **Design Original Folder**: [Open Top-Level SRAM Testbench Directory (`SRAM_Register_File/sram_final_4_bit_tb`)](./SRAM_Register_File/sram_final_4_bit_tb/schematic)

#### Physical Layout Design
Full-chip monolithic custom layout integrating the row decoder on the left, four precharge units at the top, the 4×4 cell array in the center, and the four write drivers and differential sense amplifiers at the bottom.

##### Monolithic Macro Physical Layout (Wide View)
[![Top Macro Layout Wide](images/09_top_sram_4x4_register_file/4%20word%20x%204%20Bit%20SRAM%20Final%20Layout.png)](./SRAM_Register_File/sram_final_4_bit/layout)

##### Monolithic Macro Physical Layout (Detailed Routing & Pin Views)
[![Top Macro Layout Detail](images/09_top_sram_4x4_register_file/4%20word%20x%204%20bit%20Synchronous%20SRAM%20Register%20File%20Layout.png)](./SRAM_Register_File/sram_final_4_bit/layout)

> 📂 **Design Original Folder**: [Open Top-Level Layout View (`SRAM_Register_File/sram_final_4_bit/layout`)](./SRAM_Register_File/sram_final_4_bit/layout)

* **Sign-Off Status**: Assura DRC Clean (0 errors), Assura LVS 100% matched, Parasitic RC Extracted.

---

## Top-Level Full-Chip Verification & Waveform Analysis

The top-level testbench (`sram_final_4_bit_tb`) exercises the entire memory macro across an exhaustive write-then-read verification sequence across all four address locations:

### Address & Data Verification Matrix

| Cycle | Time Interval | Address ($A_1 A_0$) | Selected Word | Mode | Input Data ($DIN[3:0]$) | Control Signals Active | Output ($DOUT[3:0]$) | Result Status |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | $0 - 100\text{ ns}$ | `00` | Word 0 | **WRITE** | `1010` ($10_{10}$) | $WE=1$, $\overline{PRE}=1$ | High-Z / Previous | **Write Success** |
| **2** | $100 - 200\text{ ns}$ | `01` | Word 1 | **WRITE** | `0101` ($5_{10}$) | $WE=1$, $\overline{PRE}=1$ | High-Z / Previous | **Write Success** |
| **3** | $200 - 300\text{ ns}$ | `10` | Word 2 | **WRITE** | `1100` ($12_{10}$) | $WE=1$, $\overline{PRE}=1$ | High-Z / Previous | **Write Success** |
| **4** | $300 - 400\text{ ns}$ | `11` | Word 3 | **WRITE** | `0011` ($3_{10}$) | $WE=1$, $\overline{PRE}=1$ | High-Z / Previous | **Write Success** |
| **5** | $400 - 500\text{ ns}$ | `00` | Word 0 | **READ** | `XXXX` | $\overline{PRE}=0 \to 1$, $SAE=1$ | `1010` ($10_{10}$) | **Data Matched** |
| **6** | $500 - 600\text{ ns}$ | `01` | Word 1 | **READ** | `XXXX` | $\overline{PRE}=0 \to 1$, $SAE=1$ | `0101` ($5_{10}$) | **Data Matched** |
| **7** | $600 - 700\text{ ns}$ | `10` | Word 2 | **READ** | `XXXX` | $\overline{PRE}=0 \to 1$, $SAE=1$ | `1100` ($12_{10}$) | **Data Matched** |
| **8** | $700 - 800\text{ ns}$ | `11` | Word 3 | **READ** | `XXXX` | $\overline{PRE}=0 \to 1$, $SAE=1$ | `0011` ($3_{10}$) | **Data Matched** |

As demonstrated in the ViVA waveform records, the read data perfectly replicates the stored binary vectors without read disturbances or timing glitches, validating the complete functional correctness of the memory register file.

---

## 7. Comprehensive Quantitative Timing, Power, Resistance-Capacitance (RC) & Energy Calculations

> 📄 **Official Technical Calculation Report**: [**Download / View sram_calculations.pdf**](./sram_calculations.pdf) *(10-Page Comprehensive Engineering Derivation Manual)*

This section provides the complete quantitative mathematical derivations, equivalent RC network models, transient timing delays ($t_{pLH}$, $t_{pHL}$, $t_{pd}$, $t_{cd}$), static and dynamic power dissipations ($P_{static}$, $P_{dynamic}$, $P_{sc}$, $P_{avg}$), Power-Delay Product (PDP), and Energy-Delay Product (EDP) for the **4-Word x 4-Bit Synchronous SRAM Register File**.

To ensure 100% mathematical fidelity and eliminate web-browser KaTeX formatting limitations on GitHub, all step-by-step proofs, integral formulations, and verification matrices have been compiled into the publication-quality document **[`sram_calculations.pdf`](./sram_calculations.pdf)** and rendered as high-resolution engineering figures below:

### GDSII Stream File & Tapeout Verification
* 💾 **Direct GDSII Download**: [`sram_final_4_bit.gds`](./sram_final_4_bit.gds) *(30.7 KB, Stream Level 7)*
* 📋 **Stream Out Log**: [`strmOut.log`](./strmOut.log) *(Clean Export with 0 errors and 0 warnings)*
* 📂 **Library Directory File**: [`SRAM_Register_File/sram_final_4_bit.gds`](./SRAM_Register_File/sram_final_4_bit.gds)

---

### 7.1 Process Technology Parameters & Analytical Framework

Physical and electrical constants of the **Cadence Generic 180nm CMOS (gpdk180)** library at nominal conditions ($T = 27^\circ\text{C} = 300\,\text{K}$, $V_{DD} = 1.80\,\text{V}$):

<p align="center">
  <img src="./images/calculations/01_process_technology_parameters.png" alt="Process Technology Parameters & Architecture" width="100%">
</p>

---

### 7.2 Fundamental Equations and Formal Definitions

Formal mathematical definitions for propagation delay ($t_{pd}$), contamination delay ($t_{cd}$), equivalent channel ON-resistance ($R_{eq}$), lumped load capacitance ($C_L$), dynamic power ($P_{dyn}$), static leakage ($P_{static}$), short-circuit power ($P_{sc}$), PDP, and EDP:

<p align="center">
  <img src="./images/calculations/02_fundamental_governing_equations.png" alt="Fundamental Theoretical Definitions & Governing Formulations" width="100%">
</p>

---

### 7.3 Step-by-Step Derivations and Numerical Calculations for Sub-Blocks

#### 1 & 2. CMOS Inverter & 2-Input Static CMOS NOR Gate
Transistor sizing, effective pull-up / pull-down channel resistances, drain/gate parasitic capacitances, propagation delays, contamination delays, and power dissipation breakdown:

<p align="center">
  <img src="./images/calculations/03_subblocks_inverter_and_nor2.png" alt="Inverter and NOR2 Calculations" width="100%">
</p>

#### 3 & 4. 2-to-4 Row Address Decoder & 6T SRAM Bitcell
Decoded wordline latency, distributed RC interconnect driving capability, static noise margin (SNM) in hold and read modes, write margin (WM), and bitcell switching energy:

<p align="center">
  <img src="./images/calculations/04_subblocks_decoder_and_6t_cell.png" alt="Row Decoder and 6T Bitcell Calculations" width="100%">
</p>

#### 5 & 6. Bitline Precharge Circuit & Push-Pull Write Driver
Active-low PMOS equalization and precharge timing ($t_{pre}$), write driver pull-down discharge latency, contention resolution, and overdrive sizing:

<p align="center">
  <img src="./images/calculations/05_subblocks_precharge_and_write_driver.png" alt="Bitline Precharge and Write Driver Calculations" width="100%">
</p>

#### 7 & 8. Latch-Based Differential Sense Amplifier (VLSA) & 4x1 Memory Core Column
Small-swing voltage sensing resolution ($\Delta V_{BL} = 150\,\text{mV}$), positive feedback regenerative gain, strobe enable timing, and 4-word column capacitance aggregation:

<p align="center">
  <img src="./images/calculations/06_subblocks_sense_amp_and_column.png" alt="Sense Amplifier and Column Calculations" width="100%">
</p>

---

### 7.4 Top-Level 4-Word x 4-Bit Synchronous SRAM System Calculations

#### Critical Path Timing, Read/Write Latency, and Contamination Delay
Complete end-to-end read cycle latency ($t_{pd,read} = 185.90\,\text{ps}$), write access latency ($t_{pd,write} = 162.80\,\text{ps}$), full-chip contamination delay ($t_{cd} = 98.90\,\text{ps}$), and distributed wire RC interconnect modeling:

<p align="center">
  <img src="./images/calculations/07_top_level_sram_timing_and_delays.png" alt="Top-Level Timing, Delay and RC Interconnect Calculations" width="100%">
</p>

#### Dynamic, Static, Short-Circuit Power Breakdown, PDP & EDP
Macro-level standby leakage ($P_{static} = 84.60\,\text{nW}$), active dynamic switching power ($P_{dyn} = 68.42\,\mu\text{W}$), crowbar short-circuit power ($P_{sc} = 11.20\,\mu\text{W}$), total average dissipation ($P_{avg} = 79.71\,\mu\text{W}$ @ 100MHz), Power-Delay Product ($\text{PDP} = 14.82\,\text{fJ}$), and Energy-Delay Product ($\text{EDP} = 2754.7\,\text{fJ}\cdot\text{ps}$):

<p align="center">
  <img src="./images/calculations/08_top_level_sram_power_and_energy.png" alt="Top-Level Power, PDP, and EDP Calculations" width="100%">
</p>

---

### 7.5 Master Performance Summary Table (Cell-by-Cell & Full-Chip)

<p align="center">
  <img src="./images/calculations/09_master_performance_summary_table.png" alt="Master Performance Summary Table and Physical Silicon Area" width="100%">
</p>

#### Summary Data (Pre-Layout Simulation @ 1.8V, 100MHz)
| Circuit Block | MOSFET Count | tpLH (ps) | tpHL (ps) | tpd (ps) | tcd (ps) | Req (kΩ) | Ctotal (fF) | Pstatic (nW) | Pdyn (µW) | Psc (µW) | Pavg (µW) | PDP (fJ) | EDP (fJ·ps) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **CMOS Inverter** | 2 | 32.58 | 28.42 | **30.50** | **19.83** | 3.28 | 13.42 | 1.48 | 2.17 | 0.46 | **2.63** | 0.080 | 2.45 |
| **2-Input CMOS NOR** | 4 | 40.51 | 35.34 | **37.93** | **23.52** | 3.28 | 16.69 | 3.51 | 1.01 | 0.39 | **1.40** | 0.053 | 2.02 |
| **2:4 Row Decoder** | 20 | 136.50 | 126.61 | **131.56** | **44.98** | — | 31.92 | 17.64 | 14.85 | 2.45 | **17.32** | 2.278 | 299.7 |
| **6T Bitcell (Write)** | 6 | 58.10 | 53.80 | **55.95** | **36.37** | 4.58 | 14.00 | 1.71 | 4.53 | 0.82 | **5.35** | 0.299 | 16.7 |
| **Precharge Unit** | 5 | 130.81 | — | **130.81** | **85.03** | 3.89 | 48.50 | 2.16 | 1.31 | 0.18 | **1.49** | 0.195 | 25.5 |
| **Write Driver** | 7 | 115.40 | 102.75 | **109.08** | **67.63** | 3.06 | 48.50 | 5.04 | 7.86 | 1.25 | **9.11** | 0.994 | 108.4 |
| **Differential Sense Amp** | 11 | 58.20 | 52.60 | **55.40** | **34.90** | — | 18.50 | 2.70 | 5.99 | 1.85 | **7.85** | 0.435 | 24.1 |
| **4x1 Bit Core Column** | 24 | 58.10 | 53.80 | **55.95** | **36.37** | 4.58 | 48.50 | 6.84 | 4.53 | 0.82 | **5.36** | 0.300 | 16.8 |
| **SRAM Macro (Read)** | **208** | 193.90 | 177.90 | **185.90** | **98.90** | — | — | **84.60** | **68.42** | **11.20** | **79.71** | **14.82** | **2754.7** |
| **SRAM Macro (Write)** | **208** | 169.50 | 156.10 | **162.80** | **81.35** | — | — | **84.60** | **68.42** | **11.20** | **79.71** | **12.98** | **2112.6** |

---

### 7.6 Pre-Layout vs. Post-Layout (RC Extracted) Verification Table

<p align="center">
  <img src="./images/calculations/10_pre_vs_post_layout_verification_table.png" alt="Pre vs Post Layout Verification and ADE Calculator Expressions" width="100%">
</p>

| Metric / Parameter | Pre-Layout (Schematic) | Post-Layout (av_extracted) | Delta (Δ) | Impact & Physical Interpretation |
| :--- | :---: | :---: | :---: | :--- |
| **Bitline Capacitance (CBL)** | 42.84 fF | **48.50 fF** | +13.2% | Metal3 fringe capacitance & diffusion proximity |
| **Wordline Capacitance (CWL)** | 25.23 fF | **31.92 fF** | +26.5% | Metal2 horizontal routing & gate contact overlap |
| **Inverter Propagation Delay (tpd)** | 30.50 ps | **35.12 ps** | +15.1% | Internal drain contact resistance (Rcont ≈ 20 Ω) |
| **Row Decoder Delay (tpd,dec)** | 131.56 ps | **148.20 ps** | +12.6% | Metal2 wordline RC delay network |
| **SRAM Read Access Delay (tpd,read)** | 185.90 ps | **214.50 ps** | +15.4% | Bitline wire resistance & sensing delay overhead |
| **SRAM Write Access Delay (tpd,write)** | 162.80 ps | **188.20 ps** | +15.6% | Bitline discharge series resistance (Rwire ≈ 12 Ω) |
| **Macro Contamination Delay (tcd)** | 98.90 ps | **112.40 ps** | +13.6% | Layout delays shift both min and max bounds safely |
| **Static Standby Power (Pstatic)** | 84.60 nW | **87.20 nW** | +3.1% | Substrate tap diode junction perimeter leakage |
| **Total Average Power (Pavg @ 100MHz)** | 79.71 µW | **92.45 µW** | +16.0% | Parasitic interconnect capacitance charging |
| **Power-Delay Product (PDP)** | 14.82 fJ | **19.83 fJ** | +33.8% | Combined delay and dynamic energy increase |
| **Energy-Delay Product (EDP)** | 2754.7 fJ·ps | **4253.5 fJ·ps** | +54.4% | Second-order delay exponent scaling |

---

### 7.7 Cadence Virtuoso ADE Calculator Syntax & Verification Equations

The exact LISP expressions used to extract all delay and power metrics in Cadence Virtuoso ADE L / Explorer:

```lisp
;; ==============================================================================
;; CADENCE VIRTUOSO ADE CALCULATOR EXPRESSIONS FOR SRAM METRIC EXTRACTION
;; Technology: GPDK 180nm | Supply: VDD = 1.8 V | Midpoint Threshold: 0.90 V
;; ==============================================================================

;; 1. Low-to-High Propagation Delay (tPLH)
delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "falling", ?nth1 1, 
      ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "rising", ?nth2 1)

;; 2. High-to-Low Propagation Delay (tPHL)
delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "rising", ?nth1 1, 
      ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "falling", ?nth2 1)

;; 3. Average Propagation Delay (tpd)
(delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "falling", ?nth1 1, 
       ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "rising", ?nth2 1) + 
 delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "rising", ?nth1 1, 
       ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "falling", ?nth2 1)) / 2.0

;; 4. Contamination Delay (tcd) - Minimum Propagation Delay
min(delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "either", ?nth1 1, 
          ?wf2 v("/Out0" ?result "tran"), ?val2 0.18, ?edge2 "either", ?nth2 1),
    delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "either", ?nth1 1, 
          ?wf2 v("/Out0" ?result "tran"), ?val2 1.62, ?edge2 "either", ?nth2 1))

;; 5. Output Rise Time (10% to 90% of VDD = 0.18V to 1.62V)
riseTime(v("/Out0" ?result "tran"), 0, nil, 1.80, nil, 10, 90)

;; 6. Output Fall Time (90% to 10% of VDD = 1.62V to 0.18V)
fallTime(v("/Out0" ?result "tran"), 0, nil, 1.80, nil, 10, 90)

;; 7. Total Average Power Dissipation (Pavg in Watts)
average(i("/V0/PLUS" ?result "tran") * 1.80)

;; 8. Static Standby Leakage Power (Pstatic measured during hold phase t = 390ns to 400ns)
value(i("/V0/PLUS" ?result "tran") * 1.80, 395e-9)

;; 9. Dynamic Switching Power (Pdynamic)
average(i("/V0/PLUS" ?result "tran") * 1.80) - value(i("/V0/PLUS" ?result "tran") * 1.80, 395e-9)

;; 10. Power-Delay Product (PDP in Joules)
average(i("/V0/PLUS" ?result "tran") * 1.80) * 
( (delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "falling", ?nth1 1, 
         ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "rising", ?nth2 1) + 
   delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "rising", ?nth1 1, 
         ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "falling", ?nth2 1)) / 2.0 )

;; 11. Energy-Delay Product (EDP in Joules * seconds)
average(i("/V0/PLUS" ?result "tran") * 1.80) * 
( ( (delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "falling", ?nth1 1, 
           ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "rising", ?nth2 1) + 
     delay(?wf1 v("/A0" ?result "tran"), ?val1 0.90, ?edge1 "rising", ?nth1 1, 
           ?wf2 v("/Out0" ?result "tran"), ?val2 0.90, ?edge2 "falling", ?nth2 1)) / 2.0 )**2 )
```

---
## 8. Physical Design & Sign-Off Verification (DRC, LVS, PEX & GDSII Stream)

The physical implementation was performed following industry-standard deep sub-micron design rules in **Cadence Virtuoso Layout Suite XL**.

```
+---------------------------------------------------------------------------------------+
|                         PHYSICAL VERIFICATION & TAPEOUT SIGN-OFF                      |
+---------------------------------------------------------------------------------------+
|  Tool / Stage          Deck / File                 Result Status      Details         |
+---------------------------------------------------------------------------------------+
|  Assura DRC            gpdk180_drc.rul             CLEAN (0 Errors)   0 Violations    |
|  Assura LVS            gpdk180_lvs.rul             CLEAN (100% Match) 0 Net/Dev Delta |
|  Assura RCX / QRC      gpdk180_pcells / RCX deck   CLEAN (RC Extr.)   av_extracted    |
|  Virtuoso XStream Out  sram_final_4_bit.gds        CLEAN (0 Errors)   Stream Level 7  |
+---------------------------------------------------------------------------------------+
```

### 8.1 Design Rule Checking (DRC)
* **Tool**: Cadence Assura DRC
* **Rule Deck**: `gpdk180_drc.rul`
* **Verification Scope**: Minimum gate width, gate length, poly endcap extension, active diffusion spacing, well enclosure of active area, contact-to-gate spacing, metal1/metal2/metal3 minimum width and pitch, and via enclosure rules.
* **Result**: **Clean (Zero DRC Violations)** across all 29 sub-blocks and the monolithic top-level macro.

### 8.2 Layout Versus Schematic (LVS)
* **Tool**: Cadence Assura LVS
* **Rule Deck**: `gpdk180_lvs.rul`
* **Verification Scope**: Device count equivalence (114 NMOS, 94 PMOS), net connectivity, port binding, and device geometrical parameter checks (channel width $W$, channel length $L$).
* **Result**: **100% Schematic-to-Layout Match**. All 208 MOS devices, 52 nets, and 17 I/O ports matched perfectly with zero floating pins, shorts, or un-routed nets.

### 8.3 Parasitic Extraction (PEX / RCX)
* **Tool**: Assura RCX / Cadence Quantus QRC
* **Extraction Mode**: Lumped and distributed Parasitic Resistance ($R$) and Capacitance ($C$).
* **View Generated**: `av_extracted` view.
* **Post-Layout Validation**: Back-annotated post-layout simulations in ADE-L confirmed that bitline parasitic wire capacitances ($C_{wire} \approx 24.0\text{ fF}$) and access gate parasitic resistances ($R_{wire} \approx 12.0\,\Omega$) do not violate read differential sensing margins ($\Delta V_{BL} \ge 150\text{ mV}$) or write flip times.

---

### 8.4 GDSII Stream Export & File Distribution

The physical tapeout database for the complete 4-Word x 4-Bit Synchronous SRAM Register File was successfully streamed out to the industry-standard **GDSII Stream Format (Stream Level 7)** using **Cadence Virtuoso XStream Out (`strmout`)**.

#### 📁 Direct File Access & Download Links
* 💾 **Primary GDSII Stream File (Repository Root)**: **[`sram_final_4_bit.gds`](./sram_final_4_bit.gds)** *(30,720 Bytes / 30.7 KB)*
* 📂 **Library GDSII Stream File Copy**: **[`SRAM_Register_File/sram_final_4_bit.gds`](./SRAM_Register_File/sram_final_4_bit.gds)**
* 🌐 **View File on GitHub**: **[View `sram_final_4_bit.gds` on GitHub](https://github.com/sohan2311/4-Word-x-4-Bit-Synchronous-SRAM-Register-File-with-Differential-Sense-Amplifiers/blob/main/sram_final_4_bit.gds)**
* ⬇️ **Direct Raw Binary Download**: **[Download Raw `sram_final_4_bit.gds`](https://github.com/sohan2311/4-Word-x-4-Bit-Synchronous-SRAM-Register-File-with-Differential-Sense-Amplifiers/raw/main/sram_final_4_bit.gds)**
* 📄 **Stream Out Translation Log**: **[`strmOut.log`](./strmOut.log)** *(0 Errors / 0 Warnings)*

#### GDSII Stream Translation Summary (from `strmOut.log`)

```
================================================================================
Product   : Virtuoso(R) XStream Out version 6.1.8-64b
Library   : SRAM_AUTO / SRAM_Register_File
Top Cell  : sram_final_4_bit
View      : layout
Stream    : sram_final_4_bit.gds
Tech Lib  : gpdk180 (Generic 180nm CMOS)
Layer Map : gpdk180.layermap
Status    : Translation completed with '0' error(s) and '0' warning(s) found.
================================================================================
```

| Translated Object Type | Count in GDSII File | Object Description |
| :--- | :---: | :--- |
| **Scalar Instances** | **48** | Hierarchical sub-cells (inverters, NOR gates, 6T cells, drivers) |
| **Array Instances** | **0** | Flattened regular macro structures |
| **Rectangles** | **348** | Physical layout geometries (diffusion, poly, metal1..3, vias) |
| **Text Labels / Pins** | **64** | Physical port and terminal text labels ($A_0, A_1, DIN, DOUT$, etc.) |
| **Total Cells Translated** | **11** | Hierarchical library cells and foundry PCells |
| **Peak Virtual Memory** | **9924 KB** | Stream-out footprint |
| **Translation Status** | **CLEAN** | **0 Errors, 0 Warnings** |

#### Complete Cell Hierarchy Embedded in GDSII File:
1. `pmos_CDNS_793954044460` (GPDK 180nm PMOS Primitive)
2. `nmos_CDNS_793954044461` (GPDK 180nm NMOS Primitive)
3. `inverter` (CMOS Inverter Sub-Cell)
4. `2_input_NOR` (2-Input CMOS NOR Sub-Cell)
5. `row_decoder_2to4` (2-to-4 Address Decoder Macro)
6. `sram_cell_6t` (6T SRAM Bitcell)
7. `precharge_unit` (Bitline Precharge & Equalization Unit)
8. `write_driver` (High-Drive Tri-State Write Driver)
9. `sense_amp` (Differential Latch Sense Amplifier)
10. `sram_core_array` (4-Word x 1-Bit Column Slice Array)
11. `sram_final_4_bit` (Top-Level 4-Word x 4-Bit Synchronous SRAM Macro)

#### Instructions to Inspect the GDSII File:
1. **Via Open-Source KLayout (Windows / Linux / macOS)**:
   ```bash
   klayout sram_final_4_bit.gds
   ```
2. **Via Cadence Virtuoso XStream In**:
   ```text
   File -> Import -> Stream
   Input File: sram_final_4_bit.gds
   Target Library: SRAM_GDS_IN
   Attach Tech Library: gpdk180
   Layer Map: gpdk180.layermap
   ```

---

## 9. Repository File Hierarchy

```
4-Word x 4-Bit Synchronous SRAM Register File with Differential Sense Amplifiers/
├── README.md                                    # Comprehensive Project Report, Calculations & Documentation
├── sram_final_4_bit.gds                         # Tapeout-Ready GDSII Stream File (30.7 KB, 0 Errors)
├── strmOut.log                                  # Virtuoso XStream Out Translation Log File
├── cds.lib                                      # Cadence Virtuoso Library Definition File
├── .gitignore                                   # Git configuration ignoring lock & temp files
├── SRAM_Register_File/                          # Cadence OpenAccess Clean Design Library
│   ├── cdsinfo.tag                              # OpenAccess Library Metadata
│   ├── sram_final_4_bit.gds                     # Design Library Copy of GDSII Stream File
│   ├── strmOut.log                              # Design Library Copy of Stream Out Log
│   ├── 2_input_NOR/                             # 2-Input NOR Gate [layout, schematic, symbol]
│   ├── 2_input_NOR_tb/                          # 2-Input NOR Testbench [schematic]
│   ├── inverter/                                # CMOS Inverter [layout, schematic, symbol]
│   ├── inverter_tb/                             # Inverter Testbench [schematic]
│   ├── precharge_unit/                          # Precharge & Equalization Unit [layout, schematic, symbol]
│   ├── precharge_tb/                            # Precharge Testbench [schematic]
│   ├── row_decoder_2to4/                       # 2-to-4 Address Decoder [layout, schematic, symbol]
│   ├── row_decoder_2to4_tb/                    # Address Decoder Testbench [schematic]
│   ├── sense_amp/                               # Differential Sense Amplifier [layout, schematic, symbol]
│   ├── sense_amp_tb/                            # Sense Amplifier Testbench [schematic]
│   ├── sram_cell_6t/                           # 6T SRAM Cell [layout, schematic, symbol]
│   ├── sram_cell_tb/                            # 6T SRAM Cell Testbench [schematic]
│   ├── sram_core_array/                         # 4×1 SRAM Column Array [layout, schematic, symbol]
│   ├── sram_core_array_tb/                      # 4×1 Column Testbench [schematic]
│   ├── sram_final_4_bit/                        # Top-Level 4×4 SRAM Macro [layout, schematic, symbol]
│   ├── sram_final_4_bit_tb/                     # Top-Level 4×4 SRAM Testbench [schematic]
│   ├── write_driver/                            # Write Driver Circuit [layout, schematic, symbol]
│   └── write_driver_tb/                         # Write Driver Testbench [schematic]
└── images/                                       # High-Resolution Visual Records (56+ Files)
    ├── System_Architecture.png                  # Full System Block & Architecture Diagram
    ├── 01_inverter/                             # Inverter Schematic, TB, Output, Layouts
    ├── 02_nor_gate/                             # NOR Gate Schematic, TB, Output, Layouts
    ├── 03_row_decoder/                          # Row Decoder Schematic, TB, Output, Layouts
    ├── 04_sram_cell_6t/                         # 6T Cell Schematic, Multiple TBs, Outputs, Layouts
    ├── 05_precharge_unit/                       # Precharge Schematic, TB, Outputs, Layouts
    ├── 06_write_driver/                         # Write Driver Schematic, TB, Output, Layouts
    ├── 07_sense_amplifier/                      # Sense Amp Schematic, TB, Output, Layouts
    ├── 08_sram_core_1bit_column/                # 1-Bit Column Schematic, TB, Output, Layouts
    └── 09_top_sram_4x4_register_file/           # Macro Schematic, TB, Transient Outputs, PDF Reports, Macro Layouts
```

---

## 10. Cadence Virtuoso Simulation & Replication Guide

To open, inspect, and simulate this design in your local Cadence Virtuoso environment:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/sohan2311/4-Word-x-4-Bit-Synchronous-SRAM-Register-File-with-Differential-Sense-Amplifiers.git
   cd "4-Word x 4-Bit Synchronous SRAM Register File with Differential Sense Amplifiers"
   ```

2. **Configure Cadence Environment**:
   * Ensure that the Cadence Generic 180nm PDK (`gpdk180`) is installed in your CAD environment.
   * Append the library path to your working `cds.lib`:
     ```text
     DEFINE SRAM_Register_File ./SRAM_Register_File
     ```

3. **Launch Cadence Virtuoso**:
   ```bash
   virtuoso &
   ```

4. **Open Library Manager**:
   * Navigate to library **`SRAM_Register_File`**.
   * Open top-level testbench **`sram_final_4_bit_tb`** (view: `schematic`).

5. **Execute Transient Simulation**:
   * Launch **ADE L** (`Launch -> ADE L`).
   * Choose simulator: `spectre`.
   * Set analysis: `tran` from `0` to `1.0 us` (step size: `100 ps`).
   * Plot signals: `/A0`, `/A1`, `/DIN0..DIN3`, `/WE`, `/PRE_bar`, `/SAE`, `/Out0..Out3`.
   * Click **Netlist and Run** to observe the transient read and write waveforms.

---

## Acknowledgements

I express my deepest gratitude and sincere appreciation to:
* **Dr. Rangababu P**, Associate Professor , Department of ECE , IIIT Kurnool , for his inspiring guidance, scholarly advice, technical critique, and continuous encouragement throughout the conception, design, and physical verification of this SRAM project.
* The **VLSI and Microelectronics Laboratory**, Department of Electronics and Communication Engineering, for providing high-performance computing clusters and infrastructure.
* **Cadence Design Systems**, for the comprehensive Electronic Design Automation (EDA) suite including **Virtuoso Schematic Editor**, **Virtuoso Layout Suite XL**, **Spectre Circuit Simulator**, **Assura DRC/LVS**, and **QRC Extraction tools**.
* Faculty members, research scholars, and peers in the VLSI & Microelectronics Division at Dept of ECE , IIIT Kurnool for their insightful discussions and support.

---

## Author Information

| Author & Academic Details | Affiliation & Contact |
| :--- | :--- |
| **Author**: Sohan Maity | **Institution**: IIITDM Kurnool, Andhra Pradesh |
| **Roll Number**: 523EC0001 | **Faculty**: Dr. Rangababu P |
| **Specialization**: VLSI & Microelectronics | **GitHub Profile**: [@sohan2311](https://github.com/sohan2311) |
| **Department**: Electronics & Communication Engg. | **Primary Contact**: [523ec0001@iiitk.ac.in](mailto:523ec0001@iiitk.ac.in) |
| **Foundry Node**: Cadence GPDK 180nm CMOS | **Alternate Email**: [sohan.maity2311@gmail.com](mailto:sohan.maity2311@gmail.com) |
