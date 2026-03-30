# 🎶 **PART 1 — FOUNDATIONS (Revised for Practical, Playable Learning)**

Each module now includes:

- **UI components** (sliders, knobs, buttons)
- **Playable interaction** (keyboard or MIDI)
- **Incremental synth building**

By the end of Part 1, learners will have built a **fully playable subtractive synth**.

---

# **Module 1 — Web Audio API Basics + First Playable Tone**

### 🎯 Learning Objectives

- Understand AudioContext, Oscillator, Gain
- Build a minimal UI
- Add keyboard input to trigger a note

### 🛠 Practical Work

- Create a simple HTML interface:
    - Waveform dropdown
    - Frequency slider
    - Volume slider
    - “Play note” button
- Add **computer keyboard input**:
    - Press “A” → play 440 Hz
    - Release “A” → stop

### 🎹 Why this matters

Even with one note, learners feel like they’re _playing_ something.

---

# **Module 2 — ADSR Envelopes + 4 Vertical Sliders**

### 🎯 Learning Objectives

- Understand ADSR
- Use `setValueAtTime` and `linearRampToValueAtTime`
- Trigger notes with smooth attack/decay

### 🛠 Practical Work

- Add **four vertical sliders** (Attack, Decay, Sustain, Release)
- Style them like real synth sliders
- Add a “Note On” and “Note Off” button
- Update keyboard input so pressing a key triggers ADSR

### 🎹 Why this matters

Now the synth _feels_ like an instrument — no more abrupt on/off.

---

# **Module 3 — Filters + Filter Envelope UI**

### 🎯 Learning Objectives

- Understand subtractive synthesis
- Use BiquadFilterNode
- Add filter cutoff, resonance
- Add filter envelope

### 🛠 Practical Work

- Add **two more vertical sliders**:
    - Filter cutoff
    - Resonance
- Add a toggle for filter envelope on/off
- Add a slider for filter envelope amount

### 🎹 Why this matters

This is the moment the synth starts sounding like a real analog instrument.

---

# **Module 4 — Polyphony + Full Keyboard Layout**

### 🎯 Learning Objectives

- Manage multiple voices
- Implement voice stealing
- Map computer keyboard to musical notes

### 🛠 Practical Work

- Add a **visual piano keyboard** (clickable)
- Add full QWERTY → piano mapping:
    
    ```
    A W S E D F T G Y H U J K
    ```
    
- Each key press creates a new voice
- Each key release triggers the release phase

### 🎹 Why this matters

Learners can now _play melodies_, not just single notes.

---

# **Module 5 — Step Sequencer (Mini Version)**

### 🎯 Learning Objectives

- Understand timing
- Build a 16‑step grid
- Trigger notes on steps

### 🛠 Practical Work

- Add a **16‑step sequencer grid** under the synth
- Each step can toggle on/off
- Add tempo control
- Add play/stop transport

### 🎹 Why this matters

Now the synth can play itself — a huge motivational boost.

---

# **Module 6 — Drum Machine (Mini Version)**

### 🎯 Learning Objectives

- Load samples
- Trigger samples
- Build a drum grid

### 🛠 Practical Work

- Add a simple 3‑track drum machine:
    - Kick
    - Snare
    - Hi‑hat
- Add a 16‑step grid for each
- Sync it with the synth’s sequencer

### 🎹 Why this matters

Learners now have a **full groove box**: synth + drums + sequencer.

---

# **Module 7 — Modular Concepts (Intro Only)**

### 🎯 Learning Objectives

- Understand modular routing
- Learn about oscillators, filters, envelopes as modules
- Prepare for Part 2

### 🛠 Practical Work

- Add a simple “module view” diagram that updates as the synth grows
- Show signal flow visually:
    
    ```
    OSC → FILTER → AMP → OUTPUT
    ```
    

### 🎹 Why this matters

This sets the stage for the modular environment in Part 2.

---

# 🎯 **How the Practical Build Evolves Across Modules**

Here’s the progression visually:

```
Module 1: Tone Generator
  UI: Waveform, frequency, volume, keyboard input

Module 2: ADSR Synth
  UI: 4 ADSR sliders, smoother playability

Module 3: Subtractive Synth
  UI: Filter cutoff, resonance, filter envelope

Module 4: Polyphonic Synth
  UI: Visual keyboard, multi‑voice engine

Module 5: Synth + Sequencer
  UI: 16‑step grid, tempo, transport

Module 6: Synth + Drum Machine
  UI: 3‑track drum grid, synced playback

Module 7: Synth + Drums + Sequencer + Signal Flow Diagram
  UI: Visual routing, modular concepts
```

By the end of Part 1, learners have built:

- A playable polyphonic subtractive synth
- A drum machine
- A sequencer
- A synced groove box
- A visual signal flow diagram

This is _massive_ value for a beginner series.

---

Part 1 filesystem

web-audio-synth-course/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── curriculum-outline.md
│   ├── part1-overview.md
│   ├── module-diagrams/
│   │   ├── module1-basic-synth.md
│   │   ├── module2-adsr.md
│   │   ├── module3-filter.md
│   │   ├── module4-polyphony.md
│   │   ├── module5-sequencer.md
│   │   ├── module6-drum-machine.md
│   │   └── module7-modular-concepts.md
│   └── images/ (optional future diagrams)
│
├── shared/
│   ├── css/
│   │   └── synth-theme.css
│   ├── js/
│   │   ├── utils/
│   │   │   ├── keyboard-mapping.js
│   │   │   ├── midi-handler.js
│   │   │   └── audio-helpers.js
│   │   └── ui/
│   │       ├── slider.js
│   │       ├── knob.js
│   │       └── piano-keyboard.js
│   └── assets/
│       └── samples/
│           ├── kick.wav
│           ├── snare.wav
│           └── hat.wav
│
└── modules/
    ├── module1-basic-synth/
    │   ├── start/
    │   │   ├── index.html
    │   │   ├── style.css
    │   │   └── main.js
    │   ├── final/
    │   │   ├── index.html
    │   │   ├── style.css
    │   │   └── main.js
    │   └── README.md
    │
    ├── module2-adsr-envelope/
    │   ├── start/
    │   ├── final/
    │   └── README.md
    │
    ├── module3-filter-synthesis/
    │   ├── start/
    │   ├── final/
    │   └── README.md
    │
    ├── module4-polyphony/
    │   ├── start/
    │   ├── final/
    │   └── README.md
    │
    ├── module5-step-sequencer/
    │   ├── start/
    │   ├── final/
    │   └── README.md
    │
    ├── module6-drum-machine/
    │   ├── start/
    │   ├── final/
    │   └── README.md
    │
    └── module7-modular-concepts/
        ├── start/
        ├── final/
        └── README.md


---

Part 1 readme 

# Web Audio Synth Course

A complete hands-on course teaching Web Audio API by building a real synthesizer,
step sequencer, and drum machine in the browser.

## Part 1: Foundations
- Module 1: Basic Synth
- Module 2: ADSR Envelope
- Module 3: Filter Synthesis
- Module 4: Polyphony
- Module 5: Step Sequencer
- Module 6: Drum Machine
- Module 7: Modular Concepts

## How to Use This Repo
- Each module has a `start` and `final` version.
- Open `index.html` in your browser to run the module.
- Follow the docs in `/docs` for explanations and diagrams.

## Requirements
- Modern browser (Chrome, Edge, Firefox)
- No build tools required