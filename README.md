# Text-to-Flashcard Generator

A single-file web app that turns any block of text — or just a topic — into an interactive deck of flippable flashcards. Paste your notes, a Wikipedia excerpt, or simply type a subject like "The French Revolution", choose how many cards you want, and the app uses the Claude API to generate concise question-and-answer pairs. Cards are displayed one at a time with a smooth 3D flip animation, dot-based progress tracking, and simple previous/next navigation — all in a clean, minimal interface with zero external dependencies.

---

## Prompt engineering techniques used

- **Structured JSON output** — the prompt explicitly instructs the model to return a JSON array with no surrounding prose, ensuring machine-readable output that can be directly parsed.
- **Few-shot example in the prompt** — a concrete example (`[{"question": "What is X?", "answer": "X is Y."}]`) is embedded in the instructions, reducing ambiguity about the exact format expected.
- **Explicit schema constraints** — the prompt specifies that each object must have exactly two keys (`question` and `answer`), which prevents the model from inventing extra fields.
- **Output-only instruction (no preamble)** — the instruction "Return ONLY a valid JSON array — no explanation, no markdown, no code fences" eliminates the conversational filler that language models often add before substantive output, making regex-free parsing more reliable.

---

## Running locally

1. Download or clone this repository.
2. Open `flashcards.html` directly in any modern browser — no server, no build step required.
3. Check **Demo mode** to see a pre-loaded set of five science flashcards instantly.

### Enabling live mode (real Claude API)

1. Obtain an API key from [console.anthropic.com](https://console.anthropic.com).
2. Leave **Demo mode** unchecked.
3. Paste your key into the **Claude API key** field.
4. Enter your text and click **Generate flashcards**.

> **Note:** The app calls the Anthropic API directly from the browser using the `anthropic-dangerous-direct-browser-access` header. Your API key is never sent anywhere except Anthropic's servers and is not stored by the app. For production use, route requests through a backend proxy so the key is not exposed in the browser.

---

## Mock / Demo mode

Demo mode is included specifically for portfolio viewers and anyone who wants to explore the UI without signing up for an API key. When the **Demo mode** checkbox is ticked, the API key field is hidden and a purple badge appears next to the title. Clicking Generate simulates a short loading delay (1.2 seconds) and then loads a hardcoded set of five general-knowledge science flashcards. All interactive features — flipping, navigation, progress dots — work identically to live mode.
