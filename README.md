# Two-Stage Miller Compensated Operational Amplifier in 90nm CMOS

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Technology](https://img.shields.io/badge/Technology-90nm%20CMOS-green.svg)
![Supply Voltage](https://img.shields.io/badge/Supply%20Voltage-1.2V-red.svg)
![Power](https://img.shields.io/badge/Power-108%C2%B5W-orange.svg)
![Phase Margin](https://img.shields.io/badge/Phase%20Margin-74%C2%B0-brightgreen.svg)

---

## 📌 Project Summary

This project presents the **design, simulation, and verification** of a **Two-Stage Miller Compensated Operational Amplifier** using **90nm CMOS Technology** in **Cadence Virtuoso**. The design achieves:

| Parameter | Value |
|-----------|-------|
| DC Gain | 60 dB |
| Unity Gain Bandwidth | 23 MHz |
| Phase Margin | 74° |
| Power Dissipation | 108 μW |
| Supply Voltage | 1.2 V |

---

## 📐 1. Why Two Stages?

### 1.1 Limitation of Single-Stage Amplifiers

A single-stage amplifier (e.g., Common Source or Cascode) has fundamental limitations:

| Limitation | Explanation |
|------------|-------------|
| **Low Gain** | Gain is limited to $g_m \cdot r_o$, typically 20-40 dB in 90nm CMOS |
| **Gain-Swing Trade-off** | Cascoding increases gain but consumes voltage headroom, reducing output swing |
| **Poor Isolation** | Direct feed-through from input to output |

### 1.2 Two-Stage Architecture Solution

The two-stage architecture separates gain and swing requirements:

| Stage | Function | Benefit |
|-------|----------|---------|
| **Stage 1** | Differential Input Stage | High gain, high CMRR, low noise |
| **Stage 2** | Common Source Stage | High output swing (rail-to-rail) |

**Total DC Gain:**
$$A_{v,total} = A_{v1} \times A_{v2}$$

Where:
$$A_{v1} = g_{m1}(r_{o2} \| r_{o4})$$
$$A_{v2} = g_{m7}(r_{o7} \| r_{o8})$$

---

## ⚡ 2. Why Miller Compensation?

### 2.1 The Stability Problem

A two-stage amplifier has **two dominant poles** ($P_1$ and $P_2$) located close in frequency.

**Problem:** When poles are close, the phase shift reaches $-180^\circ$ while the loop gain is still $>1$, causing **oscillation**.

**Barkhausen Criterion for Oscillation:**
$$|A\beta| = 1 \quad \text{and} \quad \angle A\beta = -180^\circ$$

### 2.2 Miller Effect and Pole Splitting

Miller compensation places a capacitor $C_c$ between the input and output of the second stage.

**Miller Effect:** The capacitor appears multiplied by the gain of the second stage:
$$C_{eff} = C_c(1 + A_{v2})$$

**Result - Pole Splitting:**

| Pole | Without Compensation | With Miller Compensation |
|------|---------------------|-------------------------|
| $P_1$ | Mid-frequency | **Pushed to very low frequency** (dominant pole) |
| $P_2$ | Mid-frequency | **Pushed to very high frequency** (non-dominant pole) |

$$P_1 \cong \frac{-G_{o1} G_{o2}}{g_{m7} C_c}$$
$$P_2 \cong \frac{g_{m7}}{C_L}$$

**Key Insight:** The poles are now widely separated ($P_1 \ll P_2$), ensuring stability.

### 2.3 The Right Half Plane (RHP) Zero Problem

The feed-forward path through $C_c$ creates a **Right Half Plane Zero**:
$$Z_1 = \frac{g_{m7}}{C_c}$$

**Why RHP Zero is harmful:** It adds phase lag (negative phase shift), degrading phase margin.

### 2.4 Nulling Resistor Solution

Adding a resistor $R_z$ in series with $C_c$ eliminates the RHP zero.

**Modified Zero Location:**
$$Z_1 = \frac{1}{C_c(1/g_{m7} - R_z)}$$

**Zero Cancellation Condition:**
$$R_z = \frac{1}{g_{m7}}$$

For $R_z > 1/g_{m7}$, the zero moves to the **Left Half Plane**, adding beneficial phase lead.

---

## 📊 3. Mathematical Design Procedure

### 3.1 Phase Margin Derivation

Phase Margin (PM) quantifies stability:
$$\text{PM} = 180^\circ + \angle A\beta(\omega_u)$$

At unity-gain frequency $\omega_u = g_{m1}/C_c$:

$$\text{PM} = 90^\circ - \tan^{-1}\left(\frac{\omega_u C_L}{g_{m7}}\right) + \tan^{-1}\left(\frac{\omega_u C_c}{g_{m7}}\right)$$

Substituting $\omega_u = g_{m1}/C_c$:

$$\text{PM} = 90^\circ - \tan^{-1}\left(\frac{g_{m1} C_L}{g_{m7} C_c}\right) + \tan^{-1}\left(\frac{g_{m1}}{g_{m7}}\right)$$

### 3.2 Design Target

Target PM = $60^\circ$ for well-conditioned stability (minimal ringing, ~10% overshoot).

Let $x = g_{m1}/g_{m7}$:

$$60^\circ = 90^\circ - \tan^{-1}\left(\frac{x C_L}{C_c}\right) + \tan^{-1}(x)$$

For $C_L = 1$ pF and $C_c = 0.44$ pF (typical), solving yields:
$$x \approx 0.25 \quad \Rightarrow \quad g_{m7} = 4g_{m1}$$

### 3.3 Compensation Capacitor ($C_c$)

Set $P_2 = 3 \times \text{GBW}$ for $60^\circ$ PM:
$$\frac{g_{m7}}{C_L} = 3 \left( \frac{g_{m1}}{C_c} \right)$$

Substituting $g_{m7} = 4g_{m1}$:
$$\frac{4g_{m1}}{C_L} = 3\frac{g_{m1}}{C_c} \quad \Rightarrow \quad C_c = \frac{3}{4}C_L$$

With $C_L = 1$ pF:
$$C_c = 0.75 \text{ pF} \quad \text{(selected 0.8 pF for margin)}$$

### 3.4 Transconductance Values

Target Unity Gain Bandwidth $F_u = 23$ MHz:
$$F_u = \frac{g_{m1}}{2\pi C_c}$$

$$g_{m1} = 2\pi C_c F_u = 2\pi \times (0.8 \times 10^{-12}) \times (23 \times 10^6) = 115.6\ \mu\text{S}$$

$$g_{m7} = 4 \times g_{m1} = 462.4\ \mu\text{S}$$

### 3.5 Nulling Resistor ($R_z$)

To cancel the RHP zero:
$$R_z = \frac{1}{g_{m7}} = \frac{1}{462.4 \times 10^{-6}} = 2.16\ \text{k}\Omega$$

Selected standard value: **$R_z = 2.2\ \text{k}\Omega$**

---

## 🔬 4. $g_m/I_d$ Methodology for Transistor Sizing

### 4.1 Why $g_m/I_d$?

The $g_m/I_d$ methodology is **technology-independent** and works across different CMOS nodes. It relates transistor bias to performance.

### 4.2 Key Relationships

| Region | $g_m/I_d$ | Equation |
|--------|-----------|----------|
| Strong Inversion | $< 10$ V$^{-1}$ | $\frac{g_m}{I_D} = \frac{2}{V_{GS} - V_{TH}}$ |
| Moderate Inversion | $10-20$ V$^{-1}$ | (Target region for this design) |
| Weak Inversion | $> 20$ V$^{-1}$ | $\frac{g_m}{I_D} = \frac{1}{nV_T}$ |

**Overdrive voltage:**
$$V_{ov} = V_{GS} - V_{TH} = \frac{2}{(g_m/I_d)}$$

For target $g_m/I_d = 10$ V$^{-1}$:
$$V_{ov} = \frac{2}{10} = 0.2\ \text{V}$$

### 4.3 Width Calculation

Using the square-law equation (strong inversion as starting point):
$$I_D = \frac{1}{2} \mu C_{ox} \frac{W}{L} V_{ov}^2$$

Solving for width:
$$W = \frac{2 I_D L}{\mu C_{ox} V_{ov}^2}$$

### 4.4 Bias Current Calculation

$$I_{bias} = \frac{g_m}{(g_m/I_d)}$$

| Stage | $g_m$ | $g_m/I_d$ | $I_D$ |
|-------|-------|-----------|-------|
| First Stage ($M_1, M_2$) | 115.6 μS | 10 V$^{-1}$ | 11.56 μA each |
| Tail Current ($M_5$) | - | - | 23.12 μA |
| Second Stage ($M_7, M_8$) | 462.4 μS | 10 V$^{-1}$ | 46.24 μA each |

### 4.5 Process Parameters (gpdk090 90nm CMOS)

| Parameter | NMOS | PMOS |
|-----------|------|------|
| $\mu C_{ox}$ | 300 μA/V² | 100 μA/V² |
| $V_{TH}$ (nominal) | 0.35 V | -0.38 V |

**Fixed length:** $L = 500$ nm (for high output resistance $r_o$)

### 4.6 Calculated W/L Ratios

| Transistor | Function | Type | $I_D$ | W/L (μm/μm) |
|------------|----------|------|-------|--------------|
| M1, M2 | Differential Pair | NMOS | 11.56 μA | 1.4 / 0.5 |
| M3, M4 | Active Load | PMOS | 11.56 μA | 3.2 / 0.5 |
| M5 | Tail Current Source | NMOS | 23.12 μA | 2.9 / 0.5 |
| M6 | Bias Reference | NMOS | 23.12 μA | 2.9 / 0.5 |
| M7 | Second Stage Driver | PMOS | 46.24 μA | 5.8 / 0.5 |
| M8 | Second Stage Load | NMOS | 46.24 μA | 12.5 / 0.5 |

---

## 🛠️ 5. Implementation in Cadence Virtuoso

### 5.1 Schematic Design

The circuit was implemented using the `gpdk090` PDK with the following components:

1. **Differential Input Stage:**
   - NMOS differential pair (M1, M2) with matched dimensions
   - PMOS current mirror active load (M3, M4)
   - NMOS tail current source (M5)

2. **Second Stage:**
   - PMOS common-source driver (M7)
   - NMOS active load (M8)

3. **Biasing Network:**
   - Current mirror (M5, M6) for stable biasing

4. **Compensation Network:**
   - Miller capacitor $C_c = 0.8$ pF
   - Nulling resistor $R_z = 2.2$ kΩ

### 5.2 Verification of Saturation Region

For each transistor, verify:
$$V_{DS} \geq V_{GS} - V_{TH} = V_{ov}$$

Minimum $V_{DS}$ for saturation includes 50-100 mV margin for velocity saturation effects.

---

## 📈 6. Simulation and Verification

### 6.1 DC Analysis (Operating Point)

| Measured Parameter | Value |
|--------------------|-------|
| $I_{d1} = I_{d2}$ | 11.8 μA |
| $I_{tail}$ | 23.6 μA |
| $I_{d7}$ | 47.1 μA |
| Total Current | ≈90 μA |
| **Power Dissipation** | $1.2V \times 90\mu A = 108\ \mu\text{W}$ |

✅ All transistors confirmed in saturation region.

### 6.2 AC Analysis (Frequency Response)

**Setup:**
- AC source: 1V magnitude at non-inverting input
- Sweep: 1 Hz to 1 GHz (logarithmic)

**Results:**

| Parameter | Value |
|-----------|-------|
| DC Gain | 60 dB |
| Unity Gain Bandwidth | 23 MHz |
| Phase at Unity Gain | $-106^\circ$ |
| **Phase Margin** | $180^\circ + (-106^\circ) = 74^\circ$ |

**Interpretation:** PM = $74^\circ$ indicates excellent stability with minimal overshoot.

### 6.3 Transient Analysis

| Parameter | Value |
|-----------|-------|
| Positive Slew Rate | 8.2 V/μs |
| Negative Slew Rate | 7.9 V/μs |
| Average Slew Rate | 8.05 V/μs |
| Settling Time (0.1%) | 85 ns |

---

## 📁 7. Repository Structure
