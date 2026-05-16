# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Software required
Google Collab
# Program
# Pulse Code Modulation
```sci
import numpy as np
import matplotlib.pyplot as plt

# Parameters
fs, f, d, q = 5000, 50, 0.1, 16

t = np.linspace(0, d, int(fs*d), endpoint=False)
msg = np.sin(2*np.pi*f*t)
clk = np.sign(np.sin(2*np.pi*200*t))

step = (msg.max()-msg.min())/q
qsig = np.round(msg/step)*step
pcm = ((qsig-qsig.min())/step).astype(int)

titles = ["Message Signal (Analog)",
          "Clock Signal (Increased Frequency)",
          "PCM Modulated Signal (Quantized)",
          "PCM Demodulation Signal"]

signals = [msg, clk, qsig, qsig]
colors = ['b', 'g', 'r', 'purple']

plt.figure(figsize=(12,10))

for i in range(4):
    plt.subplot(4,1,i+1)
    if i == 2:
        plt.step(t, signals[i], color=colors[i])
    else:
        plt.plot(t, signals[i], color=colors[i],
                 linestyle='--' if i==3 else '-')
    plt.title(titles[i])
    plt.xlabel("Time [s]")
    plt.ylabel("Amplitude")
    plt.grid()

plt.tight_layout()
plt.show()
```
# Delta Modulation
```sci
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

# Parameters
fs, f, T, d = 10000, 10, 1, 0.1
t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*f*t)

# Delta Modulation
enc, dm, p = [], [0], 0
for s in msg:
    b = s > p
    enc.append(b)
    dm.append(p + d if b else p - d)
    p = dm[-1]

# Demodulation
demod = [0]
for b in enc:
    demod.append(demod[-1] + d if b else demod[-1] - d)

# Low-pass filter
b, a = butter(4, 20/(0.5*fs), 'low')
filt = filtfilt(b, a, demod)

# Plot
plt.figure(figsize=(12,6))

plots = [
    (msg, 'Original Signal', '-'),
    (dm[:-1], 'Delta Modulated Signal', 'step'),
    (filt[:-1], 'Demodulated & Filtered Signal', ':')
]

for i, (s, title, style) in enumerate(plots, 1):
    plt.subplot(3,1,i)
    if style == 'step':
        plt.step(t, s, where='mid')
    else:
        plt.plot(t, s, style)
    plt.title(title)
    plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform

# Pulse-code-Modulation
<img width="1189" height="990" alt="image" src="https://github.com/user-attachments/assets/4ee08592-8010-495c-89f6-c1168bb0f47b" />

# Delta-Modulation
<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/771e8ce7-d0e2-4f35-b798-aa5f543a1e74" />


# Results
The analog signal was successfully encoded and reconstructed using PCM and DM techniques in Python, verifying their working principles..
