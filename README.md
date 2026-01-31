# 📡 Audio-Based Bluetooth Control System (Arduino + Earbuds)

## Overview

This project demonstrates how digital commands can be transmitted wirelessly using **Bluetooth audio signals** and decoded on an Arduino using **frequency analysis**.

Instead of sending data directly through Bluetooth protocols, this system converts commands into **sound waves**, sends them through Bluetooth audio, and then reconstructs the data on the receiver side.

This approach is similar to how old **dial-up modems** worked and is used here for learning and experimentation.

---

## System Concept

The system uses sound as a communication medium.

Digital data → Audio tones → Wireless transmission → Audio decoding → Digital commands

This method is called **Audio Modulation and Demodulation**.

---

## Data Transmission Method

The system uses **Frequency Shift Keying (FSK)**.

Each digital bit is represented by a specific frequency.

Example:

| Signal Type | Frequency |
| ----------- | --------- |
| START       | 1000 Hz   |
| Bit 0       | 1800 Hz   |
| Bit 1       | 2600 Hz   |
| STOP        | 1200 Hz   |

Commands are built by sending sequences of these frequencies.

---

## Command Structure

Each message is sent as a fixed frame:

```
[START] [BIT] [BIT] [STOP]
```

Example:

```
1000Hz → 1800Hz → 1800Hz → 1200Hz
START   0        0        STOP
```

This represents command `00`.

---

## Command Mapping

Two bits are used to represent one command.

| Bits | Command  |
| ---- | -------- |
| 00   | Forward  |
| 01   | Backward |
| 10   | Left     |
| 11   | Right    |

The Arduino interprets these bits and converts them into actions.

---

## System Architecture

The system has four main stages:

1. Signal Generation
2. Wireless Transmission
3. Signal Reception
4. Digital Decoding

---

## Working Principle

### 1. Signal Generation (Phone)

* The phone generates pure sine waves.
* Each wave corresponds to a predefined frequency.
* These waves represent digital bits.

### 2. Bluetooth Audio Transmission

* The phone sends the generated audio through Bluetooth.
* The earbud Bluetooth module receives the audio.
* Audio is compressed and transmitted wirelessly.

### 3. Audio Output

* The earbud PCB converts digital audio to analog signals.
* The speaker output carries the modulated waveform.
* This signal is forwarded to the Arduino input circuit.

### 4. Analog-to-Digital Conversion

* Arduino samples the incoming audio using its ADC.
* The analog waveform is converted into digital samples.
* These samples represent the received signal.

### 5. Frequency Detection

* The Arduino analyzes sampled data.
* The Goertzel algorithm is used to detect specific frequencies.
* Each detected frequency is classified as START, 0, 1, or STOP.

### 6. Frame Interpretation

* The Arduino waits for the START signal.
* Incoming bits are collected.
* When STOP is detected, the frame is closed.
* The collected bits are converted into a command.

### 7. Command Execution

* The decoded command is matched with predefined actions.
* Corresponding outputs (motors, LEDs, etc.) are activated.

---

## Flowchart (For diagrams.net)

[https://app.diagrams.net/](https://drive.google.com/file/d/1IxlsJ3XHQ6XGc9tPLCHqZ4EZtXgkm959/view?usp=sharing)

---

### Main System Flow

```
Start
  ↓
Generate Frequency (Phone)
  ↓
Send via Bluetooth Audio
  ↓
Earbud Receives Audio
  ↓
Convert to Analog Signal
  ↓
Arduino ADC Sampling
  ↓
Frequency Detection (Goertzel)
  ↓
Is START Detected?
  ↓ Yes
Begin Data Reception
  ↓
Detect Bit (0 or 1)
  ↓
Store Bit
  ↓
Enough Bits?
  ↓ Yes
Detect STOP
  ↓
Decode Command
  ↓
Execute Action
  ↓
Wait for Next Frame
```

---

### Decoder Logic Flow

```
Idle State
  ↓
Listen for START
  ↓
START Found?
  ↓ Yes
Clear Buffer
  ↓
Read Frequency
  ↓
1800Hz? → Bit = 0
2600Hz? → Bit = 1
  ↓
Store Bit
  ↓
STOP Found?
  ↓ Yes
Process Bits
  ↓
Trigger Output
  ↓
Return to Idle
```

---

## Why This System Works

* Bluetooth audio reliably transmits sound waves.
* FSK ensures data is encoded in frequency, not volume.
* Goertzel algorithm efficiently detects target frequencies.
* Framing (START/STOP) prevents data confusion.
* Fixed timing keeps synchronization stable.

---

## Technical Limitations

* Bluetooth audio introduces latency.
* Compression slightly distorts signals.
* Environmental noise affects accuracy.
* Data rate is low.
* Not suitable for real-time control systems.

This project prioritizes learning over performance.

---

## Learning Outcomes

By completing this project, students learn:

* Digital to analog data conversion
* Wireless audio transmission
* Frequency modulation techniques
* Signal sampling and ADC usage
* Goertzel frequency detection
* Protocol design and framing
* Error sources in communication systems
* Embedded signal processing

---

## Educational Value

This project bridges multiple fields:

* Embedded Systems
* Digital Signal Processing
* Wireless Communication
* Protocol Engineering
* Electronics

It demonstrates how data can be transmitted without using traditional digital networking protocols.

---

## Conclusion

This Audio-Based Bluetooth Control System shows how sound can be used as a data carrier.

Although inefficient compared to standard Bluetooth modules, it provides deep insight into communication systems and signal processing.

The project focuses on understanding fundamental principles rather than building a commercial product.
