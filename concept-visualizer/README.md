# Concept Visualizer

A single-file agentic AI app that turns any academic topic into a sequential set of **hand-drawn, Excalidraw-style diagrams** — generated entirely in the browser using your own Gemini API key.

## What it does

1. **Verify** — An AI agent checks whether the entered topic is academic and estimates its complexity.
2. **Plan** — A planner agent generates an ordered sequence of visualization prompts (3–9 steps depending on complexity).
3. **Visualize** — A visualizer agent calls Gemini's image generation model for each prompt, rendering diagrams one by one in a clean card layout.

Inspired by the multi-agent diagram pipeline described in [arXiv 2601.23265](https://arxiv.org/abs/2601.23265) and the [PaperBanana](https://github.com/llmsresearch/paperbanana) implementation.

## Stack

| Layer | Tech |
|---|---|
| UI | Vanilla HTML + Tailwind CSS CDN |
| Fonts | Inter (Google Fonts) |
| AI (text) | `gemini-2.0-flash` — verify + plan |
| AI (images) | `gemini-2.0-flash-preview-image-generation` — visualize |
| Runtime | Browser only — zero backend, zero build step |

## Deploy

Because it's a single `index.html` file, you can deploy it anywhere static files are served:

```bash
# Netlify Drop — drag the folder to netlify.com/drop

# GitHub Pages
git subtree push --prefix concept-visualizer origin gh-pages

# Python quick-serve locally
cd concept-visualizer && python3 -m http.server 8080
```

## Usage

1. Get a free Gemini API key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. Open `index.html` in a browser (or deploy it)
3. Paste your API key — it stays in your browser's `sessionStorage` only
4. Type any academic topic, press **Enter** or click **Generate Visualizations**
5. Watch the pipeline run: Verify → Plan → Generate images
6. Hover over any image and click **Save** to download it

## Examples of good topics

- Transformer attention mechanism
- CRISPR gene editing
- Fourier transforms
- How black holes form
- Gradient descent in neural networks
- The citric acid cycle
- Public-key cryptography
- Plate tectonics

## Architecture

```
┌─────────────────────────────────────────────────────┐
│  User: "Transformer attention mechanism"            │
└──────────────────────┬──────────────────────────────┘
                       │
            ┌──────────▼──────────┐
            │  Agent 1: Verifier  │  gemini-2.0-flash (JSON)
            │  · isAcademic?      │  → complexity: complex
            │  · complexity?      │  → refinedTopic, description
            └──────────┬──────────┘
                       │
            ┌──────────▼──────────┐
            │  Agent 2: Planner   │  gemini-2.0-flash (JSON)
            │  · N ordered steps  │  → 7 visualization prompts
            │  · image prompts    │  (Excalidraw-style instructions)
            └──────────┬──────────┘
                       │
         ┌─────────────▼──────────────┐
         │  Agent 3: Visualizer (×N)  │  gemini-2.0-flash-preview-image-generation
         │  · one image per step      │  → base64 PNG, rendered in browser
         └────────────────────────────┘
```

## File structure

```
concept-visualizer/
└── index.html      # Complete app — all HTML, CSS, JS in one file
```
