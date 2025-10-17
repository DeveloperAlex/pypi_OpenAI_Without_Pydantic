# OpenAI Wrapper - No Pydantic

A simple Python package for calling OpenAI's API without using Pydantic or the OpenAI SDK. Perfect for environments where Pydantic is unavailable.

## Features

- ✅ Simple, single-function API: `ask_ai_question(input_text, question_asked)`
- ✅ **Zero Pydantic dependency** - uses direct HTTP requests
- ✅ **No OpenAI SDK required** - bypasses all SDK dependencies
- ✅ Support for all OpenAI chat models
- ✅ Customizable parameters (temperature, max_tokens, model)
- ✅ Type hints for better IDE support
- ✅ Comprehensive error handling
- ✅ Only requires `requests` library

## Installation

1. Install the dependencies:
```bash
pip install -r requirements.txt
```

This only installs `requests` - no Pydantic, no OpenAI SDK!

2. Set your OpenAI API key:
```bash
export OPENAI_API_KEY='your-api-key-here'
```

## Usage

### Basic Example

```python
from openai_wrapper import ask_ai_question

input_text = """
Python is a high-level programming language created by Guido van Rossum 
and first released in 1991.
"""

question = "Who created Python?"

response = ask_ai_question(
    input_text=input_text,
    question_asked=question
)

print(response)  # Output: Python was created by Guido van Rossum.
```

### Advanced Usage

```python
from openai_wrapper import ask_ai_question

# Use custom parameters
response = ask_ai_question(
    input_text="Your context here...",
    question_asked="Your question here...",
    model="gpt-4o",              # Specify model
    temperature=0.3,              # Lower for more factual responses
    max_tokens=200,               # Limit response length
    api_key="your-key-here"       # Or use environment variable
)
```

## Function Parameters

- **input_text** (str, required): The context/text to use for answering the question
- **question_asked** (str, required): The question to ask about the input_text
- **api_key** (str, optional): OpenAI API key (defaults to OPENAI_API_KEY env var)
- **model** (str, optional): The OpenAI model to use (default: "gpt-4o-mini")
- **temperature** (float, optional): Sampling temperature 0-2 (default: 0.7)
- **max_tokens** (int, optional): Maximum tokens in response (default: None)

## Running the Examples

Run the included example file:
```bash
python main.py
```

## Project Structure

```
.
├── openai_wrapper/
│   ├── __init__.py       # Package initialization
│   └── client.py         # Main implementation
├── main.py               # Usage examples
├── requirements.txt      # Dependencies
└── README.md            # This file
```

## Error Handling

The package includes comprehensive error handling:

```python
try:
    response = ask_ai_question(input_text="...", question_asked="...")
except ValueError as e:
    print(f"Configuration error: {e}")
except Exception as e:
    print(f"API error: {e}")
```

## Why No Pydantic?

This package is specifically designed for environments where Pydantic is not available. 

**The Problem:** The official OpenAI Python SDK (`openai` package) has a hard dependency on `pydantic` and `pydantic-core`, which may be unavailable in some environments.

**Our Solution:** We bypass the OpenAI SDK entirely and call the OpenAI API directly using HTTP requests via the `requests` library. This eliminates all Pydantic dependencies while maintaining full functionality.

## Dependencies

- `requests>=2.25.0` - For making HTTP requests to OpenAI API
- **That's it!** No Pydantic, no OpenAI SDK, no hidden dependencies.

## License

MIT
