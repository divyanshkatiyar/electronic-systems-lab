# 🔈 Two-Stage Class AB Push-Pull Audio Amplifier

## 1. Circuit Overview
This project implements a discrete, two-stage Class AB complementary push-pull audio power amplifier powered by a single-supply $+9\text{ V}$ DC rail[span_0](start_span)[span_0](end_span). The architecture combines a Class A common-emitter voltage amplifier stage ($Q_1$) with a diode-biased Class AB emitter-follower output stage ($Q_2$, $Q_3$) driving a $16\ \Omega$ speaker load[span_1](start_span)[span_1](end_span).

### 🎛 Functional Blocks
* **Input Coupling ($C_{\text{in}}$):** AC-couples the incoming audio signal while blocking DC offset from the input source[span_2](start_span)[span_2](end_span). Note: In practical implementation, ensure the positive terminal of the $10\ \mu\text{F}$ electrolytic capacitor faces the base of $Q_1$ if the input source is ground-referenced, as $V_{B1} \approx +0.65\text{ V}$.
* **Pre-Amplifier & Voltage Gain Stage ($Q_1$, $R_c$, $R_f$):** $Q_1$ (BC550C NPN) operates in a common-emitter configuration with collector-to-base shunt feedback ($R_f = 150\text{ k}\Omega$) to stabilize the DC operating point and provide voltage amplification[span_3](start_span)[span_3](end_span).
* **Thermal & Crossover Bias Network ($D_1$, $D_2$):** Two series silicon diodes (1N4007) establish a constant voltage drop ($\approx 1.3\text{ V}$ to $1.4\text{ V}$) between the bases of $Q_2$ and $Q_3$[span_4](start_span)[span_4](end_span). This pre-biases both output transistors into slight conduction, eliminating crossover distortion.
* **Class AB Push-Pull Output Stage ($Q_2$, $Q_3$):** A complementary symmetry pair consisting of $Q_2$ (BC337 NPN) and $Q_3$ (BC327 PNP) configured as emitter followers[span_5](start_span)[span_5](end_span). $Q_2$ sources current during the positive half-cycle, and $Q_3$ sinks current during the negative half-cycle.
* **Output Coupling ($C_{\text{out}}$):** A $220\ \mu\text{F}$ electrolytic capacitor blocks the DC quiescent output voltage ($\approx V_{CC}/2$) from flowing through the $16\ \Omega$ speaker voice coil[span_6](start_span)[span_6](end_span).

---

## 2. 🛠 Component Stack

| Designator | Component | Value / Part Number | Specifications / Notes |
| :--- | :--- | :--- | :--- |
| **$V_{CC}$** | DC Power Supply | $9\text{ V}$ Battery[span_7](start_span)[span_7](end_span) | Single-rail supply ($+9\text{ V}$ and GND)[span_8](start_span)[span_8](end_span) |
| **$Q_1$** | NPN BJT | BC550C[span_9](start_span)[span_9](end_span) | High-gain low-noise pre-amp ($h_{FE} \approx 420 - 800$) |
| **$Q_2$** | NPN Power BJT | BC337[span_10](start_span)[span_10](end_span) | Complementary high-current NPN ($I_{C(\max)} = 800\text{ mA}$) |
| **$Q_3$** | PNP Power BJT | BC327[span_11](start_span)[span_11](end_span) | Complementary high-current PNP ($I_{C(\max)} = 800\text{ mA}$) |
| **$D_1, D_2$** | Silicon Diodes | 1N4007 (x2)[span_12](start_span)[span_12](end_span) | Biasing network for Class AB crossover compensation[span_13](start_span)[span_13](end_span) |
| **$R_c$** | Resistor | $330\ \Omega$[span_14](start_span)[span_14](end_span) | $1/4\text{ W}$, $5\%$ tolerance (Collector load for $Q_1$) |
| **$R_f$** | Resistor | $150\text{ k}\Omega$[span_15](start_span)[span_15](end_span) | $1/4\text{ W}$, $5\%$ tolerance (Shunt feedback resistor) |
| **$C_{\text{in}}$** | Electrolytic Capacitor | $10\ \mu\text{F}$[span_16](start_span)[span_16](end_span) | $\ge 16\text{ V}$ rating (Input DC blocking) |
| **$C_{\text{out}}$** | Electrolytic Capacitor | $220\ \mu\text{F}$[span_17](start_span)[span_17](end_span) | $\ge 16\text{ V}$ rating (Output DC blocking) |
| **$R_L$** | Loudspeaker | $16\ \Omega$[span_18](start_span)[span_18](end_span) | Dynamic speaker load[span_19](start_span)[span_19](end_span) |

