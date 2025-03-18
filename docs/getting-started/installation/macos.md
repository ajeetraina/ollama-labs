---
layout: default
title: macOS Installation
parent: Installation Guides
grand_parent: Getting Started
nav_order: 1
permalink: /docs/getting-started/installation/macos/
---

# Installing Ollama on macOS

This guide will walk you through the process of installing Ollama on macOS.

## System Requirements

- macOS 12 (Monterey) or newer
- 4GB of RAM minimum (8GB or more recommended for larger models)
- At least 5GB of free disk space

## Installation Steps

### Method 1: Using the Installer

1. Visit the [Ollama website](https://ollama.ai) and download the macOS installer
2. Open the downloaded .dmg file
3. Drag the Ollama icon to your Applications folder
4. Open Ollama from your Applications folder

### Method 2: Using Homebrew

If you have Homebrew installed, you can install Ollama with:

```bash
brew install ollama
```

To start the Ollama service:

```bash
brew services start ollama
```

## Verifying Installation

To verify that Ollama is installed correctly, open Terminal and run:

```bash
ollama --version
```

You should see the version number of your Ollama installation.

## Running Your First Model

To download and run the Llama 2 model:

```bash
ollama run llama2
```

This will download the model (if not already downloaded) and start a chat session.

## Next Steps

- Try running different models with `ollama run modelname`
- Check out our [Python Integration Lab](/labs/lab3-python-integration/) to learn how to use Ollama with Python
- Learn about the [Ollama API](/docs/api/) to integrate Ollama with your applications