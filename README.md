# Website Summarizer

A Python tool that scrapes web content and generates AI-powered summaries using OpenAI's GPT models.

## Features

- Web scraping with proper headers to avoid blocking
- Content cleaning (removes scripts, styles, images, and input elements)
- AI-powered summarization using OpenAI's GPT-4o-mini
- Markdown-formatted output
- Support for news and announcement summaries

## Prerequisites

- Python 3.7+
- OpenAI API key

## Installation

1. Clone or download this repository
2. Install required dependencies:

```bash
pip install requests python-dotenv beautifulsoup4 openai
```

3. Create a `.env` file in the project root directory:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

## Usage

### Basic Usage

The script is currently configured to summarize CNN's homepage:

```bash
python main.py
```

### Programmatic Usage

You can use the `summarize()` function with any URL:

```python
from main import summarize

# Summarize any website
summary = summarize("https://example.com")
print(summary)
```

### Custom Website Analysis

You can also work with the `Website` class directly:

```python
from main import Website, messages_for
from openai import OpenAI

# Create a website object
website = Website("https://example.com")
print(f"Title: {website.title}")
print(f"Content preview: {website.text[:500]}...")

# Generate custom prompts
messages = messages_for(website)
```

## How It Works

1. **Web Scraping**: The `Website` class fetches web content using the `requests` library with browser-like headers
2. **Content Cleaning**: BeautifulSoup removes irrelevant elements (scripts, styles, images, inputs)
3. **Text Extraction**: Clean text content is extracted from the webpage
4. **AI Summarization**: OpenAI's GPT-4o-mini processes the content and generates a markdown summary
5. **Output**: Returns a concise summary including news and announcements when present

## Configuration

### System Prompt

The AI behavior can be customized by modifying the `system_prompt` variable:

```python
system_prompt = "You are an assistant that analyzes the contents of a website \
and provides a short summary, ignoring text that might be navigation related. \
Respond in markdown."
```

### Headers

Browser headers can be adjusted in the `headers` dictionary to avoid being blocked by websites.

### OpenAI Model

Change the model by modifying the `model` parameter in the `summarize()` function:

```python
response = openai.chat.completions.create(
    model="gpt-4o-mini",  # or "gpt-4", "gpt-3.5-turbo", etc.
    messages=messages_for(website)
)
```

## Environment Variables

| Variable         | Description         | Required |
| ---------------- | ------------------- | -------- |
| `OPENAI_API_KEY` | Your OpenAI API key | Yes      |

## Dependencies

- `requests` - HTTP library for web scraping
- `python-dotenv` - Environment variable management
- `beautifulsoup4` - HTML parsing and content extraction
- `openai` - OpenAI API client

## Error Handling

The script includes basic error handling for:

- Missing website titles
- Content extraction from web pages

For production use, consider adding more robust error handling for:

- Network timeouts
- Invalid URLs
- API rate limits
- Missing environment variables

## Example Output

When run against a news website, the tool generates markdown summaries like:

```markdown
# CNN Homepage Summary

## Latest News

- Breaking news about current events
- Political developments and analysis
- International news coverage

## Key Announcements

- Weather alerts and updates
- Sports scores and highlights
- Business and market news
```

## License

This project is provided as-is for educational and personal use.
