# IN-DEPTH SCHEMATIC ANALYSIS: SALLEN-KEY AUDIO FILTER (NE5532)

## 1. COMPLETE CIRCUIT OVERVIEW

**Signal Path:**
`Audio_Input` $\rightarrow$ HP Filter $\rightarrow$ HP Op-Amp Stage $\rightarrow$ LP Filter $\rightarrow$ LP Buffer Stage $\rightarrow$ `Audio_Output`

- **High-Pass (HP) Section:** Located on the left, comprising capacitors $C_1, C_2$ ($150\text{ nF}$ each), resistors $R_3, R_4$ ($5.6\text{ k}\Omega$ each), dual-gang potentiometer $VR1A/VR1B$ ($50\text{ k}\Omega$), gain-setting resistors $R_5$ ($1\text{ k}\Omega$) and $R_6$ ($1.8\text{ k}\Omega$), and the first NE5532 op-amp stage.
- **Low-Pass (LP) Section:** Located on the right, comprising dual-gang potentiometer $VR1A/VR1B$ ($10\text{ k}\Omega$), series resistors $R_3, R_4$ ($1\text{ k}\Omega$ each), $R_5$ ($220\ \Omega$), capacitors $C_1, C_2, C_3$ ($10\text{ nF}$ each), and the second NE5532 op-amp stage configured as a voltage follower.
- **Op-Amp Stages:**
  1. **HP Stage:** Non-inverting Sallen-Key active filter with a gain of $A_v = 1.556\text{ V/V}$ ($+3.84\text{ dB}$).
  2. **LP Stage:** Unity-gain ($A_v = 1\text{ V/V}$, $0\text{ dB}$) non-inverting Sallen-Key buffer.
- **Signal Direction:** Left-to-right: `Audio_Input` $\rightarrow$ HP filter $\rightarrow$ HP op-amp output (pin 7) $\rightarrow$ LP filter $\rightarrow$ LP buffer op-amp output (pin 7) $\rightarrow$ `Audio_Output`.
- **Frequency Control:** HP cutoff is controlled by the left $50\text{ k}\Omega$ dual pot; LP cutoff is controlled by the right $10\text{ k}\Omega$ dual pot.
- **Section Interaction:** The HP stage ends with an op-amp output (pin 7), providing near-zero output impedance. This completely isolates the HP section from the LP section, preventing loading.
- **Functional Breakdown:**
  * *Filtering:* $C_1, C_2, R_3, R_4, VR1$ (HP); $VR1, R_3, R_4, R_5, C_1, C_2, C_3$ (LP).
  * *Buffering/Amplification:* U1B (HP) and U1B (LP).
  * *Frequency Adjustment:* Dual $50\text{ k}\Omega$ pot (HP); Dual $10\text{ k}\Omega$ pot (LP).
  * *Biasing/Gain:* $R_5$ ($1\text{ k}\Omega$) and $R_6$ ($1.8\text{ k}\Omega$) in HP stage.
  * *Active Feedback:* HP pin 7 back to $VR1A$ (HP); LP pin 7 back to $C_1/C_2$ junction (LP).

---

## 2. HIGH-PASS (HP) SECTION

### Component Breakdown:
- $C_1$ ($150\text{ nF}$): AC coupling capacitor. Blocks DC offsets and attenuates low frequencies. Connected between `Audio_Input` and Node HP1. Increasing value lowers $f_c$.
- $C_2$ ($150\text{ nF}$): Second reactive element forming the 2nd pole ($-12\text{ dB/octave}$). Connected between Node HP1 and Pin 5 ($+$ input). Increasing value lowers $f_c$.
- $R_3$ ($5.6\text{ k}\Omega$): Sets minimum resistance for first RC branch. Connected between Node HP1 and $VR1A$ terminal. Limits maximum cutoff frequency.
- $R_4$ ($5.6\text{ k}\Omega$): Sets minimum resistance for second RC branch. Connected between Pin 5 and $VR1B$ terminal. Prevents non-inverting input from shorting directly.
- $VR1A$ ($50\text{ k}\Omega$ Dual Pot, A): Variable resistor in rheostat mode. Connected between $R_3$ and HP pin 7 feedback line. Adjusts cutoff frequency.
- $VR1B$ ($50\text{ k}\Omega$ Dual Pot, B): Variable resistor in rheostat mode. Connected between $R_4$ and $R_5/R_6$ divider node. Adjusts second pole frequency simultaneously.
- $R_5$ ($1\text{ k}\Omega$): Feedback resistor. Connected between pin 7 (output) and pin 6 ($-$ input). Sets op-amp closed-loop gain along with $R_6$.
- $R_6$ ($1.8\text{ k}\Omega$): Ground gain resistor. Connected between pin 6 ($-$ input) and ground. Sets gain and $Q$ factor.
- **U1B / NE5532:** Operational amplifier. Provides $+3.84\text{ dB}$ gain, active Sallen-Key bootstrapping, and low output impedance.

