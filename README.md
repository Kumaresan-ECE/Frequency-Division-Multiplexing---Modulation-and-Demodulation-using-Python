# Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

__Aim__:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.


__Apparatus Required__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)


__Theory__:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

__Procedure__:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband

PROGRAM :
~~~
clc;
clear;

// Time parameters
t = linspace(0, 1, 1000);
fs = 1000;

// Frequencies
freqs = [4 8 12 16 20 24];

// Generate signals
signals = zeros(6, length(t));
for i = 1:6
    signals(i, :) = sin(2 * %pi * freqs(i) * t);
end

// Multiplexing (FDM signal)
fdm_signal = zeros(1, length(t));
for i = 1:6
    fdm_signal = fdm_signal + signals(i, :);
end

// Low-pass filter design (simple FIR using window method)
N = 50;
fc = 8;  // cutoff frequency
h = sinc(2 * fc/fs * (-(N/2):(N/2)));
h = h .* window('hm', N+1)';   // Hamming window

// Demultiplexing
demux_signals = zeros(6, length(t));

for i = 1:6
    mixed = fdm_signal .* sin(2 * %pi * freqs(i) * t);
    demux = conv(mixed, h, 'same');   // filtering
    demux_signals(i, :) = demux;
end

// Plot original signals
scf(1);
clf;
for i = 1:6
    subplot(3,2,i);
    plot(t, signals(i, :));
    xtitle("Original Signal f=" + string(freqs(i)));
end

// Plot FDM signal
scf(2);
clf;
plot(t, fdm_signal);
xtitle("FDM Signal");

// Plot demultiplexed signals
scf(3);
clf;
for i = 1:6
    subplot(3,2,i);
    plot(t, demux_signals(i, :));
    xtitle("Demultiplexed Signal f=" + string(freqs(i)));
end
~~~

TABULATION :

<img width="869" height="1280" alt="image" src="https://github.com/user-attachments/assets/686ab7c7-48b7-485f-8856-cf4cf75df029" />


OUTPUT :

<img width="1600" height="940" alt="image" src="https://github.com/user-attachments/assets/6928c674-1312-4359-a949-6c5f6fb8b6dd" />

<img width="1600" height="935" alt="image" src="https://github.com/user-attachments/assets/464da24b-5eb9-4691-8396-d1fae59b00e4" />

<img width="1600" height="936" alt="image" src="https://github.com/user-attachments/assets/c9814a76-e866-4617-9c61-eebc8c3bc8cc" />




RESULT :

<img width="787" height="1318" alt="image" src="https://github.com/user-attachments/assets/3b11bef2-4cda-4bc7-a193-f742885b9010" />

