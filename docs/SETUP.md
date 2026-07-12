# Setup Guide

This document explains how to run the DocuBot tinker project. Unlike a full application stack, this activity only needs local Python dependencies and an optional Gemini API key.

## Requirements

- Python 3.9 or later
- `pip`
- A terminal
- Optional: `GEMINI_API_KEY` for LLM-backed modes

## 1. Install Dependencies

From the project root:

```bash
pip install -r requirements.txt
```

## 2. Configure Environment Variables

Create a `.env` file or export variables in your shell.

Required for LLM modes:

```dotenv
GEMINI_API_KEY=your_api_key_here
```

If `GEMINI_API_KEY` is not set, DocuBot still supports retrieval only mode.

## 3. Run the CLI

Start the activity with:

```bash
python main.py
```

You can then choose:

- Naive LLM mode
- Retrieval only mode
- RAG mode

## 4. Run Evaluation

To compare retrieval performance across sample questions:

```bash
python evaluation.py
```

This script checks whether the expected source files were retrieved for each query.

## 5. Troubleshooting

- If mode 1 or mode 3 is unavailable, check whether `GEMINI_API_KEY` is set.
- If retrieval results look wrong, inspect the query tokens and which filenames were returned.
- If the docs folder is missing, the project can fall back to the in-memory sample corpus in `dataset.py`.
- If `pip install` fails, verify your Python version and virtual environment.
