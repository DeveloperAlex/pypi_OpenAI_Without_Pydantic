# Frequently Asked Questions (FAQ)

## General Questions

### What is this package for?

This package provides a simple way to call OpenAI's API without requiring Pydantic or the official OpenAI SDK. It's perfect for environments where Pydantic is unavailable or when you want minimal dependencies.

### Why avoid Pydantic?

The official OpenAI Python SDK has a hard dependency on Pydantic, which may be unavailable in some restricted environments, legacy systems, or when you want to minimize dependencies. This package solves that by using direct HTTP requests.

### What does this package depend on?

Only `requests` library (for HTTP calls). That's it! No Pydantic, no OpenAI SDK.

## Installation

### How do I install this package?

```bash
pip install developeralex-openai-without-pydantic
```

### Can I use this in a virtual environment?

Yes! In fact, it's recommended:

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install developeralex-openai-without-pydantic
```

### Does it work on Windows?

Yes! This package is fully cross-platform and works on Windows, Linux, and macOS.

## Usage

### How do I get an OpenAI API key?

1. Go to https://platform.openai.com/
2. Sign up or log in
3. Navigate to API Keys section
4. Create a new API key
5. Set it as an environment variable: `OPENAI_API_KEY=your-key-here`

### What models are supported?

All OpenAI chat models including:
- `gpt-4o` 
- `gpt-4o-mini` (default)
- `gpt-4`
- `gpt-4-turbo`
- `gpt-3.5-turbo`

### How do I use a different model?

```python
from openai_wrapper import ask_ai_question

response = ask_ai_question(
    input_text="Your context",
    question_asked="Your question?",
    model="gpt-4o"  # Specify model here
)
```

### Can I control the response randomness?

Yes! Use the `temperature` parameter (0.0 = deterministic, 2.0 = very random):

```python
response = ask_ai_question(
    input_text="Your context",
    question_asked="Your question?",
    temperature=0.3  # More factual, less creative
)
```

### How do I limit response length?

Use the `max_tokens` parameter:

```python
response = ask_ai_question(
    input_text="Your context",
    question_asked="Your question?",
    max_tokens=100  # Limit to ~100 tokens
)
```

## Errors and Troubleshooting

### "ModuleNotFoundError: No module named 'openai_wrapper'"

If developing locally:
```bash
pip install -e .
```

If using from PyPI:
```bash
pip install developeralex-openai-without-pydantic
```

### "ValueError: OpenAI API key must be provided"

Set your API key as an environment variable:

```bash
# Linux/macOS
export OPENAI_API_KEY='your-key-here'

# Windows CMD
set OPENAI_API_KEY=your-key-here

# Windows PowerShell
$env:OPENAI_API_KEY='your-key-here'
```

Or pass it directly:
```python
response = ask_ai_question(
    input_text="...",
    question_asked="...",
    api_key="your-key-here"
)
```

### "Request timed out"

Increase the timeout:

```python
response = ask_ai_question(
    input_text="...",
    question_asked="...",
    timeout=60  # 60 seconds instead of default 30
)
```

### "Rate limit exceeded"

You've hit OpenAI's rate limits. Solutions:
- Wait a few seconds between requests
- Upgrade your OpenAI plan
- Implement rate limiting in your code

### "Invalid API key"

- Double-check your API key
- Ensure there are no extra spaces or quotes
- Generate a new key from OpenAI dashboard

## Performance and Costs

### How much does it cost?

Costs depend on:
- Model used (GPT-4 is more expensive than GPT-4o-mini)
- Number of tokens processed (input + output)
- See OpenAI pricing: https://openai.com/pricing

### How can I reduce costs?

- Use `gpt-4o-mini` instead of `gpt-4`
- Set `max_tokens` to limit response length
- Keep `input_text` concise
- Use lower `temperature` for shorter, more direct responses

### Is it slower than the official SDK?

No significant difference. Both make the same HTTP requests to OpenAI's API.

## Development

### How do I contribute?

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### How do I run tests?

```bash
pytest tests/ -v
```

Or use the convenience script:
```bash
./run_tests.sh
```

### Where can I report bugs?

Open an issue on GitHub: https://github.com/DeveloperAlex/pypi_OpenAI_Without_Pydantic/issues

## Security

### Is it safe to use?

Yes! The package:
- Uses HTTPS for all API calls
- Doesn't store or log your API key
- Has no telemetry or tracking
- Is open-source (you can audit the code)

### How should I store my API key?

Best practices:
- ✅ Use environment variables
- ✅ Use `.env` files (add to `.gitignore`)
- ✅ Use secret management services in production
- ❌ Never hard-code keys in source code
- ❌ Never commit keys to version control

See [SECURITY.md](SECURITY.md) for more details.

### Can I use this in production?

Yes! The package is stable and includes:
- Comprehensive error handling
- Type hints
- Unit tests
- CI/CD pipelines

## Comparison

### vs Official OpenAI SDK?

| Feature | This Package | Official SDK |
|---------|-------------|--------------|
| Dependencies | `requests` only | Pydantic, many others |
| Size | Minimal | Larger |
| Features | Core API calls | Full SDK features |
| Streaming | Not yet | Yes |
| Async | Not yet | Yes |

### When should I use the official SDK?

Use the official SDK if you:
- Need streaming responses
- Want async/await support
- Need advanced features
- Don't mind the Pydantic dependency

### When should I use this package?

Use this package if you:
- Can't or don't want to use Pydantic
- Want minimal dependencies
- Only need basic API calls
- Prefer simplicity

## Still have questions?

- 📖 Check the [README](README.md)
- 💡 See [examples/](examples/)
- 🐛 Open an [issue](https://github.com/DeveloperAlex/pypi_OpenAI_Without_Pydantic/issues)
- 🤝 Read [CONTRIBUTING.md](CONTRIBUTING.md)
