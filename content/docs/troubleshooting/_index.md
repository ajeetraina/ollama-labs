---
title: "Troubleshooting"
weight: 5
---

# Troubleshooting Ollama

This section provides solutions for common issues you might encounter when using Ollama. If you're experiencing problems, check the relevant section below for guidance.

## Installation Issues

### Unable to Install on Linux

**Issue**: The installation script fails on Linux.

**Solutions**:

1. **Check System Requirements**
   - Ensure you have a compatible Linux distribution (Ubuntu 22.04+ recommended)
   - Verify you have at least 4GB of RAM
   - Make sure you have sufficient disk space (at least 5GB free)

2. **Manual Installation**
   ```bash
   # Download the latest release from GitHub
   wget https://github.com/ollama/ollama/releases/latest/download/ollama-linux-amd64
   
   # Make it executable
   chmod +x ollama-linux-amd64
   
   # Move to a directory in your PATH
   sudo mv ollama-linux-amd64 /usr/local/bin/ollama
   ```

3. **Check for Missing Dependencies**
   ```bash
   # Install common dependencies on Ubuntu/Debian
   sudo apt-get update
   sudo apt-get install -y ca-certificates curl gnupg
   ```

### Unable to Install on macOS

**Issue**: Installation fails on macOS.

**Solutions**:

1. **Use Homebrew**
   ```bash
   brew install ollama
   ```

2. **Check macOS Version**
   - Ensure you're running macOS 12 (Monterey) or newer

