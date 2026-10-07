# Neo4j Graph RAG Demo

A small, local demo of Graph RAG -- grounding a language model's answers in facts
retrieved from a Neo4j knowledge graph, instead of letting it guess. Built as a
companion to the Grounded AI: Graph RAG module, showing the same mechanism as
the Colab notebook, with Neo4j as the backend instead of NetworkX.

**This is a demo only.** Nothing here is required coursework -- it's provided
for anyone who wants to see how the pipeline looks running locally, end to end.

## What's in the zip

Download and unzip `neo4j-demos.zip` from this repository. Inside:

- `graph_rag.py` -- the demo script
- `create_graph.cypher` -- the small knowledge graph it runs against (a handful
  of facts about two people, Bob and Alice, and the Mona Lisa)
- `pyproject.toml` / `uv.lock` -- the project's dependencies
- `.env.example` -- a template for your own credentials (copy this to `.env`
  and fill in real values -- never commit `.env` itself)
- `README.md` -- full setup instructions, step by step

## Quick start

1. Unzip the folder and open it in VS Code (or your editor of choice).
2. Install [uv](https://docs.astral.sh/uv/), if you don't already have it.
3. Install and start Neo4j locally, and load `create_graph.cypher` into it
   via Neo4j Browser (`http://localhost:7474/browser/`).
4. Get a free Groq API key at [console.groq.com](https://console.groq.com).
5. `cp .env.example .env`, then fill in your real Neo4j password and Groq key.
6. `uv sync`
7. `uv run graph_rag.py`

Full detail on each step -- including how to install Neo4j itself -- is in the
zip's own `README.md`.

## What you'll see

The script asks the same question two ways: once with no retrieval (the model
answering from memory alone), and once grounded in facts pulled live from the
Neo4j graph. The two answers are printed side by side, along with the facts
that were actually retrieved, so you can see exactly what the model was and
wasn't given before it answered.
