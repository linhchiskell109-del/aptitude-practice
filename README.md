# Attitude

A focused aptitude-test MVP built with React, Vite, TypeScript, and Tailwind CSS. Practice sets and Mixed Sprint load from a Question Bank of PDF page images across Deductive, Diagrammatic, Logical, Numerical, Situational Judgement, and Verbal reasoning.

A lightweight shared-password access gate sits in front of the app. Set `VITE_ACCESS_PASSWORD` in `.env.local` (see `.env.example`). Sessions and practice history are stored in browser `localStorage`, keyed by the name entered at the gate.

## Run the app

```bash
npm install
cp .env.example .env.local   # then set VITE_ACCESS_PASSWORD
npm run dev -- --host 0.0.0.0 --port 43123
```

Quality checks:

```bash
npm test
npm run lint
npm run build
```

## Question Bank pipeline

Questions PDFs are paired with the matching Solutions or Answers PDF by source-test name. Instruction pages are skipped. Each ingested item records:

- id
- category
- source test
- question / page number
- question image path
- `interactionType` and option labels detected from the source page
- answer and explanation when the solutions PDF prints them

Question visuals stay as rendered PDF pages. The app does not recreate diagrams, charts, or question layouts in HTML.

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements-ingestion.txt
.venv/bin/python scripts/ingest_pdfs.py \
  --catalog scripts/full-catalog.json \
  --source-root /path/to/extracted-zip \
  --output public/content \
  --bank src/data/question-bank.json
```

Single-pair ingest is still available:

```bash
.venv/bin/python scripts/ingest_pdfs.py \
  --questions "Numerical Reasoning/NumericalReasoningTest1-Questions.pdf" \
  --solutions "Numerical Reasoning/NumericalReasoningTest1-Solutions.pdf" \
  --title "Numerical Reasoning Test 1" \
  --category "Numerical Reasoning"
```

Output is written under `public/content/<source-id>/` plus `src/data/question-bank.json`. Practice sets and Mixed Sprint read that bank; questions are not hardcoded in React components.
