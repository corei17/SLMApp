# SLMApp — Offline AI Chat on Your Device

> A production-ready offline-first AI chat application built with React Native and Small Language Models (SLMs). No API keys. No internet required. Complete privacy. Runs entirely on your phone.

---

## What Is This?

SLMApp is a mobile AI chat assistant that runs a quantized small language model **directly on your device** — no cloud, no subscriptions, no data leaving your phone. It demonstrates real-world edge AI engineering: model quantization, memory management, battery optimization, and offline-first architecture.

Built as part of a structured AI engineering curriculum focused on **resource-constrained edge AI**.

---

## Features

- **100% Offline** — works with no internet connection after initial setup
- **Zero API Cost** — no OpenAI, no Anthropic, no monthly fees
- **Complete Privacy** — your conversations never leave your device
- **Real-time Streaming** — responses stream token by token, just like ChatGPT
- **Smart Status Indicator** — shows model loading state in real time
- **Cross-platform** — built with React Native, runs on Android and iOS

---

## Tech Stack

| Layer               | Technology                   |
| ------------------- | ---------------------------- |
| Framework           | React Native + Expo          |
| On-device inference | llama.rn                     |
| Language model      | Phi-3 Mini 4K (Q4 quantized) |
| File system         | expo-file-system             |
| Language            | TypeScript                   |

---

## Architecture

```
SLMApp/
├── app/
│   └── (tabs)/
│       └── index.tsx        # Main chat screen + AI inference logic
├── assets/
│   └── models/              # Place your .gguf model file here (not in repo)
├── components/
├── constants/
├── hooks/
└── README.md
```

### Key architectural decisions

**Lazy model loading** — the model loads into memory only when the app starts, not on every message. This preserves RAM for the rest of the phone.

**Token streaming** — instead of waiting for the full response, tokens render one by one as the model generates them. This gives a responsive feel even on slower hardware.

**4-bit quantization** — the Phi-3 Mini model is quantized to 4-bit (Q4), reducing its size from ~7GB to ~2.2GB while preserving most of its intelligence. This makes it feasible to run on a phone with 4–6GB RAM.

**Context window management** — the prompt is structured with a system message and conversation turns, keeping the context window at 2048 tokens to balance memory usage and conversation quality.

---

## Getting Started

### Prerequisites

- Node.js v18 or higher
- Android phone with USB debugging enabled, or iOS device
- ~3GB of free storage on your phone for the model

### 1. Clone the repository

```bash
git clone https://github.com/corei17/SLMApp.git
cd SLMApp
```

### 2. Install dependencies

```bash
npm install
```

### 3. Download the model file

The model is not included in this repository due to its size (~2.2GB). Download it manually:

**Model:** Phi-3 Mini 4K Instruct — Q4 quantized

**Download link:**

```
https://huggingface.co/microsoft/Phi-3-mini-4k-instruct-gguf/resolve/main/Phi-3-mini-4k-instruct-q4.gguf
```

After downloading, place the file here:

```
assets/models/Phi-3-mini-4k-instruct-q4.gguf
```

### 4. Run on Android

Connect your Android phone via USB with USB debugging enabled, then run:

```bash
npx expo run:android
```

In the output, you'll find options to open the app in a

- [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), a limited sandbox for trying out app development with Expo

You can start developing by editing the files inside the **app** directory. This project uses [file-based routing](https://docs.expo.dev/router/introduction).

This takes 5–10 minutes the first time as it compiles native Android code.

### 5. First launch

On first launch the app will show **"Loading..."** in the header for approximately 30 seconds while the model loads into memory. Once the status dot turns **green**, the AI is ready.

---

## How It Works

```
User types message
        ↓
sendMessage() is called
        ↓
Prompt is formatted for Phi-3 (system + user + assistant tags)
        ↓
llama.rn runs inference on-device (CPU)
        ↓
Tokens stream back one by one
        ↓
UI updates in real time as response builds
```

No network requests are made at any point during inference.

---

## Model Details

| Property        | Value                            |
| --------------- | -------------------------------- |
| Model           | Microsoft Phi-3 Mini 4K Instruct |
| Quantization    | Q4 (4-bit)                       |
| File size       | ~2.2 GB                          |
| Context window  | 4,096 tokens                     |
| Parameters      | 3.8 billion                      |
| Recommended RAM | 4 GB minimum                     |

Phi-3 Mini was chosen for its exceptional intelligence-to-size ratio. Despite being only 3.8B parameters, it outperforms many larger models on reasoning and instruction-following tasks.

---

## Roadmap

- [x] Phase 1 — Chat UI with React Native
- [x] Phase 2 — On-device SLM inference with llama.rn
- [ ] Phase 3 — Memory management, battery optimization, sliding context window
- [ ] Phase 4 — Encrypted local chat history with offline-first sync

---

## Important Notes

- The `.gguf` model file is excluded from this repository via `.gitignore`
- First-time model loading takes 20–40 seconds depending on your device
- Inference speed varies by device — expect 3–10 tokens per second on most Android phones
- Older devices (pre-2020) may experience slower performance due to limited CPU threads

---

## License

MIT License — feel free to use, modify, and distribute.

---

## Author

Built by a developer learning edge AI and resource-constrained machine learning on mobile devices.

---

_This project proves that powerful AI does not require the cloud. The future of AI is local._
