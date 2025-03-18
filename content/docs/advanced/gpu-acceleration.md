---
title: "GPU Acceleration"
weight: 3
---

# GPU Acceleration for Ollama

Running Ollama on a GPU can significantly improve performance, especially for larger models. This guide will help you set up and optimize GPU acceleration for Ollama.

## Prerequisites

- A compatible NVIDIA GPU (RTX series recommended for best performance)
- CUDA installed (version 11.7 or newer)
- Sufficient GPU memory for your model of choice

## Hardware Requirements

Here are the approximate GPU memory requirements for different model sizes:

| Model Size | Minimum GPU Memory | Recommended GPU Memory |
|------------|-------------------|------------------------|
| 3B-7B      | 4GB               | 8GB                    |
| 13B        | 10GB              | 16GB                   |
| 33B-34B    | 20GB              | 32GB                   |
| 70B        | 40GB              | 80GB                   |

## GPU Setup on Linux

### Installing NVIDIA Drivers and CUDA

1. **Install NVIDIA drivers:**

   For Ubuntu/Debian:
   ```bash
   sudo apt update
   sudo apt install nvidia-driver-535  # or newer version
   ```

   For CentOS/RHEL:
   ```bash
   sudo dnf install nvidia-driver
   ```

2. **Install CUDA:**

   Download and install CUDA from the [NVIDIA website](https://developer.nvidia.com/cuda-downloads).

   For Ubuntu/Debian (example):
   ```bash
   wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
   sudo sh cuda_11.8.0_520.61.05_linux.run
   ```

3. **Add CUDA to your PATH:**

   Add the following to your `~/.bashrc` or `~/.zshrc`:
   ```bash
   export PATH="/usr/local/cuda/bin:$PATH"
   export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
   ```

4. **Verify installation:**
   ```bash
   nvidia-smi
   nvcc --version
   ```

### Running Ollama with GPU

Once CUDA is installed, Ollama should automatically detect and use your GPU. You can verify this by running a model and checking the GPU usage:

```bash
ollama run llama2
```

Use `nvidia-smi` in another terminal to monitor GPU usage.

## GPU Setup on Windows

To use Ollama with GPU acceleration on Windows:

1. **Install the latest NVIDIA drivers** for your GPU from the [NVIDIA website](https://www.nvidia.com/Download/index.aspx).

2. **Install Windows Subsystem for Linux (WSL)** with Ubuntu:
   ```powershell
   wsl --install -d Ubuntu
   ```

3. **Install CUDA on WSL:**
   Follow the Linux instructions above in your WSL environment.

4. **Install Ollama in WSL:**
   ```bash
   curl -fsSL https://ollama.ai/install.sh | sh
   ```

5. **Run Ollama:**
   ```bash
   ollama serve
   ```

## GPU Setup on macOS

For Apple Silicon Macs (M1, M2, M3), Ollama automatically uses the Metal Performance Shaders (MPS) to accelerate model inference. No additional setup is required.

## Verifying GPU Usage

To verify that Ollama is using your GPU:

### On Linux/Windows (WSL)

1. Start Ollama and run a model.

2. In another terminal, run:
   ```bash
   nvidia-smi
   ```

   You should see the Ollama process using GPU memory and compute resources.

### On macOS

On Apple Silicon Macs, you can use Activity Monitor to verify GPU usage:

1. Open Activity Monitor (Applications > Utilities > Activity Monitor)
2. Select the "GPU" tab
3. Look for the Ollama process and check its GPU usage

## Optimizing GPU Performance

### Memory Efficiency vs. Speed

You can trade memory usage for speed with different quantization levels:

```bash
# More memory efficient but slower
ollama run llama2:7b-q4_0

# Less memory efficient but faster
ollama run llama2:7b-q5_1
```

### Batch Size and Context Length

You can adjust the batch size and context length using environment variables:

```bash
OLLAMA_BATCH_SIZE=32 ollama run llama2
```

A larger batch size can improve throughput but requires more GPU memory.

### Multi-GPU Support

Ollama does not yet support automatic multi-GPU splitting for a single model. However, you can run different models on different GPUs by setting the `CUDA_VISIBLE_DEVICES` environment variable:

```bash
# Run on the first GPU
CUDA_VISIBLE_DEVICES=0 ollama run llama2 &

# Run on the second GPU
CUDA_VISIBLE_DEVICES=1 ollama run mistral &
```

## Common Issues and Solutions

### Out of Memory Errors

If you encounter out of memory errors:

1. **Use a smaller model variant** (e.g., 7B instead of 13B)
2. **Try a more aggressive quantization** (e.g., q4_0 instead of q5_1)
3. **Reduce the context length** with the `OLLAMA_CONTEXT_LENGTH` environment variable
4. **Close other GPU-intensive applications**

### Slow Inference

If inference is slower than expected:

1. **Increase the batch size** with the `OLLAMA_BATCH_SIZE` environment variable
2. **Update to the latest NVIDIA drivers and CUDA**
3. **Monitor for thermal throttling** using `nvidia-smi -l 1`
4. **Check system for other processes** using GPU resources

### CUDA Not Found

If Ollama can't find CUDA:

1. **Verify CUDA installation** with `nvcc --version`
2. **Check environment variables** (PATH and LD_LIBRARY_PATH)
3. **Restart Ollama** after updating environment variables
4. **Reinstall CUDA** if necessary

## Performance Benchmarks

Here are some approximate performance comparisons between CPU and GPU for different model sizes:

| Model Size | CPU (tokens/sec) | GPU (tokens/sec) | Speedup |
|------------|------------------|------------------|---------|
| 7B         | 2-5              | 30-40            | ~10x    |
| 13B        | 1-3              | 20-30            | ~10x    |
| 33B-34B    | 0.5-1            | 10-15            | ~15x    |
| 70B        | 0.2-0.5          | 5-10             | ~20x    |

*Note: Actual performance varies based on hardware specifications, quantization, and other factors.*

## Next Steps

- Learn about [model quantization](/docs/advanced/quantization/) for better performance
- Explore [Docker integration](/docs/advanced/docker/) for containerized GPU deployments
- Check out [performance optimization](/docs/advanced/performance/) for more general tips
