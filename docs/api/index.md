---
layout: default
title: API Reference
parent: Documentation
nav_order: 3
permalink: /docs/api/
---

# Ollama API Reference

Ollama provides a comprehensive HTTP API that allows you to integrate LLMs into your applications. This API reference documents the available endpoints, parameters, and response formats.

## API Base URL

The Ollama API is available at:

```
http://localhost:11434/api
```

## Authentication

By default, the Ollama API does not require authentication when accessed locally. For remote access, you should set up appropriate authentication mechanisms (e.g., reverse proxy with authentication, firewall rules, etc.).

## Common API Patterns

- All API requests use HTTP POST with JSON request bodies (except for the `GET /api/tags` endpoint)
- All API responses are in JSON format
- Many endpoints support streaming responses (with `stream: true` parameter)
- Error responses include an `error` field with a message

## Chat Endpoint

### POST /api/chat

Start a chat session with a model.

#### Request

```json
{
  "model": "llama2",
  "messages": [
    {
      "role": "user",
      "content": "Hello, how are you?"
    }
  ],
  "stream": true,
  "options": {
    "temperature": 0.7,
    "top_p": 0.9,
    "top_k": 40
  }
}
```

Parameters:

- `model` (string, required): Name of the model to use
- `messages` (array, required): List of messages in the conversation
  - `role` (string): Either "user" or "assistant"
  - `content` (string): The message content
- `stream` (boolean, optional): Whether to stream the response (default: false)
- `options` (object, optional): Additional parameters for generation
  - `temperature` (float): Controls randomness (0.0-1.0)
  - `top_p` (float): Controls diversity via nucleus sampling (0.0-1.0)
  - `top_k` (integer): Controls diversity by limiting to top K tokens
  - `num_predict` (integer): Maximum number of tokens to generate
  - `system` (string): System prompt to include

#### Response (non-streaming)

```json
{
  "model": "llama2",
  "message": {
    "role": "assistant",
    "content": "I'm doing well, thank you for asking! How can I help you today?"
  },
  "done": true
}
```

#### Response (streaming)

When streaming is enabled, the API sends a series of JSON objects, one per line:

```json
{"model":"llama2","message":{"role":"assistant","content":"I"}}
{"message":{"content":"'m"}}
{"message":{"content":" doing"}}
{"message":{"content":" well"}}
{"message":{"content":","}}
...
{"done":true}
```

## Generate Endpoint

### POST /api/generate

Generate a completion for a prompt.

#### Request

```json
{
  "model": "llama2",
  "prompt": "What is the capital of France?",
  "stream": true,
  "options": {
    "temperature": 0.7,
    "num_predict": 100
  }
}
```

Parameters:

- `model` (string, required): Name of the model to use
- `prompt` (string, required): The prompt to generate a response for
- `system` (string, optional): System prompt to include
- `template` (string, optional): Custom prompt template
- `context` (array of integers, optional): Previous context to include
- `stream` (boolean, optional): Whether to stream the response (default: false)
- `raw` (boolean, optional): Whether to return raw, unprocessed model output
- `options` (object, optional): Additional parameters for generation
  - See options under the chat endpoint

#### Response (non-streaming)

```json
{
  "model": "llama2",
  "response": "The capital of France is Paris.",
  "done": true
}
```

## Embeddings Endpoint

### POST /api/embeddings

Generate embeddings for a given input.

#### Request

```json
{
  "model": "llama2",
  "prompt": "The quick brown fox jumps over the lazy dog"
}
```

Parameters:

- `model` (string, required): Name of the model to use
- `prompt` (string, required): The text to generate an embedding for

#### Response

```json
{
  "embedding": [0.5, 0.2, 0.9, ...]
}
```

## Model Management Endpoints

### GET /api/tags

List available models.

#### Response

```json
{
  "models": [
    {
      "name": "llama2",
      "model": "llama2:7b",
      "modified_at": "2023-08-02T15:27:20.162385Z",
      "size": 3791730596,
      "digest": "..."
    },
    ...
  ]
}
```

### POST /api/pull

Download a model from the Ollama library.

#### Request

```json
{
  "name": "llama2",
  "insecure": false
}
```

Parameters:

- `name` (string, required): Name of the model to pull (e.g., "llama2:7b")
- `insecure` (boolean, optional): Allow insecure connections (default: false)

## Example API Usage

### Python Example

```python
import requests
import json

# Chat example
def chat_with_model():
    url = "http://localhost:11434/api/chat"
    payload = {
        "model": "llama2",
        "messages": [
            {"role": "user", "content": "What is the capital of France?"}
        ]
    }
    
    response = requests.post(url, json=payload)
    return response.json()

# Streaming chat example
def stream_chat():
    url = "http://localhost:11434/api/chat"
    payload = {
        "model": "llama2",
        "messages": [
            {"role": "user", "content": "Write a short poem about programming."}
        ],
        "stream": True
    }
    
    response = requests.post(url, json=payload, stream=True)
    
    for line in response.iter_lines():
        if line:
            json_response = json.loads(line)
            if 'message' in json_response and 'content' in json_response['message']:
                print(json_response['message']['content'], end='')

# List models
def list_models():
    url = "http://localhost:11434/api/tags"
    response = requests.get(url)
    return response.json()
```