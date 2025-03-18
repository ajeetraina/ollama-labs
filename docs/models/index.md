---
layout: default
title: Models
parent: Documentation
nav_order: 2
permalink: /docs/models/
---

# Ollama Model Library

Ollama supports a wide variety of open-source large language models (LLMs). This section provides information about the most popular models available for use with Ollama, their characteristics, and recommendations for different use cases.

## Available Models

Here's an overview of the main models available for Ollama:

| Model | Size | Strengths | Ideal Use Cases |
|-------|------|-----------|----------------|
| [Llama 2](#llama-2) | 7B to 70B | Well-balanced performance | General purpose, chat, coding |
| [Mistral](#mistral) | 7B | Strong at reasoning | Creative writing, complex reasoning |
| [Gemma](#gemma) | 2B, 7B | Google's lightweight model | Simple tasks, embedded systems |
| [Phi](#phi) | 2B | Microsoft's small but capable model | Education, lightweight applications |
| [Orca](#orca) | 3B, 7B | Strong instruction following | Following detailed instructions |
| [Vicuna](#vicuna) | 7B, 13B | Strong conversational abilities | Chatbots, dialogue systems |
| [CodeLlama](#codellama) | 7B to 34B | Specialized for code generation | Programming, code completion |
| [Falcon](#falcon) | 7B, 40B | Multilingual capabilities | International applications |
| [Stable LM](#stable-lm) | 3B, 7B | Low resource requirements | Edge devices, local applications |

## Choosing the Right Model

When selecting a model, consider the following factors:

1. **Hardware Requirements**: Larger models (like Llama 2 70B) require more RAM and GPU memory
2. **Speed**: Smaller models run faster, especially on CPU-only systems
3. **Capability**: Larger models generally perform better on complex tasks
4. **Specialization**: Some models are fine-tuned for specific domains like coding

## Detailed Model Information

### Llama 2

Llama 2 is a family of models developed by Meta AI, released in July 2023. It's one of the most popular open-source models and serves as a foundation for many other fine-tuned models.

#### Variants

- **Llama 2 7B**: The smallest variant, good for CPU usage and limited RAM
- **Llama 2 13B**: A good balance between performance and resource requirements
- **Llama 2 70B**: The largest and most capable variant, requires significant GPU resources

#### Chat-Tuned Versions

The chat-tuned versions (e.g., `llama2-chat`) are fine-tuned specifically for conversational use cases and follow instructions better than the base models.

#### Usage

```bash
# Download and run Llama 2 (7B)
ollama run llama2

# Download and run Llama 2 chat-tuned (7B)
ollama run llama2-chat

# Download and run Llama 2 (13B)
ollama run llama2:13b
```

### Mistral

Mistral is a 7B parameter model released by Mistral AI in September 2023. Despite its relatively small size, it often outperforms larger models on many benchmarks.

#### Strengths

- Excellent reasoning capabilities
- Good instruction following
- Efficient architecture that performs well even at 7B parameters

#### Variants

- **Mistral 7B**: Base model
- **Mistral-Instruct**: Fine-tuned for instruction following

#### Usage

```bash
# Download and run Mistral base model
ollama run mistral

# Download and run Mistral Instruct
ollama run mistral-instruct
```