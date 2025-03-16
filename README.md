# GraphRAG_Beginning
In this repository I will be telling how to get started with GraphRAGs, I have used Neo4J for creating the Knowledge Graphs
Graph RAG Setup Guide
Prerequisites
Ollama

# Local LLM runner for executing open-source language models (e.g. Llama2, Mistral)

# Installation Guide
Required for local LLM inference without cloud dependencies

# Neo4J

Graph database for storing knowledge graph relationships and vector embeddings

Desktop Version: 5.22.0+ (mandatory for vector index support)

# Desktop Setup Guide
- Vector indexes enable efficient similarity search on embeddings

# Docker

- Containerization platform for Neo4J deployment
- Alternative to desktop app using provided docker-compose.yaml

# Neo4J Configuration
Desktop Setup
1. Create new DBMS in Neo4J Desktop
2. Use version `5.22.0` or higher
3. Start database and enable Bolt connector
4. Note credentials for Python connection:
   - bolt://localhost:7687
   - Username: neo4j
   - Password: [your_password]

Also install APOC pluggin for the respective database which you would be using. 

# Docker Setup
I have added the docker-compose.yaml file.

# Critical Dependencies Breakdown
| Library   	         | Purpose	            | Mathematical/Functional Relevance                               |
|-----------------------|-----------------------|-----------------------------------------------------------------|
| langchain	            | Core RAG framework    | Implements retrieval chains: Retrieval → Ranking → Generation   |
| langchain-ollama      | Local LLM integration | Enables llama2, mistral model inference via Ollama              |
| neo4j	               | Graph database driver |	Implements Cypher query execution with graph.schema validation |
| tiktoken              |	Token counting       | BPE tokenization for context window management (CL100k base)    |
| yfiles_jupyter_graphs |	Graph visualization  |	Force-directed layout algorithms for knowledge graph display   |
| langchain-openai      | Cloud LLM fallback    |	Alternative to Ollama using text-embedding-ada-002 embeddings  |
| python-dotenv	      | Secret management     |	Secure credential loading for Neo4J/OpenAI connections         |


# Version Strategy
- `--upgrade --quiet` ensures latest stable versions with clean output
- Pinned versions in requirements.txt recommended for production

# Why This Matters for Graph RAG:

- Hybrid Retrieval
1. neo4j handles graph-aware retrieval (relationship traversal)
2. langchain-openai provides vector similarity (cosine distance in embedding space)

- Local Inference Stack
1. langchain-ollama + llama2 enables private document processing
2. 50% reduction in cloud API costs compared to GPT-4

- Debugging Tools
1. yfiles_jupyter_graphs visualizes graph structure using Fruchterman-Reingold layout
2. json-repair handles malformed LLM outputs (common in complex queries)

# Cosine Similarity: Core Concept
Mathematical Definition
Measures similarity between two vectors by calculating the cosine of the angle between them:
```
Similarity = cos(θ) = (A · B) / (||A|| ||B||)
```
Output Range: -1 (opposite) to 1 (identical). In text embeddings, values typically range 0-1 due to

# Why It Matters in Graph RAG
1. Semantic Matching
   - Text chunks/documents are encoded as dense vectors (embeddings) using models like sentence-transformers
   - Cosine similarity identifies semantically related content even without exact keyword matches

2. Vector Index Efficiency
- Neo4J uses this metric for:

```
CREATE VECTOR INDEX chunk_embeddings FOR (n:Chunk) ON n.embedding OPTIONS {indexConfig: {`vector.dimensions`: 384, `vector.similarity`: 'cosine'}}
```
- Enables fast nearest-neighbor search (O(1) to O(log N) complexity)
  
3. Magnitude Invariance
Ignores vector length differences, focusing purely on directional alignment. Critical for comparing:
- Short queries vs long documents
- Paragraphs vs sentences

# Example in Your Pipeline
When processing a query "Explain neural networks":
- Query → Embedding vector Q
- Calculate cosine similarity with all chunk embeddings C_i
```
Similarity = cos(θ) = (Q · C_i) / (||Q|| ||C_i||)
```
