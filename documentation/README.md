# 🎛️ Sallen-Key Active Audio Filter Analysis (NE5532)

## 📌 Overview
This section contains a comprehensive theoretical, mathematical, and practical engineering analysis of a **2nd-Order Sallen-Key Active High-Pass (HP) and Low-Pass (LP) Audio Filter** built with the **NE5532** low-noise operational amplifier.

---

## 🚀 Key System Specifications

* **Topology:** 2nd-Order Sallen-Key Active Filter (Cascaded HPF + LPF)
* **Active Element:** NE5532 Dual Low-Noise Op-Amp
* **High-Pass Cutoff ($f_c$):** $19.1\text{ Hz} \rightarrow 189.4\text{ Hz}$ ($Q \approx 0.707$, Butterworth)
* **Low-Pass Cutoff ($f_c$):** $1.01\text{ kHz} \rightarrow 10.18\text{ kHz}$ ($Q \approx 0.707$, Butterworth)
* **Passband Gain:** $+3.84\text{ dB}$ ($A_v = 1.556\text{ V/V}$)
* **Power Supply:** Dual Supply ($\pm 12\text{V}$ to $\pm 15\text{V}$ DC)

---

## 📂 Section Structure

| File / Topic | Description |
| :--- | :--- |
| **`01_Circuit_Overview`** | Full signal path, block breakdown, and component specs |
| **`02_High_Pass_Stage`** | HP filter equations, transfer functions, and gain calculations |
| **`03_Low_Pass_Stage`** | LP filter operation, impedance loading, and unity-gain buffer |
| **`04_Node_Analysis`** | AC response across nodes at $10\text{ Hz}$, $1\text{ kHz}$, and $20\text{ kHz}$ |
| **`05_Troubleshooting`** | Real-world schematic bugs, power decoupling, and test questions |

---

## 🧮 Fundamental Formulas

* **High-Pass Cutoff Frequency:**
  $$f_c = \frac{1}{2\pi R C}$$

* **Low-Pass Cutoff Frequency:**
  $$f_c = \frac{1}{2\pi \sqrt{R_1 R_2 C_{\text{top}} C_{\text{gnd}}}}$$

* **HP Stage Non-Inverting Gain:**
  $$A_v = 1 + \frac{R_5}{R_6}$$

---

## 🧪 Section 16: Final Understanding Check

This repository section includes 11 practical questions covering:
1. **Sallen-Key Feedback Mechanics:** Differences between active bootstrapping and negative feedback loops.
2. **Parametric Sensitivity:** Cutoff frequency shift and $Q$-factor degradation due to component variations.
3. **Hardware Troubleshooting:** Rail latch-up diagnosis, ground hum reduction, and output load drive limits.

---
*Maintained for circuit design verification, LTspice simulation comparison, and breadboard prototyping.*
