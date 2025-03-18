---
title: "Windows Installation"
weight: 3
---

# Installing Ollama on Windows

This guide will walk you through the process of installing Ollama on Windows.

## System Requirements

- Windows 10 or Windows 11
- 4GB of RAM minimum (8GB or more recommended for larger models)
- At least 5GB of free disk space
- Windows Subsystem for Linux (WSL) for native Windows support

## Installation via WSL (Windows Subsystem for Linux)

### Step 1: Install WSL

If you don't already have WSL installed, open PowerShell as Administrator and run:

```powershell
wsl --install
```

Restart your computer when prompted.

### Step 2: Install Ollama in WSL

1. Open your WSL distribution (Ubuntu by default)
2. Install Ollama using the install script:

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### Step 3: Start Ollama

In your WSL terminal, start the Ollama server:

```bash
ollama serve
```

## Native Windows Support

Native Windows support is available through the Windows installer.

1. Visit the [Ollama website](https://ollama.ai) and download the Windows installer
2. Run the installer and follow the prompts
3. Launch Ollama from the Start menu

## Using Ollama with Windows

Once installed, you can use Ollama from the command line (WSL or PowerShell, depending on installation method).

To run your first model:

```
ollama run llama2
```

## Next Steps

- Try running different models with `ollama run modelname`
- Check out our [Windows Specific Tips](/docs/advanced/windows-tips/) for optimizing Ollama on Windows
- Learn about [GUI clients](/docs/advanced/gui-clients/) for a more user-friendly experience