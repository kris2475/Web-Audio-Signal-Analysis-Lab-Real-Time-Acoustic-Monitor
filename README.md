# Web Audio Signal Analysis Lab: Real-Time Acoustic Monitor

**Web Audio Signal Analysis Lab** is a comprehensive, single-page web application designed to demonstrate the power of the modern **Web Audio API** for signal processing. Developed entirely using **HTML**, **CSS (Tailwind)**, and **Vanilla JavaScript**, this project transforms your browser into a functional laboratory for acoustic analysis.

It serves as an essential tool for developers and enthusiasts interested in understanding how sound—captured from a live microphone stream—can be digitally processed and visualised in real time. The core functionality centres around two fundamental signal-processing techniques: **Energy Analysis (Root Mean Square)** and **Frequency Analysis (Fast Fourier Transform)**, providing immediate visual feedback on the characteristics of the captured audio.

---

## 🚀 Purpose and Features

The primary purpose of this application is to serve as a demonstrator for audio signal processing within a web environment, specifically addressing the need for secure, live microphone access.

### Feature Overview

| Feature | Description | Technical Basis |
|--------|-------------|----------------|
| **Real-Time Input** | Captures audio stream from the user's default microphone. | `navigator.mediaDevices.getUserMedia({ audio: true })` |
| **Energy Analysis (RMS)** | Calculates the Root Mean Square (RMS) amplitude, a standard measure of audio signal strength or loudness. | `AnalyserNode.getByteTimeDomainData()` |
| **Peak Frequency (FFT)** | Identifies the single loudest frequency present in the current audio sample (the fundamental frequency). | `AnalyserNode.getByteFrequencyData()` |
| **Visualisation** | Displays the calculated energy (as a progress bar) and the entire frequency spectrum (as a bar chart). | HTML/CSS transitions, `requestAnimationFrame` |
| **Security Compliant** | Runs on HTTPS (via GitHub Pages) to satisfy browser security requirements for microphone access. | Deployment via GitHub Pages |

---

## ⚙️ Technical Deep Dive

The core functionality relies on chaining several nodes within the Web Audio API:

### AudioContext  
The container for all audio operations.

```js
audioContext = new (window.AudioContext || window.webkitAudioContext)();
```

### MediaStreamSource  
Takes the raw audio stream from the microphone.

```js
micSource = audioContext.createMediaStreamSource(micStream);
```

### AnalyserNode  
The central signal-processing unit.  
It does *not* modify the audio stream; instead, it exposes real-time frequency and time-domain data.

- **fftSize:** 1024 (determines FFT resolution; larger values = better frequency resolution but increased latency)  
- **frequencyBinCount:** Automatically half of fftSize (512 bins), covering frequencies from **0 Hz** to the **Nyquist frequency** (half the sampling rate)

---

### Data Acquisition and Analysis

#### RMS Calculation (Energy)  
`getByteTimeDomainData()` fills an array with waveform amplitude values (0–255).  
This array is then analysed to compute the **Root Mean Square (RMS)**, representing the current audio energy or loudness.

#### Peak Frequency Calculation (FFT)  
`getByteFrequencyData()` creates a second array containing the magnitude of each frequency bin.  
The script:

1. Finds the index of the loudest bin  
2. Converts that bin index into frequency (Hz) based on the sample rate and fftSize  

This yields the **dominant (fundamental) frequency** in real time.

#### Rendering Loop  
All analysis is performed within:

```js
requestAnimationFrame(analyseMic)
```

This keeps analysis and visualisation synchronised with the browser’s repaint cycle for smooth, low-latency updates.

---

## 💻 Deployment and Usage

### Live Application URL

Because `navigator.mediaDevices.getUserMedia` requires HTTPS, the application is hosted on GitHub Pages:

👉 **[Live Analyser App]**  
**https://kris2475.github.io/Microphone-Analyser/mic_analyser.html**

> **Note:** Using Incognito/Private Mode is recommended for fresh microphone permissions, though it will not fix server-side 404 errors.

---

### Running Locally

To run the application locally, you **must** use a local HTTPS-enabled server (Node.js, Python, or a browser extension).  
Opening the HTML file directly via `file://` will fail due to blocked microphone access.

---

### Usage Instructions

**IMPORTANT:** For consistent results, open the application in **Incognito/Private Mode** to force a fresh microphone permission request.

1. Navigate to the Live Application URL.  
2. Click **“Start Microphone Pick Up.”**  
3. Allow microphone access when prompted.  
4. The following will update in real time:
   - **RMS Amplitude** (loudness)
   - **FFT Spectrum Visualisation**
   - **Peak Frequency (Hz)**
5. Click **“Stop Microphone Pick Up”** to end the analysis and release the microphone.
