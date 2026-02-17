# Boltzmann Fit – M-Wave Recruitment Analysis

GUI-based Python tool for analyzing EMG recruitment curves.  
Loads sweep data from CSV, extracts M-wave and H-reflex amplitudes, and fits a 3-parameter Boltzmann (logistic) model to the M-wave.

---

## Features

- Loads headerless numeric CSV files
- Extracts peak-to-peak amplitudes in user-defined time windows
- Manual stimulation intensity schedule
- 3-parameter logistic (Boltzmann) fit
- Interactive Tkinter GUI
- Automatic plotting of:
  - M-wave amplitudes
  - H-reflex amplitudes
  - Fitted recruitment curve
- Returns:
  - Maximum response (max_out)
  - Half-activation intensity (x50)
  - Slope factor (k)

---

## Installation

Requires Python 3.9+.

Install dependencies:

```bash
pip install numpy pandas scipy matplotlib
```

Tkinter is included with standard Python installations on Windows.

---

## Expected CSV Format

The script expects a **headerless numeric CSV**:

| Column | Description |
|--------|-------------|
| 0      | Time (seconds) |
| 1..N   | Individual sweep signals |

Example:

```
0.0000, ...
0.0001, ...
0.0002, ...
```

Important:

- Time must be in seconds  
- No header row  
- At least two sweep columns required  

---

## Usage

Run:

```bash
python Boltzmann.py
```

The GUI will open.

Steps:

1. Click **Browse** and select your CSV file  
2. Adjust stimulation schedule if needed  
3. Adjust time windows (ms)  
4. Click **Run fit**

---

## Stimulation Schedule

Intensity values are generated manually:

```
X = start + step × level
```

Each intensity level repeats for:

```
sweeps_per_step
```

Example:

- start = 0.2 mV  
- step = 0.3 mV  
- sweeps_per_step = 3  

Sweeps 0–2 share intensity 0.2 mV  
Sweeps 3–5 share 0.5 mV  
etc.

---

## Time Windows (ms)

All windows are defined relative to stimulation time.

| Window | Purpose |
|--------|---------|
| Stim   | Stimulation artifact |
| M      | M-wave window |
| H      | H-reflex window |
| RMS    | Baseline window |

Adjust these to match your acquisition timing.

---

## Model

The M-wave recruitment curve is fitted using:

```
max_out / (1 + exp(-(x - x50) / k))
```

Where:

- **max_out** = maximum response  
- **x50** = intensity at half-max  
- **k** = slope parameter  

Only the M-wave is fitted.  
H-reflex amplitudes are displayed but not modeled.

---

## Amplitude Scaling

The script multiplies amplitudes by 1000:

```python
scale_to_mV = 1000.0
```

If your CSV is already in mV, change this to:

```python
scale_to_mV = 1.0
```


```

### Fit fails

Check:

- Time column validity  
- Window placement  
- sweeps_per_step value  
- CSV format  
- Enough sweeps for fitting  


---

## Notes

- Uses peak-to-peak amplitude  
- No baseline subtraction  
- No normalization applied  
- Deterministic small X jitter can be enabled to avoid duplicate intensity issues  

---

## License

Free for research and academic use.
