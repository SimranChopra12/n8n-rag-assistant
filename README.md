# OSFI Guidelines RAG Assistant (n8n)

A retrieval-augmented assistant that answers questions about OSFI Guideline E-23
(model risk management) and B-10 (third-party risk), cites its sources, and
refuses questions the documents don't cover. Built in n8n.

## Why
Regulatory guidelines are long and hard to search. This explores how reliably a
low-code RAG pipeline answers from them, and how to measure that.

## Demo
[video link] | [screenshots]

## Architecture
[Mermaid diagram]

## Stack
n8n | [Gemini embedding model name] | OpenAI [model name] | n8n Simple Vector Store
Chunking: 1000 characters, 150 overlap | Retrieval: top 8 chunks

## Results (20-question test set)
| Run | Settings | Pass | Partial | Fail | Strict accuracy |
|---|---|---|---|---|---|
| Baseline | Limit 4, original prompt | 12 | 6 | 2 | 60% |
| Run 2 | Limit 8, revised prompt | 14 | 6 | 0 | 70% |

| Question type | Baseline | Run 2 |
|---|---|---|
| Fact lookup (12) | 8 | 8 |
| Multi-source (5) | 1 | 3 |
| Out of scope (3) | 3 | 3 |

Scoring: Pass only if every part of the expected answer is present and correct.
Half credit for Partial gives 75% to 85%.

## What I found
- Retrieval depth and prompt changes helped multi-document questions most.
- Remaining failures: [Q3 invented a quotation; Q10 mislabeled a term; Q7 omitted
  one lifecycle component; Q6 retrieved the wrong principle].
- The assistant is good at finding passages and prone to embellishing them.

## Limitations
- Tuned on the same 20 questions it is scored on [add held-out results if done].
- In-memory vector store: data is cleared on restart.
- Single scorer; small corpus; citations name document types, not pages.

## From prototype to production
Persistent vector store, authentication and managed credentials, logging, a
regression test run on every change, access control, cost limits, privacy review,
and human review of outputs.

## How to run
1. Import both workflows. 2. Add your own API credentials. 3. Download the
documents listed in SOURCES.md. 4. Upload them through the ingestion form.
5. Ask questions in the chat.

## Sources
See SOURCES.md. The source PDFs are not included.
