# Communication Gateway (498 points/9 solves)

**Author:** 0x157

**Challenge File:** (file10.wav)[./file10.wav]

## Backstory

Initially, no one could solve this challenge. However, a hint was provided that clarified the approach, leading to multiple solves.

**Hint:** "I like modems and naming my files accordingly. The name might have been a baud idea though..."

## Analyzing the Audio

Upon opening the file in Audacity and viewing its spectrogram, it became clear that there were two lines of data, one higher and one lower.

![Spectrogram](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/f1fa9f92-62cb-4eac-9cd4-592080c18826)

## Using the Hint

The hint suggested that the signals were modem signals, and the term "baud" refers to the bitrate. This indicated that the challenge involved bitrates and modems.

## Understanding the Waves

Using the application `baudline`, the waterfall display of the audio revealed two peaks.

![Waterfall Display](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/61c9a709-9165-4269-96c2-a94f205cf95a)

### Waveform Visualization

A Python script was used to plot the waveform.

```py
import wave
import numpy as np
import matplotlib.pyplot as plt

file_path = 'file10.wav'
with wave.open(file_path, 'r') as wav_file:
    n_channels = wav_file.getnchannels()
    sampwidth = wav_file.getsampwidth()
    framerate = wav_file.getframerate()
    n_frames = wav_file.getnframes()
    audio_data = wav_file.readframes(n_frames)

audio_signal = np.frombuffer(audio_data, dtype=np.int16)

time_axis = np.linspace(0, n_frames / framerate, num=n_frames)


zoom_factor = 1000
plt.figure(figsize=(12, 6))
plt.plot(time_axis[:zoom_factor], audio_signal[:zoom_factor], linewidth=0.5)
plt.title('Waveform of file10.wav (Zoomed In)')
plt.xlabel('Time (seconds)')
plt.ylabel('Amplitude')
plt.show()
```

![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/10d92ceb-8e44-4c7d-91f3-ce06bfc81434)

### Frequency-Shift Keying (FSK)

The waveform displayed characteristics of Frequency-Shift Keying (FSK), a modulation scheme where digital data is represented by changes in frequency.

![FSK](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/42b6e880-98b7-4719-ab30-7b0dc3a2f9c5)

## Extracting the Data

### Filtering with GNU Radio

1. **Low-Pass Filter:** Isolate the lower peak.
2. **High-Pass Filter:** Isolate the upper peak.

This resulted in two separate channels, one for binary `1`s and one for binary `0`s.

### Decoding the Data

Key observations:
- Each byte is sent with the Least Significant Bit (LSB) first.
- There is an initial garbage `1`.
- Each byte is separated by a `10` delimiter.

### Assembling the Flag

By manually transferring the bits, the flag was retrieved.

**Flag:** `L3AK{s1gn4ls_0f_h0p3}`

## Another Solution

Another solution involves using `minimodem` to decode the audio. By running the command `minimodem -f file10.wav 10_N`, the data can be decoded. This solution relies on setting the baudmode, and a guess based on the filename proved successful, using a Bell-like with `10_N` baudmode.

![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/5239363a-62e1-4ed7-a5e9-1bf26ad7cdf0)

