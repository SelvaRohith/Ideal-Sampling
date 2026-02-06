# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.
# Tools required
Google colab
# Program
### IDEAL SAMPLING
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import resample


fs = 100          # Sampling frequency (Hz)
f = 5             # Signal frequency (Hz)

t = np.arange(0, 1, 1/fs)
signal = np.sin(2 * np.pi * f * t)

plt.figure(figsize=(10, 4))
plt.plot(t, signal, label='Continuous Signal')
plt.title('Continuous Signal (fs = 1000 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()
plt.show()

t_sampled = np.arange(0, 1, 1/fs)
signal_sampled = np.sin(2 * np.pi * f * t_sampled)

plt.figure(figsize=(10, 4))
plt.stem(
    t_sampled,
    signal_sampled,
    linefmt='r-',
    markerfmt='ro',
    basefmt='r-',
    label='Sampled Signal (fs = 1000 Hz)'
)
plt.title('Sampling of Continuous Signal (fs = 1000 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()
plt.show()

reconstructed_signal = resample(signal_sampled, len(t))

plt.figure(figsize=(10, 4))
plt.plot(t, reconstructed_signal, 'r--', label='Reconstructed Signal (fs = 100 Hz)')
plt.title('Reconstruction of Sampled Signal (fs = 1000 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()
plt.show()


```
### NATURAL SAMPLING
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

fs = 1000          # Sampling frequency (Hz)
T = 1              # Duration (seconds)
t = np.arange(0, T, 1/fs)

fm = 5             # Message signal frequency (Hz)

# -------------------------------------------------
# Message Signal
# -------------------------------------------------
message_signal = np.sin(2 * np.pi * fm * t)

# -------------------------------------------------
# Ideal Pulse Train (Sampling Instants)
# -------------------------------------------------
pulse_rate = 50    # Pulses per second
pulse_train_indices = np.arange(0, len(t), int(fs / pulse_rate))

pulse_train = np.zeros_like(t)
pulse_train[pulse_train_indices] = 1


# -------------------------------------------------
# Low-pass Filter (Reconstruction)
# -------------------------------------------------
def lowpass_filter(signal, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return filtfilt(b, a, signal)

cutoff_freq = 2 * fm   # Slightly above message frequency
reconstructed_signal = lowpass_filter(flat_top_signal, cutoff_freq, fs)

# -------------------------------------------------
# Plotting
# -------------------------------------------------
plt.figure(figsize=(14, 10))

plt.subplot(4, 1, 1)
plt.plot(t, message_signal, label='Original Message Signal')
plt.title('Original Message Signal')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()

plt.subplot(4, 1, 2)
plt.stem(
    t[pulse_train_indices],
    pulse_train[pulse_train_indices],
    basefmt=" ",
    label='Ideal Sampling Instances'
)
plt.title('Ideal Sampling Instances')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()

plt.subplot(4, 1, 3)
plt.plot(
    t,
    reconstructed_signal,
    color='green',
    label=f'Reconstructed Signal (LPF cutoff = {cutoff_freq} Hz)'
)
plt.title('Reconstructed Signal')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```
### FLAT TOP SAMPLING
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt
t = np.linspace(0, 1, 1000)
f = 5
x = np.sin(2 * np.pi * f * t)
p = ((t * 50) % 1 < 0.5).astype(float)  
xn = x * p                              
fs = 1000           
cutoff = 10          
order = 4
b, a = butter(order, cutoff / (fs / 2), btype='low')
x_rec = filtfilt(b, a, xn)
plt.figure()
plt.plot(t, x)
plt.title("Message Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid()
plt.show()
plt.figure()
plt.plot(t, p)
plt.title("Pulse Train")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid()
plt.show()
plt.figure()
plt.plot(t, xn)
plt.title("Natural Sampling")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid()
plt.show()
plt.figure()
plt.plot(t, x_rec, label="Reconstructed Signal")
plt.plot(t, x, 'r--', label="Original Signal")
plt.title("Reconstructed Signal using Butterworth Filter")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.legend()
plt.grid()
plt.show()

```
# Output Waveform
1.IDEAL SAMPLING
<img width="1165" height="500" alt="Screenshot 2026-01-31 141642" src="https://github.com/user-attachments/assets/47dc9b9c-b3de-4da4-805a-4ec9d961fe67" />
<img width="1150" height="534" alt="Screenshot 2026-01-31 141714" src="https://github.com/user-attachments/assets/99e6e846-1ffb-49e7-a2c0-31f1477b55e8" />
<img width="1205" height="513" alt="Screenshot 2026-01-31 141735" src="https://github.com/user-attachments/assets/2ae6d6fc-7d96-4b62-a5b6-58d39ebc8589" />

2. NATURAL SAMPLINNG
<img width="692" height="196" alt="image" src="https://github.com/user-attachments/assets/d40d491b-d800-4d8d-aca5-7e077ee6fd1f" />
<img width="692" height="185" alt="image" src="https://github.com/user-attachments/assets/dc95b081-8454-4615-9635-32e7ea99b7b1" />
<img width="692" height="226" alt="image" src="https://github.com/user-attachments/assets/f82a6a13-e355-4f78-a4b0-ae8fa54ff81e" />

3. FLAT TOP SAMPLING
<img width="692" height="479" alt="image" src="https://github.com/user-attachments/assets/f44a0f2e-4394-4898-8d98-ea11cba3023c" />
<img width="691" height="495" alt="image" src="https://github.com/user-attachments/assets/ce4c55b3-8cdf-48c1-a263-1efee323767c" />


# Results
```
Thus, the construction and reconstruction of Ideal, Natural, and Flat-top sampling were successfully implemented using Python, and the corresponding waveforms were obtained.
```