### HP Signal Path Step-by-Step:
1. `Audio_Input` feeds full AC audio spectrum.
2. Signal passes through $C_1$ to Node HP1; low frequencies encounter high capacitive reactance.
3. Active feedback from op-amp pin 7 injects energy via $VR1A + R_3$ into Node HP1 (sharpens knee).
4. Signal passes through $C_2$ to Node HP2 (Pin 5); further attenuated below cutoff.
5. Signal enters U1B non-inverting input (Pin 5).
6. Op-amp U1B amplifies by $1.556\times$ ($+3.84\text{ dB}$) and outputs at Pin 7 to drive the LP section.

---

## 3. HP CUTOFF FREQUENCY

### Transfer Function:
$$H(s) = \frac{A_v \cdot s^2}{s^2 + s \cdot \left(\frac{3 - A_v}{R \cdot C}\right) + \frac{1}{R^2 \cdot C^2}}$$

### Cutoff Formula:
$$f_c = \frac{1}{2\pi R C}$$

Where $R = R_3 + VR1A = R_4 + VR1B$, and $C = C_1 = C_2 = 150\text{ nF}$.

### Mathematical Derivation / Values:
- $C = 150\text{ nF} = 1.5 \times 10^{-7}\text{ F}$
- $R_{\text{min}} = 5.6\text{ k}\Omega = 5600\ \Omega$
- $R_{\text{max}} = 5.6\text{ k}\Omega + 50\text{ k}\Omega = 55.6\text{ k}\Omega = 55,600\ \Omega$

### Calculations:
- **Max Cutoff ($VR1 = 0\ \Omega$):**
  $$f_{c,\text{max}} = \frac{1}{2\pi \times 5600 \times 1.5 \times 10^{-7}} \approx 189.4\text{ Hz}$$
- **Min Cutoff ($VR1 = 50\text{ k}\Omega$):**
  $$f_{c,\text{min}} = \frac{1}{2\pi \times 55600 \times 1.5 \times 10^{-7}} \approx 19.1\text{ Hz}$$

**Assumptions:** Ideal op-amp, perfect dual-potentiometer resistance tracking, zero source impedance.

---

## 4. HP OP-AMP STAGE

- **Inputs:** Pin 5 = Non-inverting ($+$), Pin 6 = Inverting ($-$)-
- **Feedback:** Negative feedback through $R_5$ ($1\text{ k}\Omega$) to Pin 6; positive active feedback from Pin 7 to $VR1A$.
- **Configuration:** Non-inverting amplifier with active Sallen-Key feedback.
- **Gain Derivation:**
  $$A_v = 1 + \frac{R_5}{R_6} = 1 + \frac{1000}{1800} = 1.5556\text{ V/V} \quad (+3.84\text{ dB})$$
- **Quality Factor ($Q$):**
  $$Q = \frac{1}{3 - A_v} = \frac{1}{3 - 1.5556} = \frac{1}{1.4444} \approx 0.692 \approx 0.707 \quad \text{(Butterworth alignment)}$$
- **Impact of Op-Amp:** Provides Butterworth response, prevents loading by LP stage. If removed, circuit becomes a passive RC filter with severe attenuation and soft roll-off ($Q \le 0.5$).

---

## 5. LOW-PASS (LP) SECTION

