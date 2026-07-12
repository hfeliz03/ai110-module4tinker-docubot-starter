# DocuBot

## TF Reflection

This tinker asks students to compare three ways of answering documentation questions: naive LLM prompting, retrieval only, and retrieval augmented generation (RAG). As a Tech Fellow, the most important thing to reinforce is that the activity is about reasoning and tradeoffs, not just "making the bot sound good."

- Students should understand the difference between retrieval and generation. Retrieval finds evidence. Generation turns evidence into an answer. RAG combines both.
- A polished answer is not automatically a correct answer. Students should inspect whether the answer is grounded in the provided docs.
- The retrieval baseline matters. If retrieval is weak, RAG will still be weak because the model can only ground itself in what it receives.
- Refusals are a success condition in this activity. "I do not know based on the docs I have" is often better than a confident hallucination.
- Students may confuse these fictional docs with a real backend. Remind them the `docs/` folder is the corpus, and the project is intentionally lightweight.
- Prompting should be evaluated, not worshipped. Better prompts can help, but prompt changes do not replace missing evidence or bad retrieval.
- The evaluation script is intentionally simple. Its job is to help students compare iterations, not to act like a perfect benchmark.
- When students get stuck, guide them toward observable system behavior:
  What tokens are in the query?
  Which files were retrieved?
  Why did those files score well or poorly?
  Did the prompt tell the model to stay grounded?
- Encourage students to test edge cases, especially questions the docs cannot answer. Those cases reveal whether the system is safe and honest.
- Common debugging moves:
  print retrieved filenames
  inspect tokenization choices
  compare retrieval only output against RAG output
  try the same query across all three modes

Helpful coaching questions:

- "What evidence did your system use for that answer?"
- "If the docs do not say that, what should the bot do instead?"
- "Is the problem retrieval, prompting, or both?"
- "What changed after you modified scoring or snippet selection?"

## Project Overview

DocuBot is a small documentation assistant for a tinker activity about search, grounding, and LLM behavior. It supports three modes:

1. `Naive LLM mode`
   The full documentation corpus is sent to Gemini with no retrieval step.
2. `Retrieval only mode`
   The system returns the most relevant documents or snippets without calling an LLM.
3. `RAG mode`
   The system retrieves relevant context first, then asks Gemini to answer using only that context.

The `docs/` folder contains a fictional developer documentation corpus. These files are plain text inputs for experimentation. Students do not need to run a backend server or a real database.

## Setup

1. Install dependencies

```bash
pip install -r requirements.txt
```

2. Create an environment file

```bash
cp .env.example .env
```

3. Add your Gemini key if you want LLM features

```dotenv
GEMINI_API_KEY=your_api_key_here
```

If `GEMINI_API_KEY` is missing, retrieval only mode still works.

## Running DocuBot

Start the CLI:

```bash
python main.py
```

Modes:

- `1`: Naive LLM over the full docs
- `2`: Retrieval only
- `3`: RAG

You can either run the built-in sample queries from [dataset.py](/Users/vilma/Codepath/AI110Tinker3/ai110-module4tinker-docubot-starter/dataset.py) or type your own question.

## Running Evaluation

Use the lightweight retrieval benchmark:

```bash
python evaluation.py
```

This reports a simple hit rate based on whether the expected source file appeared in the retrieval results.

## Files Students Usually Edit

- [docubot.py](/Users/vilma/Codepath/AI110Tinker3/ai110-module4tinker-docubot-starter/docubot.py): document loading, indexing, scoring, retrieval, retrieval-only behavior
- [llm_client.py](/Users/vilma/Codepath/AI110Tinker3/ai110-module4tinker-docubot-starter/llm_client.py): Gemini prompts for naive and RAG modes
- [dataset.py](/Users/vilma/Codepath/AI110Tinker3/ai110-module4tinker-docubot-starter/dataset.py): sample questions and fallback corpus
- [evaluation.py](/Users/vilma/Codepath/AI110Tinker3/ai110-module4tinker-docubot-starter/evaluation.py): simple retrieval evaluation harness
- [model_card.md](/Users/vilma/Codepath/AI110Tinker3/ai110-module4tinker-docubot-starter/model_card.md): reflection template for system behavior, risks, and improvements

## Learning Goals

- Understand how simple retrieval can outperform unsupported generation
- See why grounding improves reliability
- Practice debugging a system by inspecting evidence, not just final answers
- Notice when a safe refusal is the correct behavior

## Requirements

- Python 3.9+
- `google-generativeai` and `python-dotenv`
- A Gemini API key only for modes `1` and `3`
