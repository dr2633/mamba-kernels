# GPU Benchmark and Profiling Setup Guide

This guide provides structured instructions to build, deploy, and benchmark your CUDA-enabled environment for optimizing inference performance. Note that **Nsight Systems** is no longer included in the Dockerfile.

---

# Step-by-step Setup Guide

## Step 1: Docker Image Build

Build your Docker image with:

```bash
docker build --platform linux/amd64 -t mamba_kernel_optimization .
```

### Push Docker Image to Docker Hub

```bash
docker login
docker tag mamba_kernel_optimization dr2633/mamba_kernel_optimization:latest
docker push dr2633/mamba_kernel_optimization:latest
```

---

## Step 2: Generate and Add SSH Key

Generate SSH key:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Add public key (`~/.ssh/id_ed25519.pub`) to Lambda Cloud dashboard.

---

## Step 3: Connect to Lambda GPU Instance

SSH into Lambda instance:

```bash
ssh -i ~/.ssh/id_ed25519 ubuntu@your_lambda_instance_ip
```

Replace `your_lambda_instance_ip` with the instance IP from Lambda dashboard.

---

## Step 2: Install Docker & NVIDIA Docker on Lambda Instance

Run these commands on the remote instance:

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
&& curl -fsSL https://get.docker.com | sh \
&& sudo usermod -aG docker $USER \
&& newgrp docker \
&& curl -s -L https://get.docker.com | sh

```

---

## Step 4: Run Docker Container with GPU

Pull and run your Docker container:

```bash
docker pull your_dockerhub_username/mamba_kernel_optimization:latest

docker run --gpus all --cap-add=SYS_ADMIN -it your_dockerhub_username/mamba_kernel_optimization:latest
```

Verify GPU availability inside the container:

```bash
nvidia-smi
python3 -c 'import torch; print("CUDA available:", torch.cuda.is_available())'
```

Expected output:

```
CUDA available: True
```

---

## Step 2: Baseline Performance Benchmark

Create a `benchmark.py` script:

```python
import torch
from mem0 import Mem0

model = Mem0.from_pretrained('path/to/model').cuda().eval()

input_tensor = torch.randn(batch_size, seq_length, model_dim).cuda()

# Warm-up
for _ in range(10):
    _ = model(input_tensor)

# Timed benchmark
torch.cuda.synchronize()
import time
start_time = time.time()
iterations = 100

for _ in range(iterations):
    _ = model(input_tensor)

torch.cuda.synchronize()
end_time = time.time()

elapsed_time = (end_time - start_time) / iterations

print(f"Average latency per inference: {elapsed_time*1000:.2f} ms")
```

Run the benchmark:



```bash
git clone https://github.com/dr2633/mamba_kernel_optimization.git
python3 benchmark.py
```

---

## Step 2.5: Record Baseline Results

Clearly document these metrics:

- GPU hardware
- CUDA version
- PyTorch version
- Inference latency (ms)
- GPU utilization & memory consumption

---

## Step 3: Version Control Your Baseline Results

Commit your baseline results clearly:

```bash
git add benchmark.py
git commit -m "Add baseline performance results"
git push
```

---

You're now ready to optimize and iterate on GPU inference workloads.
