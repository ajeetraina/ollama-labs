---
layout: default
title: Lab 3 - Python Integration
parent: Labs
nav_order: 3
permalink: /labs/lab3-python-integration/
---

# Lab 3: Integrating Ollama with Python Applications

In this lab, you'll learn how to integrate Ollama with Python applications using the Ollama API and Python libraries. This allows you to build custom applications that leverage Ollama's capabilities.

## Prerequisites

- Ollama installed and working ([Lab 1](/labs/lab1-running-first-model/))
- Python 3.8 or later installed
- Basic Python knowledge
- At least one model downloaded (e.g., `llama2`)

## Lab Objectives

By the end of this lab, you will:

1. Understand how the Ollama API works
2. Use Python to communicate with Ollama
3. Create a simple question-answering application
4. Build a basic chat interface
5. Stream responses for better user experience

## Step 1: Verify Ollama is Running

Before we start, make sure Ollama is running. Open a terminal and run:

```bash
ollama serve
```

Leave this terminal window open and use a new one for the rest of the lab.

## Step 2: Set Up Python Environment

Create a new directory for this lab and set up a virtual environment:

```bash
mkdir ollama-python-lab
cd ollama-python-lab
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

Install the required Python packages:

```bash
pip install requests
```

## Step 3: Basic API Request

Let's start by creating a simple Python script to communicate with Ollama. Create a file named `basic_request.py` with the following content:

```python
import requests
import json

def generate_response(prompt, model="llama2"):
    url = "http://localhost:11434/api/generate"
    
    data = {
        "model": model,
        "prompt": prompt
    }
    
    response = requests.post(url, json=data)
    if response.status_code == 200:
        # The response is a stream of JSON objects, one per line
        response_text = ""
        for line in response.text.strip().split('\n'):
            try:
                response_json = json.loads(line)
                if 'response' in response_json:
                    response_text += response_json['response']
            except json.JSONDecodeError:
                pass
        return response_text
    else:
        return f"Error: {response.status_code} - {response.text}"

# Test the function
if __name__ == "__main__":
    prompt = "Explain what Ollama is in one paragraph."
    response = generate_response(prompt)
    print(f"Prompt: {prompt}")
    print(f"Response: {response}")
```

Run the script:

```bash
python basic_request.py
```

You should see a response from the model explaining what Ollama is.

## Step 4: Streaming Responses

For a better user experience, let's modify our script to stream the responses as they're generated. Create a file named `streaming_response.py`:

```python
import requests
import json
import sys

def stream_response(prompt, model="llama2"):
    url = "http://localhost:11434/api/generate"
    
    data = {
        "model": model,
        "prompt": prompt,
        "stream": True
    }
    
    response = requests.post(url, json=data, stream=True)
    if response.status_code == 200:
        for line in response.iter_lines():
            if line:
                try:
                    response_json = json.loads(line)
                    if 'response' in response_json:
                        sys.stdout.write(response_json['response'])
                        sys.stdout.flush()
                except json.JSONDecodeError:
                    pass
        print()  # Add a newline at the end
    else:
        print(f"Error: {response.status_code} - {response.text}")

# Test the function
if __name__ == "__main__":
    prompt = "Write a short poem about programming."
    print(f"Prompt: {prompt}")
    print(f"Response: ", end="")
    stream_response(prompt)
```

Run the script:

```bash
python streaming_response.py
```

You should see the response being generated character by character, simulating how the model generates text.

## Step 5: Simple Chat Application

Now let's create a simple chat application that maintains conversation history. Create a file named `chat_app.py`:

```python
import requests
import json
import sys

