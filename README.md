<p align="center">
  <img src="assets/logo.jpg" alt="Project Beatrice V2 Logo" width="150" />
</p>

# Beatrice V2 Voice Trainer (macOS Edition)

[![Platform: macOS](https://img.shields.io/badge/Platform-macOS%2012.3%2B-blue?logo=apple&logoColor=white&style=flat-square)](#)
[![Python: 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white&style=flat-square)](#)
[![PyTorch: 2.0+](https://img.shields.io/badge/PyTorch-2.0%2B-orange?logo=pytorch&logoColor=white&style=flat-square)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](#)

A premium, hardware-aware desktop Web interface for training custom voice conversion models using **Beatrice V2** on macOS. This frontend-backend application features automatic hardware profiling for Apple Silicon and Intel Macs, unified memory management, robust dataset importing, real-time training analytics, and a cloud fallback bridge for training on Google Colab.

---

## Key Features

* 🍎 **Apple Silicon (MPS) Acceleration** — Leverage your Mac's integrated GPU via PyTorch Metal Performance Shaders (MPS) with fallbacks for high-performance CPU training.
* ⚙️ **Automatic Hardware Auto-Tuning** — Scans your system (unified memory budget, CPU counts, core profiles) and computes optimal training parameters (batch size, gradient accumulation steps, worker count).
* 📁 **Comprehensive Dataset Importer** — Drag & drop local `.wav` or `.zip` files, paste Hugging Face dataset IDs (with automated audio stream decoding), or input Kaggle dataset references.
* 📈 **Interactive Training Monitor** — Live-updating loss curves plotted via HTML Canvas, real-time steps-per-second, RAM & GPU allocation gauges, and stdout log terminal.
* ☁️ **Google Colab Bridge** — Seamlessly package and export your locally managed dataset into a single ZIP for deployment to Google Colab's free cloud GPUs (ideal for lengthy training runs).
* 📱 **Responsive Glassmorphism UI** — A premium, modern interface with a responsive mobile navigation drawer, fluid animations, and visual states.

---

## Quick Start

### 1. Requirements

* **OS**: macOS Monterey (12.3) or newer.
* **Architecture**: Apple Silicon (M1/M2/M3/M4 Series recommended) or Intel.
* **Interpreter**: Python 3.10 or newer.

### 2. Launch the Web App

Simply open your terminal, navigate to the project directory, and execute the startup script:

```bash
chmod +x start.sh
./start.sh
```

#### What the startup script does:
1. Creates a Python virtual environment (`venv/`) if it doesn't exist.
2. Installs required dependencies (`fastapi`, `uvicorn`, `python-multipart`, `aiofiles`, `huggingface_hub`).
3. Downloads the core Beatrice Trainer repository from Hugging Face.
4. Auto-detects local hardware capabilities.
5. Launches the backend server and opens the Web UI in your default browser at `http://localhost:8000`.

---

## Directory Structure

```text
├── assets/                  # UI assets and logos
│   └── logo.jpg             # Project Beatrice V2 Logo
├── app.js                   # Client-side WebUI application logic
├── index.html               # Frontend HTML structure
├── index.css                # Premium Glassmorphism Design System
├── server.py                # FastAPI Python server (Static file server & REST API)
├── start.sh                 # Startup shell script (handles environment setup)
├── requirements.txt         # Core backend python dependencies
├── LICENSE                  # MIT License
└── beatrice-trainer/        # Downloaded trainer package (excluded from git)
```

---

## Mac Hardware Auto-Tuning Profiles

When you select a speaker dataset in the **Training** tab, the system runs deep profiling of your unified memory to prevent out-of-memory crashes:

| Unified Memory | Optimal Batch Size | Gradient Accumulation | Effective Batch Size | Workers | Mixed Precision |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **64GB+ (M-Max/Ultra)** | 16 | 1 | 16 | 0 | Disabled (NaN protection) |
| **32GB (M-Pro/Max)** | 8 | 2 | 16 | 0 | Disabled (NaN protection) |
| **16GB (M-Pro/Base)** | 4 | 2 | 8 | 0 | Disabled (NaN protection) |
| **8GB (M-Base)** | 2 | 4 | 8 | 0 | Disabled (NaN protection) |

> [!NOTE]
> Training workers are set to `0` on macOS to prevent pickling errors in PyTorch multiprocessing, and mixed precision is disabled for feature extraction stability (preventing NaN/pitch detection failures on Apple Silicon).

---

## Contributing & License

Feel free to open issues or pull requests. Project Beatrice V2 Voice Trainer is licensed under the [MIT License](LICENSE).
