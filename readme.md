# 90nm CMOS Two-Stage Miller Compensated Op-Amp

[cite_start]This repository contains the design, mathematical modeling, and Cadence Virtuoso simulation results for a high-performance, low-voltage Two-Stage Operational Amplifier[cite: 63]. [cite_start]The project benchmarks a custom 90nm CMOS design against the industry-standard IC 741, demonstrating a 1000x improvement in power efficiency[cite: 67, 70].

---

## Project Specifications
* [cite_start]**Technology Node:** 90nm CMOS (gpdk090) [cite: 63]
* [cite_start]**Supply Voltage:** 1.2V [cite: 64]
* [cite_start]**DC Gain:** 60 dB [cite: 69]
* [cite_start]**Unity Gain Bandwidth (UGB):** 23 MHz [cite: 69]
* [cite_start]**Phase Margin:** 74° [cite: 69]
* [cite_start]**Power Consumption:** 108 µW [cite: 70]

---

## Architecture and Design Methodology

### 1. Why a Two-Stage Architecture?
[cite_start]A single-stage amplifier cannot simultaneously achieve high gain and large output swing in 90nm technology due to reduced voltage headroom[cite: 158]. [cite_start]This design separates these functions: **Stage 1 (Differential Pair)** provides high gain and common-mode rejection, while **Stage 2 (Common-Source)** provides additional gain and a large output swing[cite: 159, 169, 170].

### 2. Miller Compensation and Pole Splitting
[cite_start]Two-stage amplifiers are inherently unstable because they introduce two significant poles[cite: 229, 646]. [cite_start]Miller compensation uses a capacitor ($C_c$) between the input and output of the second stage to achieve **Pole Splitting**[cite: 275, 277]. [cite_start]This pushes the dominant pole ($P_1$) to a lower frequency and the non-dominant pole ($P_2$) to a higher frequency, ensuring stability[cite: 279, 659, 660].

### 3. The Nulling Resistor Solution
[cite_start]The Miller capacitor creates a feed-forward path that introduces a Right Half Plane (RHP) Zero[cite: 288, 680]. [cite_start]This zero adds phase lag and degrades the phase margin[cite: 291, 689]. [cite_start]By placing a **Nulling Resistor ($R_z$)** in series with $C_c$ and setting it approximately equal to $1/g_{m7}$, the zero is pushed to infinity to restore stability[cite: 292, 294, 698].

---

## Implementation in Cadence Virtuoso
[cite_start]The circuit was implemented using the **$g_m/I_d$ methodology** to determine transistor dimensions analytically[cite: 784, 788]. [cite_start]All simulations were performed in the Cadence Virtuoso environment[cite: 845]:

* **DC Analysis:** Verified that all transistors operate in the saturation region[cite: 891, 892].
* [cite_start]**AC Analysis:** Extracted the Bode plot to measure gain, bandwidth, and phase margin[cite: 900, 904].
* [cite_start]**Transient Analysis:** Measured the slew rate (8.05 V/µs) and settling behavior[cite: 905, 918, 989].

---

## Comparative Performance Results

| Parameter | Custom 90nm Design | IC 741 (Traditional) |
| :--- | :--- | :--- |
| **Technology** | [cite_start]90nm CMOS [cite: 1031] | [cite_start]Bipolar Junction Transistor [cite: 1031] |
| **Supply Voltage** | [cite_start]1.2 V [cite: 1031] | [cite_start]±15 V [cite: 1031] |
| **Power Dissipation** | [cite_start]108 µW [cite: 1031] | [cite_start]≈100 mW [cite: 1031] |
| **Gain Bandwidth** | [cite_start]23 MHz [cite: 1031] | [cite_start]1 MHz [cite: 1031] |
| **Phase Margin** | [cite_start]74° [cite: 1031] | [cite_start]≈90° [cite: 1031] |
| **Slew Rate** | [cite_start]8.05 V/µs [cite: 1031] | [cite_start]0.5 V/µs [cite: 1031] |

---

## Project Authors
* Pennam Yaswanth (N200481) [cite: 10, 26]
* [cite_start]Gadamsetty Sukanya (N200424) [cite: 10, 26]
* [cite_start]Kornu Bharath Kumar (N200046) [cite: 10, 26]
* **Project Guide:** Dr. Charles Stud [cite: 12, 28]