---

## 3. ⚙️ Mathematical Circuit Analysis

### Standard Component Assumptions
* Supply Voltage ($V_{CC}$): $9.0\text{ V}$[span_20](start_span)[span_20](end_span)
* Load Impedance ($R_L$): $16\ \Omega$[span_21](start_span)[span_21](end_span)
* Base-Emitter Forward Drop ($V_{BE}$): $0.65\text{ V}$ (for $Q_1, Q_2, Q_3$ at low quiescent currents)
* Diode Forward Drop ($V_{D1}, V_{D2}$): $0.65\text{ V}$ each (at $I_C \approx 10\text{ mA}$)
* $Q_1$ DC Current Gain ($\beta_1$ / $h_{FE1}$): $500$ (typical for BC550C)
* $Q_2, Q_3$ DC Current Gain ($\beta_{2,3}$ / $h_{FE2,3}$): $250$ (typical for BC337/BC327)
* Thermal Voltage ($V_T$ at $25^\circ\text{C}$): $26\text{ mV}$

---

### 3.1 DC Operating Point (Quiescent Analysis)

1. **Output Quiescent Voltage ($V_{\text{out,Q}}$):**
   For maximum symmetrical output swing, the emitter node of $Q_2$ and $Q_3$ should sit near half the supply voltage:
   $$V_{\text{out,Q}} \approx \frac{V_{CC}}{2} = \frac{9.0\text{ V}}{2} = 4.5\text{ V}$$

2. **Node Voltages at Bias Network:**
   * Base of $Q_2$:
     $$V_{B2} = V_{\text{out,Q}} + V_{BE2} = 4.5\text{ V} + 0.65\text{ V} = 5.15\text{ V}$$
   * Base of $Q_3$ (Collector of $Q_1$):
     $$V_{C1} = V_{B3} = V_{\text{out,Q}} - V_{BE3} = 4.5\text{ V} - 0.65\text{ V} = 3.85\text{ V}$$
   * Total Voltage Drop Across $D_1$ and $D_2$:
     $$V_{D1} + V_{D2} = V_{B2} - V_{B3} = 5.15\text{ V} - 3.85\text{ V} = 1.30\text{ V}$$
   * Base of $Q_1$:
     $$V_{B1} = V_{BE1} = 0.65\text{ V}$$

