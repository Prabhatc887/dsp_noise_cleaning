# Cleaning a Noisy Chord with FFT Filtering

Can a filter remove a 50 Hz hum and reduce background noise without removing notes from a chord? This Jupyter notebook explores that question using a synthetic A-major chord. Because the original signal is known, you can listen to the result and measure how closely the filtered waveform matches the clean one.

## The experiment

1. Generate a two-second chord from three cosine waves: A4 (440 Hz), C♯5 (554.37 Hz), and E5 (659.25 Hz). Sample it at 11,025 Hz.
2. Add a 50 Hz tone to simulate mains hum and Gaussian white noise to simulate broadband interference.
3. Plot the waveform and its FFT magnitude spectrum to identify the hum and the three notes.
4. Filter the signal in the frequency domain, reconstruct it with an inverse FFT, and compare the noisy and filtered spectrograms.
5. Sweep eight low-pass cutoffs from 300 to 4,000 Hz. For each cutoff, calculate the Pearson correlation between the filtered waveform and the original chord.

## What the cutoff changes

| Low-pass cutoff | Effect on the chord |
|---|---|
| 300 Hz | All three note frequencies lie above the cutoff. |
| 500 Hz | A4 remains, while C♯5 and E5 lie above the cutoff. |
| 700 Hz | All three note frequencies fit inside the passband. |
| 1,000 Hz | All three notes remain, along with more broadband noise than at 700 Hz. |

The notebook also applies a 40–60 Hz band-stop filter and a 100 Hz high-pass filter. Either one suppresses the 50 Hz hum in this experiment. Applying both gives the same result as the high-pass filter alone because it already removes the entire notch band.

The cutoff sweep illustrates the main trade-off: lowering the cutoff removes more noise, but once it crosses a note frequency, it also removes part of the chord. Pearson correlation measures waveform similarity and is insensitive to overall gain, so the notebook includes audio files for listening as well.

## Run the notebook

You’ll need Python, Jupyter, and an internet connection on the first run if `thinkdsp.py` is missing.

    git clone https://github.com/Prabhatc887/dsp_noise_cleaning.git
    cd dsp_noise_cleaning
    python -m pip install notebook numpy scipy matplotlib
    jupyter notebook noise_cleaning.ipynb

Run the cells from top to bottom. The notebook sets the random seed to `42` so the noise realization is repeatable. It creates an `outputs/` directory for the plots and audio files.

## Outputs

| Files | What to look or listen for |
|---|---|
| `01_time_domain.png` | The first 20 ms of the clean and noisy waveforms |
| `02_spectrum_noisy.png` | The 50 Hz hum peak and the three note frequencies |
| `03_spectrograms.png` | A side-by-side comparison before and after filtering |
| `04_cutoff_sweep.png` | How waveform correlation changes as the cutoff crosses the note frequencies |
| `clean.wav`, `noisy.wav`, `filtered.wav` | The original chord, the added interference, and the filtered result |

## Acknowledgment

The notebook uses Allen Downey’s [Think DSP](https://github.com/AllenDowney/ThinkDSP) library for signal generation, spectral analysis, and audio output.