### Component Breakdown:
- $VR1A$ ($10\text{ k}\Omega$ Dual Pot, A): First variable series resistance for LP stage.
- $R_3$ ($1\text{ k}\Omega$): Minimum series resistance for first LP branch.
- $VR1B$ ($10\text{ k}\Omega$ Dual Pot, B): Second variable series resistance for LP stage.
- $R_4$ ($1\text{ k}\Omega$): Minimum series resistance for second LP branch.
- $R_5$ ($220\ \Omega$): Series stopper/limiting resistor placed before non-inverting input.
- $C_1$ ($10\text{ nF}$) & $C_2$ ($10\text{ nF}$): Connected in parallel from Node LP1 to Output Pin 7 (Total $C_{\text{top}} = 20\text{ nF}$).
- $C_3$ ($10\text{ nF}$): Shunt capacitor connected from Node LP2 to Ground ($C_{\text{gnd}} = 10\text{ nF}$).
- **U1B / NE5532:** Unity-gain buffer (Pin 6 tied directly to Pin 7).
---

## 6. LP FILTER OPERATION

**Capacitive Reactance:** 
$$X_c = \frac{1}{2\pi f C}$$

- **Low Frequencies ($f \ll f_c$):**
  * Capacitors act as open circuits ($X_c \to \infty$).
  * Current flows directly through series resistors $VR1A \rightarrow R_3 \rightarrow VR1B \rightarrow R_4 \rightarrow R_5$ into Pin 5.
  * Output matches input voltage ($A_v = 1\text{ V/V}$).

- **High Frequencies ($f \gg f_c$):**
  * Capacitors act as low-impedance shorts ($X_c \to 0$).
  * $C_3$ shunts high frequencies at Node LP2 to Ground.
  * $C_1 \parallel C_2$ shunts high frequencies at Node LP1 back to op-amp output.
  * Voltage reaching Pin 5 is attenuated at $-12\text{ dB/octave}$ ($-40\text{ dB/decade}$).

---

## 7. LP CUTOFF FREQUENCY

### Formula:
$$f_c = \frac{1}{2\pi \sqrt{R_1 R_2 C_{\text{top}} C_{\text{gnd}}}}$$

### Parameters:
- $R_1 = VR1A + R_3 = 1\text{ k}\Omega \text{ to } 11\text{ k}\Omega$
- $R_2 = VR1B + R_4 + R_5 = 1.22\text{ k}\Omega \text{ to } 11.22\text{ k}\Omega$
- $C_{\text{top}} = C_1 + C_2 = 20\text{ nF}$
- $C_{\text{gnd}} = C_3 = 10\text{ nF}$

### Calculations:
- **Min LP Cutoff ($VR1 = 10\text{ k}\Omega$, $R_1 = 11\text{ k}\Omega$, $R_2 = 11.22\text{ k}\Omega$):**
  $$\sqrt{R_1 R_2} \approx 11,109.5\ \Omega$$
  $$f_{c,\text{min}} = \frac{1}{2\pi \times 11109.5 \times \sqrt{20\text{nF} \times 10\text{nF}}} \approx 1.01\text{ kHz}$$

- **Max LP Cutoff ($VR1 = 0\ \Omega$, $R_1 = 1\text{ k}\Omega$, $R_2 = 1.22\text{ k}\Omega$):**
  $$\sqrt{R_1 R_2} \approx 1,104.5\ \Omega$$
  $$f_{c,\text{max}} = \frac{1}{2\pi \times 1104.5 \times \sqrt{20\text{nF} \times 10\text{nF}}} \approx 10.18\text{ kHz}$$

**Key Design Detail:** $C_{\text{top}} (20\text{ nF}) = 2 \times C_{\text{gnd}} (10\text{ nF})$ forces $Q = 0.7071$ (Butterworth response) in a unity-gain Sallen-Key LP topology.

---

## 8. POTENTIOMETERS

- **Potentiometer Structure:** 3-terminal device (Track end 1, Wiper 2, Track end 3).
- **Rheostat Wiring:** Wiper is tied to one outer terminal. This converts it to a 2-terminal variable resistor and prevents open-circuit faults if wiper contact flickers.
- **Dual-Gang Control:** Mechanical shaft links section A and section B so both filter poles shift together, preserving response shape ($Q$).
- **Schematic Flags:**
  1. *Duplicate labels:* Both HP and LP dual pots are labeled "VR1A/VR1B". They are physically two separate dual potentiometers ($50\text{ k}\Omega$ for HP, $10\text{ k}\Omega$ for LP).
  2. *Taper requirement:* Must use Logarithmic (Audio) taper for linear frequency feel.

---

## 9. OP-AMP BUFFER / FOLLOWER (LP STAGE)

