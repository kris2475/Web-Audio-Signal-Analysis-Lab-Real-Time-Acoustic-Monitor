Web Audio Signal Analysis Lab: Real-Time Acoustic Monitor

Welcome to the Web Audio Signal Analysis Lab, a comprehensive, single-page web application designed to demonstrate the power of the modern Web Audio API for signal processing. Developed entirely using HTML, CSS (Tailwind), and Vanilla JavaScript, this project transforms your browser into a functional laboratory for acoustic analysis. It serves as an essential tool for developers and enthusiasts interested in understanding how sound, captured from a live microphone stream, can be digitally processed and visualised in real time. The core functionality centres around two fundamental signal processing techniques: Energy Analysis (Root Mean Square) and Frequency Analysis (Fast Fourier Transform), providing immediate visual feedback on the characteristics of the captured audio.

🚀 Purpose and Features

The primary purpose of this application is to serve as a demonstrator for audio signal processing within a web environment, specifically addressing the need for secure, live microphone access.

Feature

Description

Technical Basis

Real-Time Input

Captures audio stream from the user's default microphone.

navigator.mediaDevices.getUserMedia({ audio: true })

Energy Analysis (RMS)

Calculates the Root Mean Square (RMS) amplitude, a standard measure of audio signal strength or loudness.

AnalyserNode.getByteTimeDomainData()

Peak Frequency (FFT)

Identifies the single loudest frequency present in the current audio sample (the fundamental frequency).

AnalyserNode.getByteFrequencyData()

Visualisation

Displays the calculated energy (as a progress bar) and the entire frequency spectrum (as a bar chart).

HTML/CSS transitions, requestAnimationFrame

Security Compliant

Runs on https (via GitHub Pages) to satisfy modern browser security requirements for microphone access.

Deployment via GitHub Pages

⚙️ Technical Deep Dive

The core functionality of this script relies on chaining several nodes within the Web Audio API:

AudioContext: The container for all audio operations.

audioContext = new (window.AudioContext || window.webkitAudioContext)();


MediaStreamSource: The input node that takes the raw audio stream from the microphone.

micSource = audioContext.createMediaStreamSource(micStream);


AnalyserNode: This is the core signal processing unit. It doesn't modify the audio stream but provides real-time frequency and time-domain data.

fftSize: Set to 1024. This determines the size of the Fast Fourier Transform. A larger size provides better frequency resolution but increases latency.

frequencyBinCount: This read-only property is always half of the fftSize (512 bins in this case), representing the frequency data from $0 \text{ Hz}$ up to the Nyquist frequency (half the sample rate).

Data Acquisition and Analysis:

RMS Calculation (Energy): The getByteTimeDomainData() method fills an array with waveform amplitudes (ranging from $0$ to $255$). This array is processed to calculate the Root Mean Square (RMS) value, providing the current audio energy level.

Peak Frequency Calculation (FFT): The getByteFrequencyData() method fills a second array with the magnitude (loudness) of each frequency bin. The script iterates through this array to find the bin with the highest magnitude, and then calculates the corresponding frequency (in Hertz) using the known sample rate and fftSize.

Rendering Loop: The analysis is performed inside a loop controlled by requestAnimationFrame(analyseMic). This ensures that analysis and visualisation updates are tightly synchronised with the browser's repaint cycle, maximising efficiency and minimising lag.

💻 Deployment and Usage

Live Application URL

Due to browser security requirements, microphone access (navigator.mediaDevices.getUserMedia) must be done over a secure connection (https://).

The application is live at the following address:

[Live Analyser App] https://kris2475.github.io/Microphone-Analyser/mic_analyser.html

Note: Opening this link in Incognito/Private Mode is strongly recommended to ensure fresh microphone permissions, although it won't resolve a server-side $404$ error.

Running Locally

To run this application on your local machine, you will typically need to serve it using a local HTTP server that supports HTTPS (such as one set up with Node.js or Python, or a browser extension that enables local HTTPS serving). Simply opening the HTML file directly (file://...) will result in a security error (blocked microphone access).

Usage Instructions

IMPORTANT: For consistent results and to ensure fresh microphone permissions, please open the application in Incognito/Private Browsing Mode.

Navigate to the Live Application URL above.

Click the "Start Microphone Pick Up" button.

Your browser will prompt you for microphone permission (allow access).

Once active, the RMS Amplitude will update to show loudness, and the FFT Visualisation and Peak Frequency will update in real-time based on your voice or surrounding sounds.

Click "Stop Microphone Pick Up" to end the analysis and release the microphone resource.
