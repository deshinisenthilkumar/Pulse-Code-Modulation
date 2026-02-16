# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
1.Personal Computer
2.Colab
# Program
1.PULSE CODE MODULATION
```
import numpy as np, matplotlib.pyplot as plt

fs,f,T,L=5000,50,0.1,16
t=np.linspace(0,T,int(fs*T),False)
msg=np.sin(2*np.pi*f*t)
clk=np.sign(np.sin(2*np.pi*200*t))
q=np.round(msg/((msg.max()-msg.min())/L))*((msg.max()-msg.min())/L)

plt.figure(figsize=(10,8))
titles=["Analog Message","Clock Signal","PCM Signal","PCM Demodulation"]
signals=[msg,clk,q,q]
styles=['-','-','steps','--']

for i,(s,st,ti) in enumerate(zip(signals,styles,titles),1):
    plt.subplot(4,1,i)
    plt.step(t,s) if st=='steps' else plt.plot(t,s,st)
    plt.title(ti); plt.grid()

plt.tight_layout(); plt.show()

```
2.DELTA MODULATION

```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt
# Parameters
fs, f, T, delta = 10000, 10, 1, 0.1
t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*f*t)
# Delta Modulation
dm = [0]
bits = []
for s in msg:
    step = delta if s > dm[-1] else -delta
    bits.append(1 if step > 0 else 0)
    dm.append(dm[-1] + step)
demod = np.cumsum([0] + [delta if b else -delta for b in bits])
b, a = butter(4, 20/(0.5*fs), 'low')
filt = filtfilt(b, a, demod)
plt.figure(figsize=(10,6))
plt.subplot(3,1,1)
plt.plot(t, msg); 
plt.title("Original Signal"); 
plt.grid()
plt.subplot(3,1,2)
plt.step(t, dm[:-1], where='mid'); 
plt.title("Delta Modulated Signal"); 
plt.grid()
plt.subplot(3,1,3)
plt.plot(t, filt[:-1], 'r:'); 
plt.title("Demodulated Signal"); 
plt.grid()
plt.tight_layout()
plt.show()
```
# Output Waveform
1.PULSE CODE MODULATION

<img width="779" height="621" alt="image" src="https://github.com/user-attachments/assets/1d629ea2-1de9-4ea8-b7f2-4fc45bdb9a3e" />

2.DELTA MODULATION

<img width="782" height="465" alt="image" src="https://github.com/user-attachments/assets/bc6d6046-a2ce-476b-8564-9869b342f4a7" />

# Result
The analog signal was successfully encoded and reconstructed using PCM and DM techniques in Python, verifying their working principles.

