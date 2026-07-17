# Two-Stage CMOS Operational Amplifier Design

This repository contains the design methodology, schematic architectures, and small-signal frequency response analyses for a high-gain, wide-bandwidth **Two-Stage CMOS Operational Amplifier (Op-Amp)**. 

## 📋 Design Specifications & Requirements
The circuit architecture is systematically synthesized to meet the following performance criteria:
*   **DC Voltage Gain ($A_v$):** High gain target achieved via a cascaded multi-stage architecture.
*   **Gain Bandwidth Product ($GBW$):** Wideband operation optimized via transconductance configuration.
*   **Phase Margin ($PM$):** Frequency compensation tailored to guarantee closed-loop stability ($\ge 60^\circ$).
*   **Slew Rate ($SR$):** Dynamic transient response constrained by tail current and compensation loading.
*   **Power Dissipation ($P_{diss}$):** Strict upper bound optimization on total current consumption.
*   **Input Common-Mode Range ($ICMR$):** Maximum and minimum voltage limits defined by input pair saturation requirements.

---

## 🏗️ Circuit Architecture
The topology is split into two distinct, cascading gain stages:

### 1. First Stage: Differential Amplifier
*   **Input Pair:** NMOS differential pair managing input transconductance ($g_{m1,2}$).
*   **Active Load:** PMOS current mirror providing differential-to-single-ended conversion and maximizing first-stage output impedance ($r_{o}$).
*   **Tail Current Source:** NMOS transistor setting the total quiescent bias current for the input branch.

### 2. Second Stage: Common-Source Amplifier
*   **Driver:** PMOS common-source transistor providing additional high voltage gain.
*   **Active Load:** NMOS current source acting as the output stage sink.

---

## 📈 Frequency Response & Stability Methodology

A primary challenge of a two-stage op-amp is its inherent nature as a **two-pole system**, which threatens loop stability under negative feedback. The engineering steps applied below resolve this issue:

### The Multi-Pole Challenge
*   **Uncompensated System:** The output nodes of the first and second stages generate two distinct poles ($P_1$ and $P_2$) close to each other in frequency. 
*   **Stability Risk:** Without compensation, the phase shifts toward $-180^\circ$ near the crossover frequency, destroying the phase margin.

### Miller Compensation Strategy
*   **The Miller Effect:** A small compensation capacitor ($C_c$) is bridged across the high-gain inverting second stage.
*   **Capacitance Multiplication:** The effective capacitance at the intermediate node is scaled by the stage gain ($A$), behaving as $C_{eff} = C_c(1 + A)$. This saves physical silicon area while offering massive stabilization properties.

### Dominant Pole Splitting
*   **Pole 1 ($P_1$ Displacement):** Pushed down drastically to lower frequencies to serve as the dominant pole, ensuring a controlled $-20\text{ dB/decade}$ roll-off early on.
*   **Pole 2 ($P_2$ Displacement):** Pushed out to high frequencies well past the unity-gain crossover point ($GBW$).
*   **System Emulation:** The technique successfully forces the two-stage circuit to act safely as a single-pole system over its operating bandwidth.

---

## 🛠️ Step-by-Step Design Flowchart

1.  **Compensation Selection ($C_c$):** Determine minimum $C_c$ relative to load capacitance ($C_L$) to fulfill phase margin targets.
2.  **Tail Current Sizing ($I_5$):** Establish base bias current derived explicitly from Slew Rate limits.
3.  **Input Pair Sizing ($M_1, M_2$):** Calculate required input transconductance ($g_{m1}$) from $GBW$ parameters to solve for $(W/L)_{1,2}$ ratios.
4.  **Load Pair Sizing ($M_3, M_4$):** Size active loads utilizing $ICMR$ maximum voltage boundary conditions.
5.  **Output Driver Sizing ($M_6$):** Size the common-source transistor to place the non-dominant pole sufficiently high, satisfying pole-splitting constraints ($g_{m6} \ge 10 \cdot g_{m1}$).
6.  **Current Mirror & Biasing ($M_5, M_7, M_8$):** Size biasing transistors using proper current-mirror ratios to match branch distributions accurately.



