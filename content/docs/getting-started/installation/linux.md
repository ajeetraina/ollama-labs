---
title: "Linux Installation"
weight: 2
---

# Installing Ollama on Linux

This guide will walk you through the process of installing Ollama on Linux.

## System Requirements

- Linux with glibc 2.35 or newer (Ubuntu 22.04 or newer recommended)
- x86_64 architecture
- 4GB of RAM minimum (8GB or more recommended for larger models)
- At least 5GB of free disk space

## Installation

### Method 1: Using the Install Script

The easiest way to install Ollama on Linux is using the install script:

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### Method 2: Manual Installation

1. Download the latest Linux release from [GitHub](https://github.com/ollama/ollama/releases)
2. Extract the archive
3. Move the `ollama` binary to a directory in your PATH, e.g., `/usr/local/bin`

```bash
chmod +x ollama
sudo mv ollama /usr/local/bin/
```

## Starting Ollama

After installation, you can start Ollama with:

```bash
ollama serve
```

This will start the Ollama server in the foreground. If you want to run it in the background, you can use:

```bash
nohup ollama serve &
```

## Creating a Systemd Service (Optional)

To run Ollama as a service that starts automatically, you can create a systemd service file:

1. Create a service file at `/etc/systemd/system/ollama.service`:

```
[Unit]
Description=Ollama Service
After=network.target

[Service]
Type=simple
User=YOUR_USERNAME
ExecStart=/usr/local/bin/ollama serve
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

2. Replace `YOUR_USERNAME` with your username
3. Enable and start the service:

```bash
sudo systemctl enable ollama
sudo systemctl start ollama
```

## Verifying Installation

To verify that Ollama is installed correctly:

```bash
ollama --version
```

## Running Your First Model

To download and run the Llama 2 model:

```bash
ollama run llama2
```

## Next Steps

- Try running different models with `ollama run modelname`
- Check out our [Docker Integration Lab](/labs/lab5-docker-integration/) to learn how to use Ollama with Docker
- Learn about [GPU acceleration](/docs/advanced/gpu-acceleration/) for better performance