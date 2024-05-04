# Rag from Scratch

- Embedd without OpenAI API Model Using Claude API

- LLMs are a powerful new platform, but they are not always trained on data that is relevant for our tasks. This is where retrieval augmented generation (or RAG) comes in: RAG is a general methodology for connecting LLMs with external data sources such as private or recent data. It allows LLMs to use external data in generation of their output. This notobook build up an understanding of RAG from scratch, starting with the basics of indexing, retrieval, and generation. It will build up to more advanced techniques to address edge cases or challenges in RAG.

- Query rewriting is a popular strategy to improve retrieval. Multi-query is an approach that re-writes a question from multiple perspectives, performs retrieval on each re-written question, and takes the unique union of all docs.
- RAG-fusion is an approach that re-writes a question from multiple perspectives, performs retrieval on each re-written question, and performs reciprocal rank fusion on the results from each retrieval, giving a consolidated ranking. 
- Query decomposition is a strategy to improve question-answering by breaking down a question into sub-questions. These can either be (1) solved sequentially or (2) independently answered followed by consolidation into a final answer. 
- Step-back prompting is an approach to improve retrieval that builds on chain-of-thought reasoning. From a question, it generates a step-back (higher level, more abstract) question that can serve as a precondition to correctly answering the original question. This is especially useful in cases where background knowledge or more fundamental understanding is helpful to answer a specific question.
- HyDE (Hypothetical Document Embeddings) is an approach to improve retrieval that generates hypothetical documents that could be used to answer the user input question. These documents, drawn from the LLMs knowledge, are embedded and used to retrieve documents from an index. The idea is that hypothetical documents may be better aligned with the indexes documents than the raw user question.

- different types of query routing (logical and semantic).
- Problem: We interact w/ databases using domain-specific languages (e.g., SQL, Cypher for Relational and Graph DBs). And, many vectorstores have metadata that can allow for structured queries to filter chunks. But RAG systems ingest questions in natural language.

Idea: A great deal of work has focused on query structuring, the process of text-to-DSL where DSL is a domain specific language required to interact with a given database. This converts user questions into structured queries. Below are links that dive into text-to-SQL/Cypher, and the below video overviews query structuring for vectorstores using function calling.

- focuses on some useful tricks for indexing full documents.

Problem: Many RAG approaches focus on splitting documents into chunks and returning some number upon retrieval for the LLM. But chunk size and chunk number can be brittle parameters that many user find difficult to set; both can significantly affect results if they do not contain all context to answer a question.

Idea: Proposition indexing (@tomchen0 et al) is a nice paper that uses an LLM to produce document summaries ("propositions") that are optimized for retrieval. We've built on this with two retrievers: (1) multi-vector retriever embeds summaries, but returns full documents to the LLM. (2) parent-doc retriever embeds chunks but returns full documents to the LLM. Idea is to get best of both worlds: use smaller / concise representations (summaries or chunks) to retrieve, but link them to full documents / context for generation.

The approach is very general, and can be applied to tables or images: in both cases, index a summary but return the raw table or image for reasoning. This gets around challenges w/ directly embedding tables or images (multi-modal embeddings), using a summary as a representation for text-based similarity search.