3. **Install from DMG**
   - Download the latest .dmg file from the [Ollama website](https://ollama.ai)
   - Open the DMG and drag Ollama to your Applications folder

### Unable to Install on Windows

**Issue**: Installation fails on Windows.

**Solutions**:

1. **WSL Method**
   - Install WSL: `wsl --install`
   - Open Ubuntu WSL and install Ollama there

2. **Check Windows Version**
   - Ensure you're running Windows 10 or Windows 11

3. **Windows Defender/Antivirus**
   - Temporarily disable antivirus software during installation
   - Add Ollama to the exclusions list

## Startup Issues

### Ollama Service Won't Start

**Issue**: The Ollama service fails to start or crashes immediately.

**Solutions**:

1. **Check Logs**
   ```bash
   # Linux
   journalctl -u ollama.service -n 50
   
   # macOS
   cat ~/Library/Logs/ollama.log
   ```

2. **Port Conflicts**
   - Check if port 11434 is already in use:
   ```bash
   # Linux/macOS
   lsof -i :11434
   
   # Windows
   netstat -ano | findstr :11434
   ```
   - Stop the process using that port or configure Ollama to use a different port

3. **Restart Ollama**
   ```bash
   # Linux
   sudo systemctl restart ollama
   
   # macOS
   brew services restart ollama
   ```

### Unable to Connect to Server

**Issue**: Getting errors like "unable to connect to server" or "connection refused".

**Solutions**:

1. **Start the Ollama Service**
   ```bash
   ollama serve
   ```

2. **Check Firewall Settings**
   - Ensure your firewall allows connections on port 11434

3. **Check Network Configuration**
   - Make sure you're connecting to the correct host and port
   - Default is `http://localhost:11434`

## Model Issues

### Unable to Pull Models

**Issue**: Cannot download models with `ollama pull modelname`.

**Solutions**:

1. **Check Internet Connection**
   - Verify you have a stable internet connection

2. **Check Disk Space**
   - Ensure you have enough free disk space for the model

3. **Try with `--insecure` Flag**
   ```bash
   OLLAMA_INSECURE=true ollama pull llama2
   ```

4. **Pull from Local Library**
   ```bash
   ollama pull ./path/to/local/model.bin
   ```

### Model Crashes or Out of Memory

**Issue**: The model crashes, or you get "out of memory" errors when running a model.

**Solutions**:

1. **Use a Smaller Model**
   - Try a 7B model instead of 13B or larger
   - For example, use `llama2:7b` instead of `llama2:13b`

2. **Use More Aggressive Quantization**
   ```bash
   ollama run llama2:7b-q4_0 # More memory efficient but lower quality
   ```

3. **Reduce Context Length**
   ```bash
   OLLAMA_CONTEXT_LENGTH=2048 ollama run llama2
   ```

4. **Close Other Applications**
   - Free up system memory by closing other applications

### Slow Model Performance

**Issue**: Model responses are too slow.

**Solutions**:

1. **Use a Smaller Model**
   - Try a 3B or 7B model for faster responses

2. **Increase Batch Size**
   ```bash
   OLLAMA_BATCH_SIZE=32 ollama run llama2
   ```

3. **Use GPU Acceleration**
   - [Configure GPU support](/docs/advanced/gpu-acceleration/) if you have a compatible GPU

4. **Optimize Context Length**
   - Reduce the context length if you don't need long-term memory

## API and Integration Issues

### CORS Errors

**Issue**: Getting CORS errors when calling the API from a web application.

**Solutions**:

1. **Use a CORS Proxy**
   - Set up a simple proxy server that adds CORS headers

2. **Set Up a Reverse Proxy**
   - Configure Nginx or Apache as a reverse proxy with CORS headers
   ```nginx
   # Example Nginx configuration
   server {
       listen 8080;
       location / {
           proxy_pass http://localhost:11434;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           
           # CORS headers
           add_header 'Access-Control-Allow-Origin' '*';
           add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
           add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
       }
   }
   ```

### Connection Timeouts

**Issue**: API requests time out.

**Solutions**:

1. **Increase Timeout Settings**
   - Adjust the timeout settings in your client code

2. **Check Server Load**
   - Verify that your server has enough resources

3. **Use Streaming Responses**
   - For long-running requests, use streaming responses with `stream: true`

## Platform-Specific Issues

### Linux: CUDA/GPU Issues

**Issue**: Unable to use GPU acceleration on Linux.

**Solutions**:

1. **Check CUDA Installation**
   ```bash
   nvidia-smi
   nvcc --version
   ```

2. **Install CUDA Toolkit**
   ```bash
   # For Ubuntu
   sudo apt install nvidia-cuda-toolkit
   ```

3. **Set Environment Variables**
   ```bash
   export OLLAMA_CUDA_VISIBLE_DEVICES=0
   ```

### macOS: High CPU Usage

**Issue**: Ollama causes high CPU usage on macOS.

**Solutions**:

1. **Limit Models**
   - Use smaller models (7B or less)
   - Use more aggressive quantization (q4_0)

2. **Check Activity Monitor**
   - Monitor CPU and memory usage
   - Close unnecessary applications

3. **Ensure MPS Acceleration**
   - For Apple Silicon Macs, ensure Metal Performance Shaders (MPS) is being used

### Windows (WSL): Path Issues

**Issue**: File path issues when using Ollama in WSL.

**Solutions**:

1. **Use Linux Paths**
   - Windows paths like `C:\folder` don't work in WSL
   - Use WSL paths `/mnt/c/folder` instead

2. **Copy Files to WSL Filesystem**
   - For better performance, copy files to the WSL filesystem

## Debugging Techniques

### Enable Debug Logging

To get more information about what's happening:

```bash
OLLAMA_DEBUG=1 ollama serve
```

### Check Model and Library Path

Verify where Ollama is storing models and libraries:

```bash
echo $OLLAMA_MODELS # Default: ~/.ollama/models
echo $OLLAMA_LIBRARIES # Default: ~/.ollama/libraries
```

### Network Connectivity Test

Test if you can connect to the Ollama API:

```bash
curl http://localhost:11434/api/tags
```

## Getting Help

If you're still experiencing issues after trying these solutions:

1. **Check GitHub Issues**
   - Search for similar issues on the [Ollama GitHub repository](https://github.com/ollama/ollama/issues)

2. **Join the Community**
   - Ask for help on [Discord](https://discord.gg/ollama)

3. **File a Bug Report**
   - If you've found a bug, [report it on GitHub](https://github.com/ollama/ollama/issues/new)

4. **Include Information**
   - When seeking help, include:
     - Ollama version (`ollama --version`)
     - Operating system and version
     - Error messages (copy exact text)
     - Steps to reproduce the issue
