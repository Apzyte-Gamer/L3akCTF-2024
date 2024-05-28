# Communication Gateway (498 points/9 solves)

Something is in-front of you, and you don't even know it.

**Author:** 0x157

**Challenge File:** [file10.wav](./file10.wav)

## Backstory

Initially, no one could solve this challenge. However, a hint was provided that clarified the approach, leading to multiple solves.

**Hint:** "I like modems and naming my files accordingly. The name might have been a baud idea though..."

## Analyzing the Audio

First of all, we can open the audio in Audacity and analyze the waveform. We can zoom in it and see that there are 2 waves going:

![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/90b7996e-fe5d-4511-a477-424bb33efebd)

I also used the application `baudline` to see the waterfall display of the audio.

![Waterfall Display](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/61c9a709-9165-4269-96c2-a94f205cf95a)

## Understanding the Waves

To understand both of the waves, we can plot the spectrum of the audio (using audacity) and see that the frequency of one wave is 1415 Hz and frequency of the other one is 1583 Hz.

![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/89acffe9-838a-46f6-90e4-8784e05bbda2)
![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/f79c9cfb-2ee2-43bb-a69f-3f9fbc85fa51)

We can see that the audio has 2 peaks.

### Frequency-Shift Keying (FSK)

The waveform displayed characteristics of Frequency-Shift Keying (FSK), a modulation scheme where digital data is represented by changes in frequency.

![FSK](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/42b6e880-98b7-4719-ab30-7b0dc3a2f9c5)

We can clearly see that this signal is a modulated wave.

## Extracting the Data

### Filtering

1. **Low-Pass Filter:** Isolate the lower peak. (1415 Hz)
2. **High-Pass Filter:** Isolate the upper peak. (1583 Hz)

This resulted in two separate channels, one for binary `1`s and one for binary `0`s.

![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/0e2e75b8-dd34-4524-9b15-e780d1e47382)

### Decoding the Data

Key observations:
- Each byte is sent with the Least Significant Bit (LSB) first.
- There is an initial garbage `1`.
- Each byte is separated by a `10` delimiter.

### Assembling the Flag

Now, we can just manually get the binary values and convert from binary to plaintext and we get the flag!

**Flag:** `L3AK{s1gn4ls_0f_h0p3}`

## Another Solution

Another solution involves using `minimodem` to decode the audio. By running the command `minimodem -f file10.wav 10_N`, the data can be decoded. This solution relies on setting the baudmode, and a guess based on the filename proved successful, using a Bell-like with `10_N` baudmode.

![image](https://github.com/Apzyte-Gamer/L3akCTF-2024/assets/71684682/5239363a-62e1-4ed7-a5e9-1bf26ad7cdf0)



Overall, this was a very fun challenge (being my 1st hardware solve ever) and I learned A LOT from this challenge!
