---
title: "API Reference"
weight: 3
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

#### Response (streaming)

```json
{"model":"llama2","response":"The"}
{"response":" capital"}
{"response":" of"}
{"response":" France"}
{"response":" is"}
{"response":" Paris"}
{"response":"."}
{"done":true}
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

#### Response

The API streams status updates as the model is downloaded:

```json
{"status":"pulling manifest"}
{"status":"pulling 14be503076e1... 100% complete"}
{"status":"pulling 88eb8c2e37d1... 100% complete"}
{"status":"pulling 7cb599a18fab... 100% complete"}
{"status":"verifying sha256 digest"}
{"status":"writing manifest"}
{"status":"success"}
```

### POST /api/push

Push a model to a registry.

#### Request

```json
{
  "name": "username/modelname",
  "insecure": false
}
```

Parameters:

- `name` (string, required): Name of the model to push
- `insecure` (boolean, optional): Allow insecure connections (default: false)

#### Response

The API streams status updates as the model is uploaded.

### POST /api/create

Create a model from a Modelfile.

#### Request

```json
{
  "name": "my-model",
  "modelfile": "FROM llama2\nSYSTEM You are a helpful assistant"
}
```

Parameters:

- `name` (string, required): Name for the new model
- `modelfile` (string, required): Contents of the Modelfile
- `stream` (boolean, optional): Whether to stream the response (default: false)

#### Response

The API streams status updates as the model is created.

### POST /api/copy

Copy a model.

#### Request

```json
{
  "source": "llama2",
  "destination": "llama2-backup"
}
```

Parameters:

- `source` (string, required): Name of the model to copy from
- `destination` (string, required): Name of the model to copy to

#### Response

```json
{}
```

### POST /api/delete

Delete a model.

#### Request

```json
{
  "name": "llama2-backup"
}
```

Parameters:

- `name` (string, required): Name of the model to delete

#### Response

```json
{}
```

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

### JavaScript (Node.js) Example

```javascript
const fetch = require('node-fetch');

// Chat example
async function chatWithModel() {
    const response = await fetch('http://localhost:11434/api/chat', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            model: 'llama2',
            messages: [
                {role: 'user', content: 'What is the capital of France?'}
            ]
        })
    });
    
    return await response.json();
}

// Streaming chat example
async function streamChat() {
    const response = await fetch('http://localhost:11434/api/chat', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            model: 'llama2',
            messages: [
                {role: 'user', content: 'Write a short poem about programming.'}
            ],
            stream: true
        })
    });
    
    // Handle streaming response
    const reader = response.body.getReader();
    const decoder = new TextDecoder();
    
    while (true) {
        const {value, done} = await reader.read();
        if (done) break;
        
        const lines = decoder.decode(value).split('\n');
        for (const line of lines) {
            if (line.trim() === '') continue;
            
            try {
                const data = JSON.parse(line);
                if (data.message && data.message.content) {
                    process.stdout.write(data.message.content);
                }
            } catch (error) {
                console.error('Error parsing JSON:', error);
            }
        }
    }
}
```

## Next Steps

- Check out the [Labs section](/labs/) for hands-on examples using the Ollama API
- Learn about [embedding generation](/docs/advanced/embeddings/) for semantic search applications
- Explore [integration with other frameworks](/docs/advanced/integrations/) like LangChain and LlamaIndex