def chat_with_ollama(model="llama2"):
    url = "http://localhost:11434/api/chat"
    history = []
    
    print(f"Chat with {model} (type 'exit' to quit)")
    
    while True:
        user_input = input("\nYou: ")
        if user_input.lower() == 'exit':
            break
        
        data = {
            "model": model,
            "messages": history + [{"role": "user", "content": user_input}],
            "stream": True
        }
        
        print(f"\n{model}: ", end="")
        full_response = ""
        
        response = requests.post(url, json=data, stream=True)
        if response.status_code == 200:
            for line in response.iter_lines():
                if line:
                    try:
                        response_json = json.loads(line)
                        if 'message' in response_json and 'content' in response_json['message']:
                            content = response_json['message']['content']
                            sys.stdout.write(content)
                            sys.stdout.flush()
                            full_response += content
                    except json.JSONDecodeError:
                        pass
            print()  # Add a newline at the end
            
            # Add the exchange to the conversation history
            history.append({"role": "user", "content": user_input})
            history.append({"role": "assistant", "content": full_response})
        else:
            print(f"Error: {response.status_code} - {response.text}")

# Start the chat application
if __name__ == "__main__":
    chat_with_ollama()
```

Run the chat application:

```bash
python chat_app.py
```

You can now have a conversation with the model, and it will remember the context of your previous messages.

## Step 6: Advanced Application - Document Q&A

Let's build a more advanced application that answers questions about a given document. Create a file named `document_qa.py`:

```python
import requests
import json
import sys

def answer_question(document, question, model="llama2"):
    url = "http://localhost:11434/api/generate"
    
    # Construct a prompt that includes the document and the question
    prompt = f"""Document: {document}

Question: {question}

Answer the question based only on the information provided in the document above. If the answer cannot be found in the document, say "I don't have enough information to answer this question."

Answer: """
    
    data = {
        "model": model,
        "prompt": prompt,
        "stream": True
    }
    
    print("Thinking...", end="\r")
    response = requests.post(url, json=data, stream=True)
    if response.status_code == 200:
        print("Answer:   ")
        for line in response.iter_lines():
            if line:
                try:
                    response_json = json.loads(line)
                    if 'response' in response_json:
                        sys.stdout.write(response_json['response'])
                        sys.stdout.flush()
                except json.JSONDecodeError:
                    pass
        print()  # Add a newline at the end
    else:
        print(f"Error: {response.status_code} - {response.text}")

# Example document
sample_document = """
Ollama is an open-source project that allows users to run large language models (LLMs) locally on their own hardware. It was created to simplify the process of downloading, installing, and running these models without requiring specialized knowledge or equipment.

Ollama supports various models including Llama 2, Mistral, and others. It provides a command-line interface and an API that developers can use to integrate these models into their applications. The tool is designed to work on macOS, Linux, and Windows (via WSL).

The main advantages of Ollama include:
- Privacy: All processing happens locally, so your data never leaves your device
- Customization: Users can create and modify their own models using Modelfiles
- Simplicity: Easy to install and use with simple commands
- Integration: API support for building applications

Ollama was first released in July 2023 and has quickly gained popularity in the AI developer community.
"""

# Start the Q&A application
if __name__ == "__main__":
    print("Document Q&A System")
    print("------------------\n")
    print(f"Document:\n{sample_document}\n")
    
    while True:
        question = input("\nEnter your question (or 'exit' to quit): ")
        if question.lower() == 'exit':
            break
        
        answer_question(sample_document, question)
```

Run the Q&A application:

```bash
python document_qa.py
```

You can now ask questions about the provided document, and the model will answer based on the information in the document.

## Conclusion

Congratulations! You've learned how to integrate Ollama with Python applications, create a chat interface, stream responses, and build a document Q&A system.

With these skills, you can now build more complex applications that leverage Ollama's capabilities, such as:

- Custom chatbots for specific domains
- Document analysis tools
- Content generation applications
- Creative writing assistants

## Next Steps

Now that you know how to use Ollama with Python, you might want to explore:

- [Lab 4: Building a Chat Interface](/labs/lab4-chat-interface/) to create a web-based chat application
- [API Reference](/docs/api/) for more details on the Ollama API
- [Advanced Usage](/docs/advanced/) for more advanced techniques and configurations
