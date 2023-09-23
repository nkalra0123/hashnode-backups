---
title: "Building a Chat Bot with LLM on your data"
datePublished: Sat Sep 23 2023 08:52:04 GMT+0000 (Coordinated Universal Time)
cuid: clmvsl5h4000008kz1rmw0xnr
slug: building-a-chat-bot-with-llm-on-your-data
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/2EJCSULRwC8/upload/a47e7d496cfa181d791115ff750c27c1.jpeg
tags: chatbot, gpt-3, llm, chatgpt, gpt-4

---

In recent years, chatbots have become an integral part of various industries, from customer support to personal assistants. They enable automated interactions with users, providing timely responses and assistance. With the advent of Large Language Models (LLM) like GPT-3 and GPT-4, building chatbots that can understand and generate human-like text has become more accessible and powerful. In this blog post, we will explore the steps to build a chatbot using an LLM with your data.

Large Language Models are deep learning models trained on massive amounts of text data, enabling them to understand and generate human-like text. They have the ability to complete sentences, generate paragraphs, and even hold coherent conversations with users.

> But they are not trained on your data, your documents, your information.

A solution that is commonly used to solve the above problem is **RAG**

**Retrieval Augmented Generation (RAG)** is an exciting and innovative approach in the field of natural language processing (NLP) that combines the power of both retrieval-based and generative models.

RAG can be used for various tasks like question-answering, text generation, and more. Let's delve into the details, steps, and relevant concepts of RAG.

**What is RAG?**

RAG is a framework that combines two key components:

1. **Retrieval Component**: This part focuses on retrieving relevant information from a large corpus of text. It uses information retrieval techniques to find the most relevant documents or passages related to a given query.
    
2. **Generation Component**: Once the relevant documents or passages are retrieved, the generation component takes over. It uses generative models, like GPT-3 or GPT-4, to generate human-like text based on the retrieved information.
    

This simple approach is able to produce great results.

For the **Retrieval Component** to work, you need to have the documents on which you want LLM to work.

If you have a very small document, you can send that complete document in the API call to LLM. But if your document is big enough or you have multiple documents, you have to select the best documents that you can send to LLM.

Some basic steps that are needed to select the best documents.

1. Load documents
    
2. Split documents
    
3. Create embeddings for documents (using a Text Embedding Model)
    
4. Store documents and embeddings in a vectorstore
    
5. Query the vectorstore and select the best matching documents.
    

Now your documents can be present in PDF files, doc files, CSV or databases, etc, so you might need different types of document loaders, frameworks like [langchain](https://docs.langchain.com/docs/) can help.

Even a single document file, can be big enough, but we need a way to split the file into multiple parts, we can use approaches like:

* splitting the documents based on page numbers
    
* splitting on word count (each split containing 20 words)
    
* creating a split of each line
    
* If your data is in csv, creating a split of each row.
    

Now you have loaded the documents, and splitted the docs, you need to find the embeddings and save them in a place, so that you can retreive the most similar documents easily.

### What are embeddings

An embedding is a vector (list) of floating point numbers.

![](https://cdn.openai.com/new-and-improved-embedding-model/draft-20221214a/vectors-2.svg align="left")

Text embeddings measure the relatedness of text strings. Embeddings are commonly used for:

* **Search** (where results are ranked by relevance to a query string)
    
* **Clustering** (where text strings are grouped by similarity)
    
* **Recommendations** (where items with related text strings are recommended)
    
* **Anomaly detection** (where outliers with little relatedness are identified)
    
* **Diversity measurement** (where similarity distributions are analyzed)
    
* **Classification** (where text strings are classified by their most similar label)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694932146446/638d6c0b-b442-4cf3-a1c8-61704426509d.png align="center")

The distance between two vectors measures their relatedness. Small distances suggest high relatedness and large distances suggest low relatedness

> For measuring distance you can use cosine angle between two vectors

For calculating the embedding, you can use different models. Few options can be

1. [Open AI text embeddings](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)
    
2. Sbert text embeddings
    

[SBERT](https://www.sbert.net/) model is an open source that you can deploy on your own, and use without paying any cost per API call.

But this model truncates the input sequence length to [256 tokens](https://twitter.com/NirantK/status/1681023465082720261)

Now you have the embeddings of the input documents. You have to save the embeddings somewhere. Few options include

* Keeping embedding in memory
    
* Using a vector database
    
    Vector database options include:
    
    * [Chroma](https://github.com/chroma-core/chroma), an open-source embeddings store
        
    * [Milvus](https://github.com/openai/openai-cookbook/blob/main/examples/vector_databases/Using_vector_databases_for_embeddings_search.ipynb), a vector database built for scalable similarity search
        
    * [Pinecone](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/pinecone), a fully managed vector database
        
    * [Qdrant](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/qdrant), a vector search engine
        
    * [Redis](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/redis) as a vector database
        
    * [Typesense](https://typesense.org/docs/0.24.0/api/vector-search.html), fast open source vector search
        
    * [Weaviate](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/weaviate), an open-source vector search engine
        
    * [Zilliz](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/zilliz), data infrastructure, powered by Milvus
        

If the number of embeddings are small, you can keep them in-memory also.

When you receive user input, you have to calculate the embedding and search for the best-matching documents in the vector store, Then we have to send these documents as context, and user query to LLM with a prompt.

A simple prompt can be

> You are an AI assistant. Follow the user's requirements carefully, and try to answer the question from the following context information only. If the user query is not present in the context information, Say "Sorry" and end the chat.
> 
> Context Information: &lt;Best matching documents&gt;
> 
> User Query: &lt;user question&gt;

Complete Diagram of the flow:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694923761181/97ea147a-1091-4438-8a7c-0c8fecb2ae9d.png align="center")

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.