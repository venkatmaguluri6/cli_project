# MCP Chat

MCP Chat is a command-line interface application that enables interactive chat capabilities with AI models through the Anthropic API. The application supports document retrieval, command-based prompts, and extensible tool integrations via the MCP (Model Context Protocol) architecture.

## Prerequisites

- Python 3.11 or 3.12 (recommended — Python 3.13+ lacks pre-built wheels for some dependencies)
- Anthropic API Key

## Setup

### Step 1: Configure the environment variables

Create or edit the `.env` file in the project root and set the following:

```
ANTHROPIC_API_KEY=""  # Enter your Anthropic API secret key
```

To get your API key, visit [console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys).

### Step 2: Install dependencies

#### Option 1: Setup with uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a fast Python package installer and resolver.

1. Install uv:

**Windows (PowerShell):**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation, add uv to your PATH for the current session:
```powershell
$env:Path = "C:\Users\<YourUsername>\.local\bin;$env:Path"
```

To make this permanent:
```powershell
[Environment]::SetEnvironmentVariable("Path", "C:\Users\<YourUsername>\.local\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
```

**macOS/Linux:**
```bash
pip install uv
```

2. Create a virtual environment with Python 3.12:

```bash
uv venv --python 3.12
```

If Python 3.12 is not installed, uv can fetch it automatically:
```bash
uv python install 3.12
uv venv --python 3.12
```

3. Activate the virtual environment:

```bash
# macOS/Linux:
source .venv/bin/activate

# Windows (PowerShell):
.venv\Scripts\activate
```

4. Install dependencies using the public PyPI index:

```bash
# Windows (PowerShell) - set index to public PyPI if behind a corporate proxy:
$env:UV_INDEX_URL = "https://pypi.org/simple/"

uv pip install -e .
```

5. Run the project:

```bash
uv run main.py
```

> **Note for Windows users:** If you see a `401 Unauthorized` error when installing packages, your environment may be routing through a corporate package registry. Set the index URL as shown above to use public PyPI directly.

#### Option 2: Setup without uv

1. Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

2. Install dependencies:

```bash
pip install anthropic python-dotenv prompt-toolkit "mcp[cli]==1.8.0"
```

3. Run the project:

```bash
python main.py
```

## Usage

### Basic Interaction

Simply type your message and press Enter to chat with the model.

### Document Retrieval

Use the `@` symbol followed by a document ID to include document content in your query:

```
> Tell me about @deposition.md
```

### Commands

Use the `/` prefix to execute commands defined in the MCP server:

```
> /summarize deposition.md
```

Commands will auto-complete when you press Tab.

## Troubleshooting

### `uv` not recognized after installation (Windows)
Run the following in PowerShell to add it to your PATH, then reopen your terminal:
```powershell
$env:Path = "C:\Users\<YourUsername>\.local\bin;$env:Path"
```

### `401 Unauthorized` when installing packages
Your system may be routing through a corporate Artifactory registry. Override it with:
```powershell
$env:UV_INDEX_URL = "https://pypi.org/simple/"
```

### Build errors (`link.exe not found` / Rust compilation errors)
This typically happens when using Python 3.13+, which lacks pre-built wheels for packages like `pydantic-core` and `jiter`. Switch to Python 3.12:
```bash
uv venv --python 3.12
```

### `McpError: Connection closed`
The MCP server process failed to start. Check that:
- The server script path in `mcp_client.py` is correct
- The server runs without errors on its own: `python mcp_server.py`
- The `command` in `StdioServerParameters` points to a valid executable

## Development

### Adding New Documents

Edit the `mcp_server.py` file to add new documents to the `docs` dictionary.

### Implementing MCP Features

To fully implement the MCP features:

1. Complete the TODOs in `mcp_server.py`
2. Implement the missing functionality in `mcp_client.py`

### Linting and Typing Check

There are no lint or type checks implemented.
