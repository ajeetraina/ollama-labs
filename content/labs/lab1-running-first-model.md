---
title: "Lab 1: Running Your First Model"
weight: 1
---

# Lab 1: Running Your First Model with Ollama

In this lab, you'll learn how to run your first language model using Ollama and interact with it through the command line.

## Prerequisites

- Ollama installed on your system ([Installation Guide](/docs/getting-started/installation/))
- Terminal or command prompt access
- At least 5GB of free disk space

## Lab Objectives

By the end of this lab, you will:

1. Download and run a language model
2. Understand the basic Ollama commands
3. Interact with the model through the command line
4. Use system prompts to customize model behavior

## Step 1: Verify Ollama Installation

Before we begin, let's make sure Ollama is correctly installed. Open your terminal and run:

```bash
ollama --version
```

You should see the version number of your Ollama installation. If you get a "command not found" error, please follow the [installation guide](/docs/getting-started/installation/) for your platform.

## Step 2: List Available Models

Let's see what models are available in the Ollama library:

```bash
ollama list
```

If you've just installed Ollama, this command will likely return an empty list since you haven't downloaded any models yet.

## Step 3: Download Your First Model

Let's download the Llama 2 model, which is a good starting point for most users:

```bash
ollama pull llama2
```

This will download the Llama 2 model, which is approximately 3.8GB. The download might take a few minutes depending on your internet connection.

## Step 4: Run the Model

Now let's start a chat session with the model:

```bash
ollama run llama2
```

You'll see a welcome message from the model, and the cursor will be waiting for your input.

## Step 5: Interact with the Model

Try asking the model some questions or giving it tasks. Here are some examples:

- "Explain the concept of machine learning in simple terms."
- "Write a short poem about technology."
- "What are three ways to improve productivity?"
- "Create a simple Python function that calculates the factorial of a number."

After each response, you'll be prompted to enter a new message. To exit the chat session, press `Ctrl+C` or type `/exit`.

## Step 6: Use System Prompts

System prompts allow you to define the behavior and context for the model. Let's try using a system prompt:

```bash
ollama run llama2 "You are a helpful math tutor. Explain concepts clearly and provide step-by-step solutions."
```

Now the model will behave as a math tutor. Try asking it math-related questions like:

- "Explain the Pythagorean theorem."
- "How do I solve quadratic equations?"
- "What's the derivative of x²?"

## Step 7: Customize Model Parameters

You can customize various model parameters using environment variables. For example, to adjust the temperature (randomness) of the responses:

```bash
OLLAMA_TEMPERATURE=0.5 ollama run llama2
```

A lower temperature (like 0.2) will make responses more deterministic and focused, while a higher temperature (like 0.8) will make them more creative and varied.

## Step 8: Explore Different Models

Ollama supports many different models. Let's try a few more:

```bash
# Pull and run the Mistral model
ollama pull mistral
ollama run mistral

# Pull and run a smaller model for faster responses
ollama pull tinyllama
ollama run tinyllama
```

Compare the responses from different models to see how they perform on the same prompts.

## Conclusion

Congratulations! You've successfully run your first language model with Ollama. You've learned how to:

- Download and run models
- Interact with models through the command line
- Use system prompts to customize behavior
- Adjust model parameters
- Explore different models

## Next Steps

Now that you're familiar with the basics of Ollama, you can proceed to [Lab 2: Model Comparison and Selection](/labs/lab2-model-comparison/) to learn how to choose the right model for your specific use case.

## Troubleshooting

### Common Issues

- **Error: unable to connect to server**: Make sure the Ollama service is running. On Linux and macOS, you can start it with `ollama serve`.
- **Out of memory error**: If you're running on a system with limited RAM, try using a smaller model like `tinyllama` or `orca-mini`.
- **Slow responses**: Model response times depend on your hardware. CPUs will be much slower than systems with GPUs.

### Getting Help

If you encounter any issues not covered here, check the [Troubleshooting](/docs/troubleshooting/) section or join the [Ollama Discord server](https://discord.gg/ollama) for community support.