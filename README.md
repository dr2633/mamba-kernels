# Mamba Custom Kernels for Optimized Memory Management and Inference

This repository focuses on developing custom CUDA and Metal kernels optimized explicitly for efficient inference with the Mamba model architecture. It emphasizes memory management, GPU utilization, and inference performance, aiming to provide a practical, highly optimized toolkit for accelerating Mamba inference workloads.

## Motivation

- To enhance inference speed and efficiency for Mamba-based models.
- To optimize GPU memory management and maximize throughput on both NVIDIA GPUs (CUDA) and Apple Silicon GPUs (Metal).
- To create clear, reusable kernels that can be leveraged in various inference workloads.

## 📂 Repository Structure

```
mamba-kernels/
├── README.md
├── docs/                   # Documentation and results
├── kernels/
│   ├── cuda/               # CUDA kernels
│   └── metal/              # Metal kernels for Apple Silicon GPUs
├── benchmarks/             # Scripts and results for benchmarking performance
├── notebooks/              # Interactive notebooks demonstrating kernel usage
├── scripts/                # Benchmarking, testing, and profiling scripts
├── pyproject.toml          # Project metadata and dependency management
└── requirements.txt        # Explicit Python dependencies
```

## Getting Started

### Step 1: Clone the repository

```bash
git clone <repo_url>
cd mamba-kernels
```

### Step 2: Install dependencies

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Step 2: Kernel Compilation

#### CUDA Kernels:

Navigate to CUDA kernels and compile:

```bash
cd kernels/cuda
make
```

### Step 2: Run a benchmarking script

```bash
python scripts/benchmark.py --kernel cuda
```

---

## Key Areas of Development

- **CUDA Custom Kernels**: Memory-optimized kernels for faster Mamba inference.
- **Metal Kernels**: GPU optimization specifically for Apple Silicon.
- **Benchmarks**: Clear performance comparisons of kernel implementations against baseline Mamba implementations.

---

## ⚙Dependencies

- Python >=3.8
- CUDA Toolkit (for CUDA kernels)
- Xcode (for Metal kernels)
- PyTorch
- Mamba SSM

---

## Contributions

Contributions are welcome! Feel free to submit pull requests or issues clearly documenting proposed changes or improvements.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

