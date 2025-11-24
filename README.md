# QSFPGA: Quantum Simulation Inspired FPGA

**A framework for deploying Quantum-Inspired Machine Learning models onto low-power FPGA hardware using bit-accurate simulation and automatic Verilog generation.**

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-Quantum-orange)
![FPGA](https://img.shields.io/badge/Hardware-Verilog%20%2F%20HLS-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

## 🚀 Overview

**QSFPGA** bridges the gap between Quantum Machine Learning (QML) and Edge AI. It allows researchers to design "Quantum-Inspired" algorithms (Tensor Networks, Holographic Compression) in PyTorch, simulate them with hardware-accurate constraints (Fixed-Point Arithmetic), and automatically generate Verilog RTL for physical deployment on FPGAs (e.g., Xilinx Artix-7, Pynq).

This project implements a **Holographic Image Compression** pipeline that:
1.  Encodes images into a quantum state (Amplitude Encoding).
2.  Processes them using unitary rotations (Quantum Gates).
3.  Runs on a simulated 10-qubit FPGA core with **Q3.13 Fixed-Point precision**.
4.  Reconstructs the image with negligible loss (`MSE ≈ 0.00`).

## 🧠 The Architecture

The workflow consists of three stages, all contained within the `QSFPGA.ipynb` notebook:
### 1. The "Digital Twin" (PyTorch)
A differentiable quantum circuit simulator used to train rotation angles ($\theta$) via backpropagation. It learns the optimal parameters to represent data in a Hilbert space.

### 2. The Bit-Accurate Simulator (Python)
A custom hardware emulator that replaces floating-point math with **Fixed-Point Arithmetic (Q3.13)**.
- **Class `QFixed`:** Simulates 16-bit integer math, overflows, and quantization noise.
- **Class `Quantum_FPGA_Core`:** Simulates the DSP (Digital Signal Processing) pipeline, memory access patterns, and clock cycles of a physical chip.
### 3. Auto-HDL Generator
A script that translates the verified Python object model directly into **Verilog (`.v`)**.
- Generates `quantum_dsp.v`: The butterfly unit (DSP48 slices).
- Generates `quantum_core_top.v`: The memory controller and state machine.
## 📊 Key Results

### 1. Hardware Efficiency
The architecture is optimized for extremely low-resource FPGAs:
| Metric | Value | Notes |
| :--- | :--- | :--- |
| **Precision** | Q3.13 (16-bit) | Validated for up to 10-qubit depth |
| **DSP Slices** | 4 | Extremely lightweight (Fits on Lattice iCE40) |
| **On-Chip RAM** | ~4 KB | For 10-qubit state storage |
| **Throughput** | ~1 ms | For 5-layer circuit at 100MHz |
 
### 2. Holographic Compression Demo
We successfully encoded a 32x32 image into a simulated FPGA quantum state, applied a mixing layer (encryption/compression), and reconstructed it.
*   **Input:** 1024 Normalized Pixels.
*   **Intermediate:** "Purple Noise" (The Holographic State).
*   **Output:** Perfect reconstruction with minimal quantization noise.
---

## 📂 Project Structure & Notebooks
 
### 1. Core Prototype
*   **`QSFPGA.ipynb`**: The main prototype implementing the Holographic Image Compression pipeline. Contains the PyTorch Digital Twin, Bit-Accurate Simulator, and Auto-HDL Generator.
 
### 2. Advanced Modules
*   **`QSFPGA_RN.ipynb`**: **Hardware Random Number Generator (RNG) Benchmark**. Simulates quantum randomness on FPGA hardware, testing various circuit configurations (Hadamard, Rotated, Entangled, Parallel) for entropy generation.
*   **`QSFPGA_QuILT.ipynb`**: **Quantum-Inspired Learning / Universal Quantum FPGA Core (QuILT)**. An upgraded processor design supporting CNOT and RZ gates, capable of running quantum neural networks.
*   **`QSFPGA_QuILT_NAS.ipynb`**: **Neural Architecture Search (NAS)**. Uses the QuILT core to autonomously search for optimal quantum circuit architectures for specific tasks (e.g., arithmetic).
 
---
 
## 🛠️ How to Run
 
1.  **Open a Notebook:**
Load `QSFPGA.ipynb` (or any other notebook) in Google Colab or Jupyter.
 
2.  **Execute the Pipeline:**
     - **Step 1:** Run the PyTorch block to understand the math.
     - **Step 2:** Run the `Quantum_FPGA_Core` to verify bit-accuracy.
     - **Step 3:** View the "Holographic Demo" (Matplotlib visualization).
     - **Step 4:** The script will generate `quantum_dsp.v` and `quantum_core_top.v` in your directory.

3.  **Hardware Synthesis (Optional):**
    Take the generated `.v` files and load them into Xilinx Vivado or an open-source toolchain (Yosys) to synthesize the bitstream.

---

## 🔗 Inspiration & References

This project builds upon concepts from:
*   [**Quantum-Holographic-Compression**](https://github.com/peterbabulik/Quantum-Holographic-Compression) - Peter Babulik
*   [**QuantumPyTorch**](https://github.com/peterbabulik/QuantumPyTorch-Differentiable-Quantum-Circuits) - Peter Babulik
*   [**Quantum Inspired Learning In Tensor Space**](https://github.com/peterbabulik/QuILT) - Peter Babulik
---

## 📜 License

MIT License. Free for research and educational use.
