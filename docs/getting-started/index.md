---
layout: default
title: Getting Started
parent: Documentation
nav_order: 1
has_children: true
permalink: /docs/getting-started/
---

# Getting Started with Ollama

This guide will help you install Ollama and run your first language model locally.

## Installation

Ollama is available for macOS, Linux, and Windows. Choose your platform below to get started:

- [macOS Installation Guide](./installation/macos/)
- [Linux Installation Guide](./installation/linux/)
- [Windows Installation Guide](./installation/windows/)

## Running Your First Model

Once you have Ollama installed, you can run your first model with a simple command:

```bash
ollama run llama2
```

This will download the Llama 2 model (if you don't already have it) and start a chat session.

## Basic Commands

Here are some basic commands to get you started:

- `ollama list` - List all available models
- `ollama pull modelname` - Download a model
- `ollama run modelname` - Run a model
- `ollama rm modelname` - Remove a model

## Next Steps

Check out our [Labs section](/labs/) for hands-on exercises that will help you build practical applications with Ollama.