- **Configuration:** Voltage follower / Unity-gain buffer (Pin 6 tied to Pin 7).
- **Gain:** $A_v = 1\text{ V/V}$ ($0\text{ dB}$).
- **Input Impedance:** Very high ($\sim 300\text{ k}\Omega$ diff / $>100\text{ M}\Omega$ CM). Prevents loading on filter capacitors.
- **Output Impedance:** Very low ($<0.1\ \Omega$ closed-loop).
- **Purpose:** Isolates LP filter network from external output cables, power amplifiers, or loads.

---

## 10. NODE-BY-NODE ANALYSIS

| Node | Connections | $10\text{ Hz}$ Signal | $1\text{ kHz}$ Signal | $20\text{ kHz}$ Signal |
| :--- | :--- | :--- | :--- | :--- |
| **N1** | `Audio_Input`, $C_1$ | Full input | Full input | Full input |
| **N2** | $C_1, C_2, R_3$ | Attenuated | Unattenuated | Unattenuated |
| **N3** | $C_2, R_4$, HP Pin 5 | Strongly atten. | Full signal | Full signal |
| **N4** | HP Pin 6, $R_5, R_6, VR1B$ | Min voltage | $0.643 \times V_{\text{out}}$ | $0.643 \times V_{\text{out}}$ |
| **N5** | HP Pin 7, LP Input | Attenuated | $+3.84\text{ dB}$ passed | $+3.84\text{ dB}$ passed |
| **N6** | LP $VR1A, R_3, VR1B, C_1 \parallel C_2$ | Passes through | Passes through | Shunted to Output |
| **N7** | LP $VR1B, R_4, C_3, R_5$ | Passes through | Passes through | Shunted to Ground |
| **N8** | LP $R_5$, LP Pin 5 | Passes through | Passes through | Attenuated ($\approx 0\text{V}$) |
| **N9** | `Audio_Output`, LP Pin 6 & 7 | Passes through | Passes through | Attenuated ($-12\text{dB/oct}$) |

---

## 11. FREQUENCY RESPONSE

- **Very Low Frequencies ($< 10\text{ Hz}$):** Attenuated at $-12\text{ dB/octave}$ by HP stage.
- **HP Cutoff ($19\text{ Hz} - 189\text{ Hz}$):** $-3\text{ dB}$ relative to passband peak ($+0.84\text{ dB}$ absolute), $+90^\circ$ phase shift.
- **Passband Region ($f_{c,\text{HP}} \ll f \ll f_{c,\text{LP}}$):** Constant gain $A_v = 1.556$ ($+3.84\text{ dB}$), $0^\circ$ phase shift.
- **LP Cutoff ($1.0\text{ kHz} - 10.2\text{ kHz}$):** $-3\text{ dB}$ relative to passband peak ($+0.84\text{ dB}$ absolute), $-90^\circ$ phase shift.
- **High Frequencies ($> 20\text{ kHz}$):** Attenuated at $-12\text{ dB/octave}$ by LP stage. Phase lags toward $-180^\circ$.
- **Combined System:** Variable Bandpass Filter (Variable subsonic rumble cut $+$ Variable treble roll-off).

---

## 12. MATHEMATICAL VERIFICATION & SCHEMATIC FLAGS

### Summary Values:
- **HP Cutoff Range:** $19.1\text{ Hz}$ to $189.4\text{ Hz}$
- **LP Cutoff Range:** $1.01\text{ kHz}$ to $10.18\text{ kHz}$
- **HP Stage Gain:** $1.556\text{ V/V}$ ($+3.84\text{ dB}$), $Q \approx 0.707$
- **LP Stage Gain:** $1.000\text{ V/V}$ ($0.00\text{ dB}$), $Q \approx 0.707$

### Schematic Errors / Inconsistencies:
1. **IC Identifier:** Both op-amps are labeled `U1B NE5532`. Should be `U1A` (pins 1,2,3) and `U1B` (pins 5,6,7).
2. **Designator Duplications:** $VR1A, VR1B, R_3, R_4, R_5, C_1, C_2$ are reused in both stages despite different values.
3. **Parallel Capacitors:** $C_1$ ($10\text{ nF}$) and $C_2$ ($10\text{ nF}$) in LP stage are parallel; can be a single $20\text{ nF}$ capacitor.
4. **Supply Pins:** $V_{cc}/V_{ee}$ supply decoupling pins (8 and 4) are omitted from the diagram.

---

## 13. PHYSICAL INTUITION

