# Keithley SMU Control with Python

Python scripts for controlling a **Keithley Source Measure Unit (SMU)** using **SCPI commands through VISA**.  
This project enables automated measurements such as voltage sweeps, current sweeps, and IV curve characterization.

The scripts communicate with the SMU through **USB, GPIB, or LAN interfaces** using the `pyvisa` library.

---

# Features

- Control Keithley SMU from Python
- Perform automated voltage sweeps
- Perform current measurements
- Generate IV curves
- Automate laboratory measurements
- Plot measurement results using matplotlib

---

# Supported Instruments

Tested with:

- Keithley 2400 Series SMU

Other Keithley SMUs that support **SCPI commands** should work with minimal modifications.

---

# Hardware Setup

Example setup:

```
Computer
   │
   │ USB / GPIB / LAN
   ▼
Keithley SMU
   │
   │ DUT (Device Under Test)
   ▼
Measurement
```

---

# Software Requirements

Install Python dependencies:

```bash
pip install pyvisa
pip install pyvisa-py
pip install numpy
pip install matplotlib
```

If using NI-VISA drivers:

```
https://www.ni.com/en/support/downloads/drivers/download.ni-visa.html
```

---

# Project Structure

```
i'll add them later

---

# Connecting to the SMU

You can list available VISA instruments using:

```python
import pyvisa

rm = pyvisa.ResourceManager()
print(rm.list_resources())
```

Example output:

```
('USB0::0x05E6::0x2400::1234567::INSTR')
```

Use that address to connect.

---

# Basic Example

```python
import pyvisa

rm = pyvisa.ResourceManager()
smu = rm.open_resource('USB0::0x05E6::0x2400::1234567::INSTR')

# set voltage
smu.write(":SOUR:VOLT 1")

# measure current
current = smu.query(":MEAS:CURR?")

print("Measured current:", current)
```

---

# Voltage Sweep Example

Example script for generating an IV curve:

```python
import numpy as np
import matplotlib.pyplot as plt
import pyvisa

rm = pyvisa.ResourceManager()
smu = rm.open_resource('USB0::0x05E6::0x2400::1234567::INSTR')

voltages = np.linspace(0,5,50)
currents = []

for v in voltages:
    smu.write(f":SOUR:VOLT {v}")
    i = float(smu.query(":MEAS:CURR?"))
    currents.append(i)

plt.plot(voltages,currents)
plt.xlabel("Voltage (V)")
plt.ylabel("Current (A)")
plt.title("IV Curve")
plt.show()
```

---

# Example Output

Example IV curve generated using the scripts:

```
plots/example_iv_curve.png
```

---

# Applications

This project can be used for:

- Semiconductor characterization
- Sensor testing
- Electronic component analysis
- Automated laboratory measurements
- Research and development experiments

---

# Notes

- Ensure the correct **VISA address** for your SMU.
- Some instruments require **NI-VISA drivers**.
- Always verify safe voltage and current limits before running automated sweeps.

---

# Author

Oussama Nordine  
Electrical Engineering Student  
Embedded Systems, Instrumentation, and IoT
