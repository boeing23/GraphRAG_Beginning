# GraphRAG_Beginning
In this repository I will be telling how to get started with GraphRAGs, I have used Neo4J for creating the Knowledge Graphs
Graph RAG Setup Guide
Prerequisites
Ollama

Local LLM runner for executing open-source language models (e.g. Llama2, Mistral)

Installation Guide
Required for local LLM inference without cloud dependencies

Neo4J

Graph database for storing knowledge graph relationships and vector embeddings

Desktop Version: 5.22.0+ (mandatory for vector index support)

Desktop Setup Guide
Vector indexes enable efficient similarity search on embeddings

Docker

Containerization platform for Neo4J deployment

Alternative to desktop app using provided docker-compose.yaml

Neo4J Configuration
Desktop Setup
1. Create new DBMS in Neo4J Desktop
2. Use version `5.22.0` or higher
3. Start database and enable Bolt connector
4. Note credentials for Python connection:
   - bolt://localhost:7687
   - Username: neo4j
   - Password: [your_password]

Also install APOC pluggin for the respective database which you would be using. 

Docker Setup
I have added the docker-compose.yaml file.

Critical Dependencies Breakdown
| Library   	         | Purpose	            | Mathematical/Functional Relevance                               |
| langchain	            | Core RAG framework    | Implements retrieval chains: Retrieval → Ranking → Generation   |
| langchain-ollama      | Local LLM integration | Enables llama2, mistral model inference via Ollama              |
| neo4j	               | Graph database driver |	Implements Cypher query execution with graph.schema validation |
| tiktoken              |	Token counting       | BPE tokenization for context window management (CL100k base)    |
| yfiles_jupyter_graphs |	Graph visualization  |	Force-directed layout algorithms for knowledge graph display   |
| langchain-openai      | Cloud LLM fallback    |	Alternative to Ollama using text-embedding-ada-002 embeddings  |
| python-dotenv	      | Secret management     |	Secure credential loading for Neo4J/OpenAI connections         |
