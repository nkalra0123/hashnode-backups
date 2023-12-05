---
title: "Running an LLM locally"
seoTitle: "A Guide to Running Large Language Models Locally"
seoDescription: "Uncover the power of running an LLM locally, granting you the freedom to experiment, fine-tune, and unleash the full potential of these LLMs"
datePublished: Sun Nov 05 2023 06:13:21 GMT+0000 (Coordinated Universal Time)
cuid: clol2unrv000109k021hp9pca
slug: running-an-llm-locally
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/D2LbL802UW0/upload/951e54a42cd64ec24d2367ab43167632.jpeg
tags: llm, llama, lmstudio, ollama, mistral

---

In the ever-evolving landscape of natural language processing, large language models (LLMs) have emerged as powerful tools for understanding and generating human-like text. Running an LLM locally provides enthusiasts and developers with the flexibility to experiment, fine-tune, and explore the capabilities of these models without relying on external services. In this blog post, let's go through the process of setting up and running an LLM on your local machine.

In this blog post, we will learn how to run LLMs locally on your computer.

**First, let's learn some keywords that are used in the LLM world**

1. **Tokenization:**
    
    * *Definition:* The process of breaking down text into smaller units, called tokens. Tokens can be words, subwords, or even characters.
        
    * *Importance:* Tokenization is crucial for feeding input data into language models, as it converts continuous text into a format that the model can understand.
        
2. **Pre-trained Models:**
    
    * *Definition:* Models that have been trained on massive amounts of data before being fine-tuned for specific tasks. Pre-trained models capture general language patterns and knowledge.
        
    * *Importance:* Pre-trained models provide a starting point for various natural language processing tasks, saving time and computational resources.
        
3. **Fine-tuning:**
    
    * *Definition:* The process of training a pre-trained model on a specific dataset or for a specific task to adapt it to a particular use case.
        
    * *Importance:* Fine-tuning allows developers to customize pre-trained models for specific applications, improving performance on domain-specific data.
        
4. **Inference:**
    
    * *Definition:* The process of using a trained model to make predictions or generate outputs based on new, unseen data.
        
    * *Importance:* Inference is the phase where a model demonstrates its learned capabilities by processing real-world inputs and producing relevant outputs.
        
5. **Tokenizer:**
    
    * *Definition:* A tool or module responsible for tokenizing input text, breaking it down into tokens that can be understood by the model.
        
    * *Importance:* Tokenizers play a crucial role in preparing data for language models, ensuring that the input is in a suitable format for processing.
        
6. **Embedding:**
    
    * *Definition:* A numerical representation of words or tokens learned by a language model during training.
        
    * *Importance:* Embeddings capture semantic relationships between words and enable the model to understand the context and meaning of the input.
        
7. **Hyperparameters:**
    
    * *Definition:* Parameters set before the training process begins, influencing the learning process of the model.
        
    * *Importance:* Adjusting hyperparameters can significantly impact a model's performance and generalization to different tasks.
        
