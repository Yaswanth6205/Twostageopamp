# Two-Stage Miller Compensated Operational Amplifier in 90nm CMOS

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Technology](https://img.shields.io/badge/Technology-90nm%20CMOS-green.svg)
![Supply Voltage](https://img.shields.io/badge/Supply%20Voltage-1.2V-red.svg)
![Power](https://img.shields.io/badge/Power-108%C2%B5W-orange.svg)

## 📌 Project Overview

This project presents the **design, simulation, and implementation** of a **Two-Stage Miller Compensated Operational Amplifier** using **90nm CMOS Technology** in the **Cadence Virtuoso** environment. The design achieves high DC gain, excellent stability, and ultra-low power consumption, making it suitable for modern portable electronics.

### 🎯 Key Specifications

| Parameter | Achieved Value | Target |
|-----------|---------------|--------|
| **Supply Voltage** | 1.2 V | 1.2 V |
| **DC Gain** | 60 dB | ≥ 60 dB |
| **Unity Gain Bandwidth** | 23 MHz | ≥ 20 MHz |
| **Phase Margin** | 74° | ≥ 60° |
| **Power Dissipation** | 108 μW | < 200 μW |
| **Slew Rate** | 8.05 V/μs | ≥ 5 V/μs |
| **Load Capacitance** | 1 pF | 1 pF |
| **CMRR** | 75 dB | - |
| **PSRR+** | 70 dB | - |

### ✨ Key Features

- ✅ **Miller Compensation** with Nulling Resistor for pole splitting and RHP zero cancellation
- ✅ **High DC Gain** of 60 dB using two-stage architecture
- ✅ **Excellent Stability** with 74° Phase Margin (well above 60° requirement)
- ✅ **Ultra-Low Power** consumption of only 108 μW (≈1000x more efficient than IC 741)
- ✅ **Wide Bandwidth** of 23 MHz (23x faster than traditional IC 741)
- ✅ **Low Voltage Operation** at 1.2V single supply
- ✅ **Rail-to-Rail Output Swing**

---

## 📐 Design Methodology

### Circuit Architecture

The design consists of three main functional blocks:

1. **Differential Input Stage** - NMOS differential pair with PMOS current mirror active load for high gain and CMRR
2. **Common Source Second Stage** - PMOS driver with NMOS active load for high output swing
3. **Biasing Network** - Current mirrors for stable biasing across PVT variations

### Compensation Strategy

#### Problem Statement
Two-stage amplifiers inherently have two dominant poles close in frequency, causing 180° phase shift while gain > 1 → instability/oscillation.

#### Solution: Miller Compensation + Nulling Resistor

| Technique | Purpose | Mathematical Formulation |
|-----------|---------|-------------------------|
| **Miller Capacitor ($C_c$)** | Pole splitting - pushes $P_1$ lower, $P_2$ higher | $C_c = \frac{3}{4}C_L = 0.75$ pF |
| **Nulling Resistor ($R_z$)** | Eliminates RHP zero that degrades stability | $R_z = \frac{1}{g_{m7}} \approx 2.2$ kΩ |

### Key Mathematical Derivations

#### Pole Locations
$$P_1 \cong \frac{-G_{o1} G_{o2}}{g_{m7} C_c} \quad \text{(Dominant Pole)}$$
$$P_2 \cong \frac{g_{m7}}{C_L} \quad \text{(Non-Dominant Pole)}$$

#### Gain Bandwidth Product
$$\text{GBW} = \frac{g_{m1}}{C_c} = \omega_u = 23 \text{ MHz}$$

#### Phase Margin
$$\text{PM} = 90^{\circ} - \tan^{-1}\left(\frac{g_{m1} C_L}{g_{m7} C_c}\right) + \tan^{-1}\left(\frac{g_{m1}}{g_{m7}}\right) = 74^{\circ}$$

#### Transistor Sizing ($g_m/I_d$ Methodology)

| Transistor | Function | W/L Ratio (μm/μm) |
|------------|----------|-------------------|
| M1, M2 | Differential Pair (NMOS) | 1.4 / 0.5 |
| M3, M4 | Active Load (PMOS) | 3.2 / 0.5 |
| M5 | Tail Current Source (NMOS) | 2.9 / 0.5 |
| M7 | Second Stage Driver (PMOS) | 5.8 / 0.5 |
| M8 | Second Stage Load (NMOS) | 12.5 / 0.5 |

---

## 📊 Simulation Results

### AC Analysis - Gain Response

| Parameter | Value |
|-----------|-------|
| DC Gain | 60 dB |
| Unity Gain Bandwidth | 23 MHz |
| Gain Roll-off | -20 dB/decade |

### Phase Margin Analysis

| Parameter | Value |
|-----------|-------|
| Phase at Unity Gain | -106° |
| **Phase Margin** | **74°** |
| Stability Status | Excellent |

### Transient Analysis

| Parameter | Value |
|-----------|-------|
| Positive Slew Rate | 8.2 V/μs |
| Negative Slew Rate | 7.9 V/μs |
| Average Slew Rate | 8.05 V/μs |
| Settling Time (0.1%) | 85 ns |

---

## 🔬 Comparative Analysis: Custom 90nm vs. IC 741

| Parameter | Custom 90nm Design | Traditional IC 741 | Improvement |
|-----------|-------------------|-------------------|-------------|
| **Technology** | 90nm CMOS | Bipolar (BJT) | - |
| **Supply Voltage** | **1.2 V** | ±15 V | 25x reduction |
| **Power Dissipation** | **108 μW** | ≈100 mW | **≈1000x better** |
| **Gain Bandwidth** | **23 MHz** | 1 MHz | **23x faster** |
| **Phase Margin** | **74°** | ≈90° | Excellent |
| **Slew Rate** | **8.05 V/μs** | 0.5 V/μs | **16x faster** |
| **Area** | ≈600 μm² | ≈1 mm² | **≈1600x smaller** |

### Key Insights from Comparison

- ✅ **1000× more power-efficient** - Suitable for battery-operated IoT and biomedical devices
- ✅ **23× higher bandwidth** - Can process higher frequency signals
- ✅ **16× faster slew rate** - Better transient response
- ✅ **Operates at 1.2V** vs ±15V - Compatible with modern low-voltage systems

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Cadence Virtuoso** | Schematic design, simulation, and verification |
| **gpdk090** | 90nm CMOS Process Design Kit |
| **ADE L** | Analog Design Environment for simulation setup |
| **ViVA** | Waveform visualization and analysis |
| **NI Multisim** | IC 741 comparative analysis |

---

## 📁 Repository Structure
