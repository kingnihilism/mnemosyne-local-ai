# Mnemosyne

**Mnemosyne** is a private, locally operated AI research assistant for Windows. It combines local language-model inference through Ollama with a PySide6 desktop interface, persistent SQLite storage, selective long-term memory, multiple named conversations, and document-grounded answers with visible source references.

> Mnemosyne does not train or provide its own language model. It supplies the desktop application, memory, retrieval, conversation-management, document-handling, and testing layers around a locally installed Ollama model.

## Current features

- Multiple persistent conversations with rename and delete controls
- User-managed long-term memories that can be enabled, edited, or removed
- Lightweight lexical memory retrieval without embeddings or cloud services
- Local document library for Markdown and text files
- Document excerpt retrieval with source IDs and character offsets
- SQLite persistence across application restarts
- Local Ollama inference through the loopback interface
- Visible memory and document traces for each response
- Bounded conversation context to control prompt size
- Markdown rendering for assistant responses
- Dark desktop interface built with PySide6

## Architecture

```text
PySide6 desktop interface
        |
        +-- SQLite
        |     +-- conversations and messages
        |     +-- user-managed memories
        |     +-- imported-document index
        |
        +-- Local retrieval
        |     +-- weighted lexical memory matching
        |     +-- document excerpt ranking
        |
        +-- Ollama local API
              +-- installed local language model
```

All application data is created inside the local `data/` directory. That directory is excluded from Git so conversations, memories, imported documents, and the SQLite database are not committed.

## Technology

- Python
- PySide6
- SQLite and SQLite FTS5
- Ollama local API
- Requests

## Requirements

- Windows 10 or later
- Python 3.10 or later
- [Ollama](https://ollama.com/) installed and running
- A locally installed Ollama model

The application currently expects an Ollama model named `mnemosyne`. You may either create an Ollama alias with that name or change the `MODEL` constant near the top of `mnemosyne.py` to the name of an installed model.

## Installation

Clone the repository and enter its directory:

```powershell
git clone https://github.com/kingnihilism/mnemosyne-local-ai.git
cd mnemosyne-local-ai
```

Create and activate a virtual environment:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the Python dependencies:

```powershell
pip install -r requirements.txt
```

Confirm that Ollama is running and that your selected model is available:

```powershell
ollama list
```

Run the application:

```powershell
python mnemosyne.py
```

The database and document-storage directories are created automatically on first launch.

## Manual verification

The current build has been checked with the following workflow:

1. Create and rename multiple conversations.
2. Save a memory, restart the application, and confirm persistence.
3. Ask a related question and confirm that the relevant memory ID appears in the trace.
4. Ask an unrelated question and confirm that unrelated memories are not injected.
5. Import a fictional `.txt` or `.md` document.
6. Ask a document-specific question and confirm that the response cites the displayed document ID.
7. Challenge an incorrect interpretation and confirm that the assistant can distinguish retrieved evidence from its own inference.
8. Delete a conversation, memory, or document and confirm that it is removed locally.

## Privacy

Mnemosyne is designed for local use:

- Conversations, memories, and imported documents are stored in a local SQLite database.
- Imported document copies remain in the local `data/documents/` directory.
- Model requests are sent to Ollama through `127.0.0.1`.
- The repository excludes databases, imported documents, logs, virtual environments, and environment files.

Users should still review files before committing and should never remove the privacy rules from `.gitignore` when working with real personal data.

## Current limitations

- Retrieval is lexical and does not use semantic embeddings.
- Document import currently supports only `.txt` and `.md` files.
- Document excerpts are ranked with straightforward keyword counts.
- The interface is designed for one local user.
- Automated tests and packaged Windows releases are not yet included.
- Ollama installation and model selection are handled outside the application.

## Roadmap

- Automated tests for storage, retrieval, and deletion behavior
- JSON backup and restore
- Improved document chunking and citation display
- Configurable Ollama model selection
- Safer database export tools
- Packaged Windows release
- Additional accessibility and interface refinements

## Project status

Mnemosyne is an active personal learning project. The current public source represents version **0.5A** and focuses on local persistence, transparent retrieval, document-grounded responses, and privacy-conscious desktop design.