8. **Attention Mechanism:**
    
    * *Definition:* A component in neural networks that allows the model to focus on different parts of the input sequence when making predictions.
        
    * *Importance:* Attention mechanisms enhance the model's ability to consider relevant information while processing input data.
        
    
    [Attention is All You Need](https://research.google/pubs/pub46201/)
    
9. **Hugging Face:**
    
    * *Definition:* A popular platform and library that provides a collection of pre-trained models, including transformers for natural language processing tasks.
        
    * *Importance:* Hugging Face simplifies access to state-of-the-art language models and facilitates their integration into various projects.
        
10. **SOTA (State-of-the-Art):**
    
    * *Definition:* SOTA refers to the current highest level of performance or advancement in a particular field, often used to describe the best-known results or models at a given point in time.
        
    * *Importance:* It serves as a benchmark for evaluating the effectiveness of new models, algorithms, or techniques.
        
11. **Transformer:**
    
    * *Definition:* Transformer replaces recurrent or convolutional layers with self-attention mechanisms, allowing for parallelized computation and capturing long-range dependencies in data efficiently.
        
    * *Importance:* Transformers have become a cornerstone in deep learning, particularly in natural language processing. Their ability to process sequential data in parallel enhances training efficiency. Transformers, through self-attention, excel at understanding context and relationships within input sequences.
        
12. **GPT (Generative Pre-trained Transformer):**
    
    * *Definition:* GPT, or Generative Pre-trained Transformer, is a type of large language model architecture that utilizes transformer-based neural networks. Pre-trained on vast datasets, GPT excels in natural language understanding and generation tasks.
        
    * *Importance:* GPT revolutionized language processing by demonstrating exceptional capabilities in text generation, translation, and context comprehension. Its transformer architecture, with self-attention mechanisms, enables capturing intricate relationships in data.
        
13. **Parameters**
    
    * *Definition:* Parameters in LLMs refer to the weights and biases that the model learns during the training process. These numerical values determine the behavior and functionality of the model, allowing it to make predictions, generate text, or perform various natural language processing tasks.
        
    * *Importance:* The number of parameters in an LLM is a critical factor influencing its capacity to capture intricate language patterns and representations. A "20B parameters model," for instance, signifies a model with 20 billion learned parameters. Larger models with more parameters often demonstrate enhanced performance by capturing more nuanced relationships in language. However, the size of the model also comes with increased computational and resource demands, prompting a careful balance between model complexity, performance gains, and resource requirements in the development and deployment of LLMs.
        

These keywords provide a foundational understanding of the concepts and processes involved in the world of Large Language Models.

**Choosing a Model**

The first step in running an LLM locally is choosing a model. You can see the models on Hugging Face

Hugging Face, hosts a handy "[Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)" that does just this, automatically evaluating open LLMs submitted to their [Hub](https://huggingface.co/docs/hub/index) on several foundational benchmarks, measuring various reasoning and knowledge tasks in zero to 25-shot settings. Hugging Face’s four choice benchmarks are:

1. [AI2 Reasoning Challenge](https://arxiv.org/abs/1803.05457) - The ARC question set is partitioned into a Challenge Set and an Easy Set, where the Challenge Set contains only questions answered incorrectly by both a retrieval-based algorithm and a word co-occurence algorithm. The dataset contains only natural, grade-school science questions (authored for human tests), and is the largest public-domain set of this kind (7,787 questions).
    
2. [HellaSwag](https://arxiv.org/abs/1905.07830) - Can a Machine Really Finish Your Sentence? This contains tasks of commonsense natural language inference: given an event description such as "A woman sits at a piano," a machine must select the most likely followup: "She sets her fingers on the keys.
    
3. [Massive Multitask Language Understanding](https://arxiv.org/abs/2009.03300) - The test covers 57 tasks including elementary mathematics, US history, computer science, law, and more. To attain high accuracy on this test, models must possess extensive world knowledge and problem solving ability.
    
4. [TruthfulQA](https://arxiv.org/abs/2109.07958) - Measuring How Models Mimic Human Falsehoods, this measures whether a language model is truthful in generating answers to questions. The benchmark comprises 817 questions that span 38 categories, including health, law, finance and politics.
    

Now the first question, that comes to mind is if these evaluation data sets/ questions are available, then a new LLM can be trained on this data set, and then it will score well in the evaluation.

These things happen

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699113636471/1ffc3f24-3f79-4d22-b91f-78470f00c9bc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699113681243/229afc99-5071-4dd7-8530-007fb9b06bcd.png align="center")

Now the next question in choosing the LLM model is how much resources are needed to run a model.

For this, we will use a tool called [LM Studio](https://lmstudio.ai/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699159954548/51644ba6-9085-4d9c-b1a1-9f9c2ef21aae.png align="center")

It shows you clearly, how much RAM is required and how much disk space is needed to run the model.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699160103797/9c9deabd-9214-4445-a880-abd4b01c522c.png align="center")

You can get this information from the model page on huggingface also

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699160206118/388ccdf9-aa23-48c7-9517-7d271769b4c8.png align="center")

Now what is this Q3, Q4 and gguf

*Q stands for Quantization*

**Quantized** versions of models refer to models that have undergone quantization, which is a process of reducing the precision of the weights and activations in a neural network. The idea is to represent the numerical values with fewer bits (lower precision) without significantly sacrificing the model's performance. This can lead to reduced memory usage and faster inference, making it more efficient for deployment on devices with limited resources.

GGUF and GGML are file formats used for storing models for inference

[GGML officially phased out, new format called GGUF · Issue #1370 · nomic-ai/gpt4all · GitHub](https://github.com/nomic-ai/gpt4all/issues/1370)

Q4 means 4 bit quantized version

* q4\_0 = 32 numbers in chunk, 4 bits per weight, 1 scale value at 32-bit float (5 bits per value in average), each weight is given by the common scale \* quantized value.
    

In simple language, more Quantized version means more loss, but the model will take less resources to run, similar to 144p video resolution compared to 1080p video resolution.

Now we have the app to run LLM and the model, let's download some models

* Zephyr 7B β
    
* Mistral 7B Instruct v0.1
    

With this you get an API server also so that you can use it via API, and this API style is compatible with OpenAI APIs, so you just have to replace the base urls, and then rest of your code that used OpenAI, can the remain same

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699161100844/4f622206-5e01-4fad-80d4-d2250d1492c3.png align="center")

Now let's try asking some questions

with Zephyr

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699161589135/3f9c346f-57d2-4bc9-be6d-009c423206b8.png align="center")

Same question from bard

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699161610267/6ed5188b-f84b-4c6d-bc49-1ab5f6a48272.png align="center")

Same question with Mistral

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699164044704/68019522-81fc-42e8-998d-3dd91d859bc3.png align="center")

There are some other options also to run LLMs locally, we can try using [ollama](https://ollama.ai/)

We can run LLM from the command line.

```bash
ollama run mistral
```

It also exposes and API server, and we can call LLMs with APIs also.

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.