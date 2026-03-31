# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.
# Tools required
Google Collab
# Program
# Impluse sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import resample

# Parameters
fs = 100
T = 1
f = 5

# Time
t = np.arange(0, T, 1/fs)

# Signal
signal = np.sin(2 * np.pi * f * t)

# Plot Continuous Signal
plt.figure()
plt.plot(t, signal)
plt.title("Continuous Signal")
plt.grid()
plt.show()

# Sampling
t_sampled = np.arange(0, T, 1/fs)
signal_sampled = np.sin(2 * np.pi * f * t_sampled)

# Plot Sampled Signal
plt.figure()
plt.stem(t_sampled, signal_sampled)
plt.title("Impulse Sampling")
plt.grid()
plt.show()

# Reconstruction
reconstructed = resample(signal_sampled, len(t))

plt.figure()
plt.plot(t, reconstructed)
plt.title("Reconstructed Signal")
plt.grid()
plt.show()

```
## Natural sampling 
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Parameters
fs = 1000
T = 1
fm = 5

t = np.arange(0, T, 1/fs)

# Message Signal
message = np.sin(2 * np.pi * fm * t)

# Pulse Train
pulse_rate = 50
pulse_train = np.zeros_like(t)

pulse_width = int(fs / pulse_rate / 2)

for i in range(0, len(t), int(fs / pulse_rate)):
    pulse_train[i:i+pulse_width] = 1

# Natural Sampling
natural_signal = message * pulse_train

# Low-pass Filter
def lowpass(signal, cutoff, fs):
    nyq = 0.5 * fs
    normal = cutoff / nyq
    b, a = butter(5, normal, btype='low')
    return lfilter(b, a, signal)

reconstructed = lowpass(natural_signal, 10, fs)

# Plots
plt.figure(figsize=(10,8))

plt.subplot(4,1,1)
plt.plot(t, message)
plt.title("Original Signal")

plt.subplot(4,1,2)
plt.plot(t, pulse_train)
plt.title("Pulse Train")

plt.subplot(4,1,3)
plt.plot(t, natural_signal)
plt.title("Natural Sampling")

plt.subplot(4,1,4)
plt.plot(t, reconstructed)
plt.title("Reconstructed Signal")

plt.tight_layout()
plt.show()
```
## Flat-Top Sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Parameters
fs = 1000
T = 1
fm = 5

t = np.arange(0, T, 1/fs)

# Message Signal
message = np.sin(2 * np.pi * fm * t)

# Sampling
pulse_rate = 50
indices = np.arange(0, len(t), int(fs / pulse_rate))

flat_signal = np.zeros_like(t)

pulse_width = int(fs / (2 * pulse_rate))

# Flat-top Sampling
for i in indices:
    value = message[i]
    flat_signal[i:i+pulse_width] = value

# Low-pass Filter
def lowpass(signal, cutoff, fs):
    nyq = 0.5 * fs
    normal = cutoff / nyq
    b, a = butter(5, normal, btype='low')
    return lfilter(b, a, signal)

reconstructed = lowpass(flat_signal, 10, fs)

# Plots
plt.figure(figsize=(10,8))

plt.subplot(4,1,1)
plt.plot(t, message)
plt.title("Original Signal")

plt.subplot(4,1,2)
plt.stem(t[indices], np.ones_like(indices))
plt.title("Sampling Points")

plt.subplot(4,1,3)
plt.plot(t, flat_signal)
plt.title("Flat-Top Sampling")

plt.subplot(4,1,4)
plt.plot(t, reconstructed)
plt.title("Reconstructed Signal")

plt.tight_layout()
plt.show()


```

# Output Waveform
## Ideal sampling
<img width="568" height="435" alt="image" src="https://github.com/user-attachments/assets/b4ccd017-ac37-4965-8787-5d5d8edc2908" />
<img width="568" height="435" alt="image" src="https://github.com/user-attachments/assets/c264623f-24e8-4402-9cdf-c43ffdcc6b8c" />
<img width="568" height="435" alt="image" src="https://github.com/user-attachments/assets/e03db7a1-ac56-4285-8830-5106449cd89f" />
## Natural sampling
<img width="1238" height="985" alt="image" src="https://github.com/user-attachments/assets/c33c33c0-bd86-49e6-8a81-e5f5e4b9ca32" />
## Flat top sampling
<img width="1232" height="981" alt="image" src="https://github.com/user-attachments/assets/2cd7f03f-add4-4ec7-a0ff-c94c835d3872" />



# Results
Thus, the construction and reconstruction of Ideal, Natural, and Flat-top sampling were successfully implemented using Python, and the corresponding waveforms were obtained.
