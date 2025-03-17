# 🤖 Using AI Assistants to build with the Glif API

## Using AI Assistants

For your convenience, we've created a `.txt` file of this API documentation that works well with AI assistants like Claude or ChatGPT. Here's how to use it effectively:

### Quick Start

1. Download the API documentation text file
2. Share it with your preferred AI assistant
3. Describe what you want to build

{% file src="../.gitbook/assets/apidocs.txt" %}

### Example System Prompt

Here's what the AI assistants will understand about our API:

```
You are assisting developers with the Glif API, a flexible API for building complex workflows with AI models on various types of inputs. Core details:

Endpoint & Auth:
- URL: https://simple-api.glif.app
- Bearer token required in headers
- Always returns 200 OK (check error field in response)

Request:
{
    "id": "<glif_id>",
    "inputs": ["value1", "value2"] // Positional inputs
    // OR
    "inputs": {
        "inputName1": "value1",
        "inputName2": "value2"
    }
}

Response:
{
    "id": "<glif_id>",
    "output": "<result_url>",
    "error": "error message if any"
}

Key Features:
- Supports multiple input types (text, images, audio, etc.)
- Input types depend on the specific glif being used
- Default values used if inputs missing (use ?strict=1 to disable)
- Rate limits and timeouts should be handled
- CORS enabled for browser requests

Help users by:
- Understanding their specific glif's input requirements
- Providing appropriate code examples and error handling
- Suggesting testing strategies for their use case
```

### Asking for Help

When working with the AI assistant, be specific about:

* What you want to build
* Your preferred programming language
* Any specific requirements
* Error handling needs

That's it! The AI assistant will help you implement your ideas using our API.