- **Capacitor Mechanics:** High frequencies cause fast charge sloshing across plates, making capacitors act as AC short circuits. Low frequencies cause full charge build-up, blocking current.
- **Filtering Action:** HP series caps block DC/lows while passing highs. LP shunt caps short highs to ground/output while passing lows through series resistors.
- **Active Feedback:** Op-amp injects energy back into the RC node near cutoff to eliminate droop and sharpen the filter transition knee.

---

## 14. DESIGN THINKING (STEP-BY-STEP)

1. **Define Specs:** Select cutoff ranges (e.g., HP $20 - 200\text{ Hz}$, LP $1 - 10\text{ kHz}$) and Butterworth alignment ($Q = 0.707$).
2. **Choose HP Caps:** Set $C = 150\text{ nF}$.
3. **Calculate HP Resistors:**
   $$R_{\text{min}} = \frac{1}{2\pi \times 190\text{Hz} \times 150\text{nF}} = 5.6\text{ k}\Omega$$
   $$R_{\text{total}} = \frac{1}{2\pi \times 19\text{Hz} \times 150\text{nF}} = 55.6\text{ k}\Omega \implies R_{\text{pot}} = 50\text{ k}\Omega$$
4. **Set HP Gain:** 
   $$A_v = 3 - \frac{1}{Q} = 1.586\text{ V/V}$$
   Choose $R_5 = 1\text{ k}\Omega, R_6 = 1.8\text{ k}\Omega$ ($A_v = 1.556\text{ V/V}$).
5. **Choose LP Caps:** $C_{\text{gnd}} = 10\text{ nF}$, $C_{\text{top}} = 2 \times C_{\text{gnd}} = 20\text{ nF}$ (forces $Q = 0.707$ at unity gain).
6. **Calculate LP Resistors:** $R_{\text{min}} = 1\text{ k}\Omega$, $R_{\text{pot}} = 10\text{ k}\Omega$.
7. **Buffer:** Add unity-gain follower to eliminate downstream loading.

---

## 15. PRACTICAL ENGINEERING CONSIDERATIONS

- **Op-Amp Selection:** NE5532 is ultra-low noise ($5\text{ nV}/\sqrt{\text{Hz}}$) and high GBW ($10\text{ MHz}$).
- **Capacitors:** Use 5% film capacitors (Polypropylene or C0G/NP0 ceramic). Avoid high-K ceramics.
- **Power Supply:** Requires dual supply ($\pm 12\text{ V}$ to $\pm 15\text{ V}$) with $100\text{ nF}$ decoupling caps at pins 8 and 4.
- **Headphone Loading:** Output current limit is $\sim 38\text{ mA}$. Connecting $32\ \Omega$ headphones directly will cause severe current clipping. Requires a dedicated high-current output buffer (e.g., BUF634).

---

## 16. FINAL UNDERSTANDING CHECK (TEST QUESTIONS)

- **Q1:** Why is positive feedback brought to $VR1A$ in HP, but negative feedback to $C_1/C_2$ in LP?
- **Q2:** What happens to the HP frequency response if $R_5$ is changed from $1\text{ k}\Omega$ to $2\text{ k}\Omega$?
- **Q3:** Calculate HP $f_c$ if $C_1, C_2$ are changed to $100\text{ nF}$ with $VR1$ set to $25\text{ k}\Omega$.
- **Q4:** Calculate LP $f_c$ and $Q$ if $C_1 \parallel C_2$ is replaced with a single $10\text{ nF}$ cap ($VR1 = 0\ \Omega$).
- **Q5:** What happens to `Audio_Output` if $C_3$ in the LP stage shorts to ground?
- **Q6:** What happens to HP gain and cutoff if $R_6$ ($1.8\text{ k}\Omega$) becomes an open circuit?
- **Q7:** What happens to LP cutoff if the mechanical shaft breaks ($VR1A = 0\ \Omega$, $VR1B = 10\text{ k}\Omega$)?
- **Q8:** *Troubleshooting:* HP output latches to $-14\text{ V}$ rail. What are two probable causes?
- **Q9:** *Troubleshooting:* $50\text{ Hz}/60\text{ Hz}$ power hum at output regardless of pot settings. What is missing?
- **Q10:** *Design:* How would you extend maximum LP cutoff up to $20\text{ kHz}$?
- **Q11:** *Design:* How do you modify this circuit to run on a single $+12\text{ V DC}$ power supply?