3. **Quiescent Currents ($I_{CQ1}$, $I_{BQ1}$, $I_{\text{bias}}$):**
   * Current through collector resistor $R_c$:
     $$I_{Rc} = \frac{V_{CC} - V_{B2}}{R_c} = \frac{9.0\text{ V} - 5.15\text{ V}}{330\ \Omega} = \frac{3.85\text{ V}}{330\ \Omega} \approx 11.67\text{ mA}$$
   * Feedback current through $R_f$:
     $$I_{Rf} = \frac{V_{B2} - V_{B1}}{R_f} = \frac{5.15\text{ V} - 0.65\text{ V}}{150\text{ k}\Omega} = \frac{4.50\text{ V}}{150,000\ \Omega} = 30\ \mu\text{A}$$
   * Base current of $Q_1$:
     $$I_{BQ1} = I_{Rf} = 30\ \mu\text{A}$$
   * Collector current of $Q_1$ (assuming $\beta_1 = 500$):
     $$I_{CQ1} = \beta_1 \cdot I_{BQ1} = 500 \cdot 30\ \mu\text{A} = 15.0\text{ mA}$$
   * *Self-Bias Equilibrium Check:* Because $I_{CQ1}$ ($\approx 11.64\text{ mA}$ in equilibrium) pulls $V_{B2}$ down until $I_{Rc} = I_{CQ1} + I_{Rf}$:
     $$I_{Rc} = \beta_1 \left(\frac{V_{B2} - V_{BE1}}{R_f}\right) + \frac{V_{B2} - V_{BE1}}{R_f} = (\beta_1 + 1)\left(\frac{V_{B2} - 0.65}{150,000}\right)$$
     $$\frac{9.0 - V_{B2}}{330} = 501 \left(\frac{V_{B2} - 0.65}{150,000}\right) \implies 9.0 - V_{B2} = 1.1022(V_{B2} - 0.65)$$
     $$2.1022 V_{B2} = 9.7164 \implies V_{B2} \approx 4.62\text{ V}$$
     *Actual Equilibrium Voltages:*
     * $V_{B2} \approx 4.62\text{ V}$
     * $V_{\text{out,Q}} = V_{B2} - 0.65\text{ V} \approx 3.97\text{ V}$
     * $V_{C1} = V_{B3} = V_{\text{out,Q}} - 0.65\text{ V} \approx 3.32\text{ V}$
     * $I_{CQ1} \approx \frac{9.0\text{ V} - 4.62\text{ V}}{330\ \Omega} \approx 13.27\text{ mA}$

---

### 3.2 Small-Signal AC Analysis

