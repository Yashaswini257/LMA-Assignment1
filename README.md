# RAG: Baseline vs. Reranking

Language Models and Agents assignment comparing bi-encoder retrieval with cross-encoder reranking on the [FIFA Club World Cup 2025 Regulations](https://digitalhub.fifa.com/m/18848e4224efbd91/original/FCWC25_Regulations_EN.pdf).

## Approach

- **Baseline:** retrieve 10 chunks using normalized embeddings and FAISS; pass the top 3 to the LLM.
- **Reranked:** score the same 10 candidates with a cross-encoder; pass the reordered top 3 to the same LLM using the same prompt template.

Models: `sentence-transformers/all-MiniLM-L6-v2`, `cross-encoder/ms-marco-MiniLM-L6-v2`, and `models/gemini-3.5-flash-lite`.

## Run

1. Open the RAG notebook in Jupyter or VS Code.
2. Run the dependency installation cell.
3. Download the PDF above and set `PDF_PATH` to its local location.
4. Keep `fifa_stress_tests.py` beside the notebook if its `%run` cell is present.
5. Provide your own Gemini API key through `GEMINI_API_KEY` or the hidden input prompt. Never commit the key.
6. Run the cells in order. Generation requires API access and available quota; successful answers are saved for resuming.

## Results

Evaluation used 28 exploratory questions over 132 chunks.

| Metric | Baseline | Reranked |
| --- | ---: | ---: |
| Labelled-evidence hit@3 | 20/28 (71.43%) | 21/28 (75.00%) |
| Mean retrieval latency | 8.88 ms | 29.55 ms |

Reranking recovered evidence for Q30 and Q39, but lost evidence for Q36. Evidence hits measure retrieval, not answer correctness. Latency excludes LLM generation and was measured over five repetitions per question.

The notebook contains the implementation and outputs. Detailed answers, rank changes, and timings are saved under `rag_results/`; the report discusses the qualitative findings and computational trade-off.
