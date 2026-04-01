# Insurance Claims Semantic Search (DataCamp Project)

This project builds a semantic search workflow for workers' compensation insurance claims using OpenAI embeddings and Pinecone vector search.

Instead of keyword matching, the notebook converts claim descriptions into vector embeddings, stores them in a Pinecone index, and retrieves the most similar historical claims for a natural-language query.

## Project Goal

The goal is to answer questions like:

- "Find claims similar to a repetitive strain injury"
- "Which past claim is closest to this accident description?"

This can help claims teams quickly locate comparable cases for triage, investigation, and decision support.

## Dataset

The project uses:

- `insurance_claims_top_100.csv`

The data is a synthetic workers' compensation claims sample (100 records) with fields such as:

- Claim metadata (claim number, accident and report timestamps)
- Worker demographics (age, gender, marital status, dependents)
- Work profile (hours and days worked, wages, part-time/full-time)
- Free-text incident narrative in `ClaimDescription`
- Cost information

The semantic search part of this project uses `ClaimDescription` as the text source for embeddings.

## How The Notebook Works

The notebook (`notebook.ipynb`) follows this pipeline:

1. Install and import dependencies (`openai`, `pinecone`, `pandas`).
2. Load `insurance_claims_top_100.csv` into a DataFrame.
3. Initialize OpenAI and Pinecone clients.
4. Create a Pinecone index (`insurance-claims`) with embedding dimension `1536`.
5. Generate embeddings from each claim description using `text-embedding-3-small`.
6. Upsert vectors into Pinecone with `ClaimNumber` as the vector ID.
7. Run query embedding search and return top similar claims.
8. Map result IDs back to the original claim descriptions in the DataFrame.

## Project Structure

- `notebook.ipynb`: Main end-to-end analysis and semantic search workflow.
- `insurance_claims_top_100.csv`: Input claims data.
- `images/`: Visual assets/screenshots used in notebook instructions.

## Setup

## 1) Prerequisites

- Python 3.9+
- OpenAI API key
- Pinecone API key

## 2) Install packages

```bash
pip install pandas openai pinecone
```

## 3) Configure environment variables

Use environment variables instead of hardcoding keys:

```bash
# PowerShell
$env:OPENAI_API_KEY="your_openai_key"
$env:PINECONE_API_KEY="your_pinecone_key"
```

Then initialize clients in code with environment variables.

## Running The Project

Open `notebook.ipynb` and run cells in order:

1. Dependency install/imports
2. Data load
3. Client + index setup
4. Embedding creation and upsert
5. Query and retrieval examples

Example query used in the notebook:

- `"Worker developed carpal tunnel syndrome from repetitive typing"`

The notebook returns the most similar claim ID and description from the indexed data.

## Expected Output

For a given natural-language claim query, you should receive:

- Top-k similar claim matches from Pinecone
- The closest claim ID
- The corresponding original claim description from the dataset

## Why This Is Useful

- Improves recall over exact keyword search
- Handles phrasing variation in injury descriptions
- Speeds up retrieval of relevant historical cases

## Notes And Recommendations

- The index metric in the notebook is set to `euclidean`; cosine similarity is also common for embedding search.
- Re-running index creation without checking if the index exists may cause errors; add existence checks in production code.
- Include metadata (for example, date, cost, or worker profile) during upsert if you plan to filter search results later.

## Security Note

The current notebook contains API keys directly in code. Rotate those keys and switch to environment-variable based loading immediately.

## Next Improvements (Optional)

- Add metadata to Pinecone vectors and use filtered queries.
- Build a simple evaluation set to measure retrieval quality (precision at k).
- Add a lightweight app endpoint or dashboard for claims search.
- Add robust error handling and logging around API calls.

## Acknowledgment

This project is based on a DataCamp-style applied workflow and uses a synthetic workers' compensation dataset derived from the Kaggle actuarial loss estimation source.