1. **Dynamic Emitter Resistance ($r_e'$) of $Q_1$:**
   $$r_{e1}' = \frac{V_T}{I_{CQ1}} = \frac{26\text{ mV}}{13.27\text{ mA}} \approx 1.96\ \Omega$$

2. **Input Impedance ($Z_{\text{in}}$):**
   * Base input resistance of $Q_1$:
     $$r_{\pi 1} = \beta_1 \cdot r_{e1}' = 500 \cdot 1.96\ \Omega = 980\ \Omega$$
   * Miller-reflected feedback resistance ($R_{f,\text{in}}$):
     $$R_{f,\text{in}} = \frac{R_f}{1 + |A_v|}$$
   * Total amplifier input impedance:
     $$Z_{\text{in}} = r_{\pi 1} \parallel R_{f,\text{in}}$$

3. **Output Stage Input Impedance ($Z_{\text{in,stage2}}$):**
   Looking into the bases of the Class AB push-pull pair driving $R_L = 16\ \Omega$:
   $$Z_{\text{in,stage2}} \approx \beta_{2,3} \cdot (R_L + r_{e2}') \approx 250 \cdot (16\ \Omega + 1\ \Omega) = 4.25\text{ k}\Omega$$

4. **Effective AC Collector Load of $Q_1$ ($R_{L1}$):**
   Neglecting the small dynamic resistance of forward-biased diodes $D_1, D_2$ ($r_d \approx 2\ \Omega$ each):
   $$R_{L1} = R_c \parallel R_f \parallel Z_{\text{in,stage2}} = 330\ \Omega \parallel 150\text{ k}\Omega \parallel 4.25\text{ k}\Omega \approx 305.6\ \Omega$$

5. **Voltage Gain ($A_v$):**
   * Stage 1 Voltage Gain ($A_{v1}$):
     $$A_{v1} = -\frac{R_{L1}}{r_{e1}'} = -\frac{305.6\ \Omega}{1.96\ \Omega} \approx -155.9\text{ V/V}$$
   * Stage 2 Voltage Gain ($A_{v2}$ - Emitter Follower):
     $$A_{v2} = \frac{R_L}{R_L + r_{e2}'} \approx \frac{16\ \Omega}{16\ \Omega + 1\ \Omega} \approx 0.94\text{ V/V}$$
   * Overall Open-Loop Voltage Gain ($A_{v,\text{total}}$):
     $$A_{v,\text{total}} = A_{v1} \cdot A_{v2} \approx -155.9 \cdot 0.94 \approx -146.5\text{ V/V} \quad (\approx 43.3\text{ dB})$$
   * *Note on Source Impedance:* If driven by a source with internal resistance $R_s$ (e.g., $600\ \Omega$ line-out), the closed-loop shunt-feedback voltage gain becomes:
     $$A_{v,\text{closed}} \approx -\frac{R_f}{R_s} = -\frac{150\text{ k}\Omega}{600\ \Omega} = -250 \text{ (limited by } A_{v,\text{total}} \text{ to } \approx -70\text{ V/V)}$$
   * Refined Input Impedance ($Z_{\text{in}}$ using $|A_{v1}| \approx 156$):
     $$R_{f,\text{in}} = \frac{150\text{ k}\Omega}{1 + 156} \approx 955\ \Omega$$
     $$Z_{\text{in}} = 980\ \Omega \parallel 955\ \Omega \approx 484\ \Omega$$

---

### 3.3 Frequency Response (Low-Frequency Cutoffs)

1. **Input High-Pass Cutoff Frequency ($f_{L,\text{in}}$):**
   Determined by $C_{\text{in}} = 10\ \mu\text{F}$ and $Z_{\text{in}} \approx 484\ \Omega$ (assuming $R_s \approx 0\ \Omega$):
   $$f_{L,\text{in}} = \frac{1}{2\pi \cdot Z_{\text{in}} \cdot C_{\text{in}}} = \frac{1}{2\pi \cdot 484\ \Omega \cdot 10 \times 10^{-6}\text{ F}} \approx 32.9\text{ Hz}$$

2. **Output High-Pass Cutoff Frequency ($f_{L,\text{out}}$):**
   Determined by $C_{\text{out}} = 220\ \mu\text{F}$ and $R_L = 16\ \Omega$:
   $$f_{L,\text{out}} = \frac{1}{2\pi \cdot R_L \cdot C_{\text{out}}} = \frac{1}{2\pi \cdot 16\ \Omega \cdot 220 \times 10^{-6}\text{ F}} \approx 45.2\text{ Hz}$$

3. **Overall $-3\text{ dB}$ Lower Cutoff Frequency ($f_L$):**
   $$f_L \approx \sqrt{f_{L,\text{in}}^2 + f_{L,\text{out}}^2} = \sqrt{32.9^2 + 45.2^2} \approx 55.9\text{ Hz}$$

---

### 3.4 Power, Current, and Efficiency Calculations

1. **Maximum Unclipped Output Voltage Swing:**
   * Positive peak swing is limited by $V_{CC} - V_{\text{out,Q}} - V_{BE2} - V_{CE2(\text{sat})}$:
     $$V_{p(+)} \approx 9.0\text{ V} - 3.97\text{ V} - 0.65\text{ V} - 0.2\text{ V} = 4.18\text{ V}$$
   * Negative peak swing is limited by $V_{\text{out,Q}} - V_{CE3(\text{sat})}$:
     $$V_{p(-)} \approx 3.97\text{ V} - 0.2\text{ V} = 3.77\text{ V}$$
   * Symmetrical Maximum Peak Output Voltage ($V_{p,\max}$):
     $$V_{p,\max} = \min(V_{p(+)}, V_{p(-)}) \approx 3.77\text{ V}$$
   * Maximum RMS Output Voltage ($V_{\text{rms},\max}$):
     $$V_{\text{rms},\max} = \frac{V_{p,\max}}{\sqrt{2}} = \frac{3.77\text{ V}}{1.414} \approx 2.67\text{ V}_{\text{rms}}$$

2. **Output Power ($P_{\text{out}}$):**
   * Continuous Sine-Wave RMS Output Power:
     $$P_{\text{out(rms)}} = \frac{V_{\text{rms},\max}^2}{R_L} = \frac{(2.67\text{ V})^2}{16\ \Omega} = \frac{7.11\text{ W}}{16} \approx 0.444\text{ W} \quad (444\text{ mW})$$
   * Peak Instantaneous Output Power:
     $$P_{\text{out(peak)}} = 2 \cdot P_{\text{out(rms)}} \approx 0.888\text{ W}$$

3. **Peak Load & Transistor Currents:**
   * Peak Output Current ($I_{p,\max}$):
     $$I_{p,\max} = \frac{V_{p,\max}}{R_L} = \frac{3.77\text{ V}}{16\ \Omega} \approx 235.6\text{ mA}$$
     *(Well within the $800\text{ mA}$ continuous collector current limit of the BC337 and BC327).*
   * Peak Base Drive Required for $Q_2/Q_3$:
     $$I_{B2,3(\text{peak})} = \frac{I_{p,\max}}{\beta_{2,3}} = \frac{235.6\text{ mA}}{250} \approx 0.94\text{ mA}$$
     *(Easily supplied by the $13.27\text{ mA}$ standing current in $R_c$ and $Q_1$).*

4. **Power Dissipation & Efficiency ($\eta$):**
   * Average DC Current Drawn by Output Stage at Max Sine Output:
     $$I_{\text{DC,stage2}} = \frac{I_{p,\max}}{\pi} = \frac{235.6\text{ mA}}{3.1416} \approx 75.0\text{ mA}$$
   * Total DC Supply Current at Max Output (including $Q_1$ bias current):
     $$I_{\text{DC,total}} = I_{\text{DC,stage2}} + I_{CQ1} = 75.0\text{ mA} + 13.27\text{ mA} \approx 88.3\text{ mA}$$
   * Total DC Power Input ($P_{\text{DC}}$):
     $$P_{\text{DC}} = V_{CC} \cdot I_{\text{DC,total}} = 9.0\text{ V} \cdot 88.3\text{ mA} \approx 0.795\text{ W}$$
   * Overall Amplifier Efficiency ($\eta$):
     $$\eta = \frac{P_{\text{out(rms)}}}{P_{\text{DC}}} \times 100\% = \frac{0.444\text{ W}}{0.795\text{ W}} \times 100\% \approx 55.8\%$$
   * Worst-Case Power Dissipation per Output Transistor ($P_{D(Q2,Q3)}$):
     $$P_{D(\max)} = \frac{V_{CC}^2}{2\pi^2 R_L} = \frac{9.0^2}{2 \cdot \pi^2 \cdot 16} = \frac{81}{315.8} \approx 0.256\text{ W} \quad (256\text{ mW})$$
     *(Safe for TO-92 packages rated at $625\text{ mW}$ max at $25^\circ\text{C}$).*

---

## 4. ⚡️ Summary of Key Performance Metrics

| Parameter | Calculated Value | Unit |
| :--- | :--- | :--- |
| **Supply Voltage ($V_{CC}$)** | $9.0$[span_22](start_span)[span_22](end_span) | $\text{V}$ |
| **Load Impedance ($R_L$)** | $16$[span_23](start_span)[span_23](end_span) | $\Omega$ |
| **Quiescent Output Voltage ($V_{\text{out,Q}}$)** | $3.97$ | $\text{V}$ |
| **Quiescent Pre-Amp Current ($I_{CQ1}$)** | $13.27$ | $\text{mA}$ |
| **Input Impedance ($Z_{\text{in}}$)** | $484$ | $\Omega$ |
| **Open-Loop Voltage Gain ($A_v$)** | $\approx 146.5$ ($43.3\text{ dB}$) | $\text{V/V}$ |
| **Lower $-3\text{ dB}$ Cutoff Frequency ($f_L$)** | $55.9$ | $\text{Hz}$ |
| **Max Unclipped Output Swing ($V_{p,\max}$)** | $3.77$ ($2.67\text{ V}_{\text{rms}}$) | $\text{V}$ |
| **Max Continuous Output Power ($P_{\text{out}}$)** | $444$ | $\text{mW}$ |
| **Peak Load Current ($I_{p,\max}$)** | $235.6$ | $\text{mA}$ |
| **System Efficiency at Max Output ($\eta$)** | $55.8$ | $\%$ |
