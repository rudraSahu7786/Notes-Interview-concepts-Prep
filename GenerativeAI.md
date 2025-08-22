# Generative AI - Comprehensive Interview Notes

## Table of Contents
1. [Fundamental Concepts](#fundamental-concepts)
2. [Large Language Models (LLMs)](#large-language-models-llms)
3. [Tokens and Tokenization](#tokens-and-tokenization)
4. [Model Architectures](#model-architectures)
5. [Training Process](#training-process)
6. [Prompt Engineering](#prompt-engineering)
7. [LangChain Framework](#langchain-framework)
8. [Retrieval-Augmented Generation (RAG)](#retrieval-augmented-generation-rag)
9. [Vector Databases and Embeddings](#vector-databases-and-embeddings)
10. [Fine-tuning and Optimization](#fine-tuning-and-optimization)
11. [Evaluation Metrics](#evaluation-metrics)
12. [Deployment and Production](#deployment-and-production)
13. [Ethics and Limitations](#ethics-and-limitations)
14. [Common Interview Questions](#common-interview-questions)

---

## Fundamental Concepts

### What is Generative AI?
**Definition**: Generative AI refers to artificial intelligence systems that can create new content, including text, images, code, audio, and video, based on patterns learned from training data.

**Key Characteristics**:
- **Creative**: Generates novel content rather than just classifying or predicting
- **Pattern-based**: Learns statistical patterns from large datasets
- **Probabilistic**: Uses probability distributions to generate outputs
- **Context-aware**: Considers context and relationships in data

### Types of Generative AI Models
1. **Language Models**: GPT, Claude, PaLM, LLaMA
2. **Image Generation**: DALL-E, Midjourney, Stable Diffusion
3. **Code Generation**: Codex, GitHub Copilot
4. **Multimodal**: GPT-4V, Claude 3, Gemini

---

## Large Language Models (LLMs)

### Definition
**LLM**: A large language model is a type of neural network trained on vast amounts of text data to understand and generate human-like text.

### Key Properties
- **Scale**: Billions to trillions of parameters
- **Emergent Abilities**: Capabilities that arise at scale (reasoning, few-shot learning)
- **Transfer Learning**: Can adapt to new tasks without retraining
- **In-context Learning**: Learn from examples in the input prompt

### Popular LLMs
| Model | Parameters | Company | Key Features |
|-------|------------|---------|--------------|
| GPT-4 | ~1.8T | OpenAI | Multimodal, strong reasoning |
| Claude 3 | Unknown | Anthropic | Long context, safety-focused |
| PaLM 2 | 340B | Google | Multilingual, coding |
| LLaMA 2 | 7B-70B | Meta | Open source, efficient |

### LLM Capabilities
- **Text Generation**: Creative writing, summarization
- **Question Answering**: Factual and analytical responses
- **Code Generation**: Programming in multiple languages
- **Translation**: Between different languages
- **Reasoning**: Logic, math, problem-solving
- **Analysis**: Document review, data interpretation

---

## Tokens and Tokenization

### What are Tokens?
**Definition**: Tokens are the smallest units of text that a language model processes. They can represent words, subwords, characters, or symbols.

### Tokenization Process
1. **Input Text**: "Hello, world!"
2. **Tokenization**: ["Hello", ",", " world", "!"]
3. **Token IDs**: [15496, 11, 995, 0]
4. **Processing**: Model works with numerical token IDs

### Types of Tokenization
- **Word-level**: Each word is a token
- **Subword**: Byte Pair Encoding (BPE), SentencePiece
- **Character-level**: Each character is a token
- **Byte-level**: Each byte is a token

### Key Metrics
- **Vocabulary Size**: Number of unique tokens (typically 32K-100K)
- **Token Limits**: Maximum sequence length (2K-2M tokens)
- **Token Efficiency**: Tokens per word (varies by language)

### Common Tokenizers
- **GPT**: BPE-based (tiktoken)
- **BERT**: WordPiece
- **T5**: SentencePiece
- **Claude**: Custom tokenizer

---

## Model Architectures

### Transformer Architecture
**Core Components**:

#### 1. Attention Mechanism
- **Self-Attention**: Relates different positions in a sequence
- **Multi-Head Attention**: Multiple attention heads for different relationships
- **Scaled Dot-Product**: Attention(Q,K,V) = softmax(QK^T/√d_k)V

#### 2. Encoder-Decoder vs Decoder-Only
- **Encoder-Decoder**: BERT (encoder), T5 (both)
- **Decoder-Only**: GPT family, most modern LLMs
- **Encoder-Only**: BERT for classification tasks

#### 3. Key Components
```
Input → Embedding → Positional Encoding → 
Transformer Layers → Output Projection → Probability Distribution
```

### Architecture Variations
- **GPT**: Decoder-only, autoregressive generation
- **BERT**: Encoder-only, bidirectional understanding
- **T5**: Encoder-decoder, text-to-text transfer
- **PaLM**: Decoder-only with improvements

### Recent Innovations
- **Mixture of Experts (MoE)**: Sparse activation patterns
- **Retrieval-Augmented**: External knowledge integration
- **Multi-Modal**: Text, image, audio processing
- **Long Context**: Extended sequence lengths

---

## Training Process

### Pre-training
**Objective**: Learn general language patterns from massive text corpora

**Process**:
1. **Data Collection**: Web crawls, books, articles (trillions of tokens)
2. **Data Preprocessing**: Cleaning, filtering, deduplication
3. **Self-Supervised Learning**: Predict next token given context
4. **Optimization**: Gradient descent with massive compute resources

### Supervised Fine-Tuning (SFT)
**Purpose**: Adapt model to specific tasks or formats
- **Instruction Tuning**: Follow human instructions
- **Task-Specific**: Question answering, summarization
- **Format Alignment**: Consistent response patterns

### Reinforcement Learning from Human Feedback (RLHF)
**Goal**: Align model outputs with human preferences

**Steps**:
1. **Preference Data**: Humans rank model outputs
2. **Reward Model**: Train model to predict human preferences
3. **Policy Optimization**: PPO to maximize reward
4. **Iterative Improvement**: Multiple rounds of refinement

### Constitutional AI
**Approach**: Training models to follow a set of principles
- **Self-Critique**: Model evaluates its own outputs
- **Revision**: Improves responses based on principles
- **Harmlessness**: Reduces harmful outputs

---

## Prompt Engineering

### Definition
**Prompt Engineering**: The practice of designing inputs to guide AI models toward desired outputs.

### Core Principles
1. **Clarity**: Be specific and unambiguous
2. **Context**: Provide relevant background information
3. **Structure**: Use clear formatting and organization
4. **Examples**: Show desired output format
5. **Constraints**: Specify limitations and requirements

### Prompting Techniques

#### 1. Zero-Shot Prompting
```
Classify the sentiment: "I love this product!"
```

#### 2. Few-Shot Prompting
```
Classify sentiment:
"Great service" → Positive
"Terrible experience" → Negative
"It was okay" → Neutral
"Amazing quality!" → ?
```

#### 3. Chain-of-Thought (CoT)
```
Solve: 23 + 47 = ?
Let me think step by step:
First, I'll add the ones place: 3 + 7 = 10
Then add the tens place: 2 + 4 = 6, plus 1 carried = 7
Therefore: 23 + 47 = 70
```

#### 4. Tree of Thoughts
- Generate multiple reasoning paths
- Evaluate each path
- Select best solution

#### 5. ReAct (Reasoning + Acting)
```
Question: What's the weather in Paris?
Thought: I need current weather information
Action: Search for Paris weather
Observation: 22°C, partly cloudy
Thought: I have the information needed
Answer: The weather in Paris is 22°C and partly cloudy
```

### Advanced Techniques
- **Role Prompting**: "Act as an expert in..."
- **System Messages**: High-level behavioral instructions
- **Temperature Control**: Creativity vs consistency
- **Constrained Generation**: JSON, XML format requirements

---

## LangChain Framework

### What is LangChain?
**Definition**: An open-source framework for building applications with large language models, providing abstractions and tools for common LLM use cases.

### Core Components

#### 1. LLMs and Chat Models
```python
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI

llm = OpenAI(temperature=0.7)
chat = ChatOpenAI(model_name="gpt-4")
```

#### 2. Prompts and Prompt Templates
```python
from langchain import PromptTemplate

template = """
You are a {role}. Answer the following question:
Question: {question}
Answer:
"""
prompt = PromptTemplate(template=template, input_variables=["role", "question"])
```

#### 3. Chains
**Types**:
- **LLMChain**: Basic LLM + prompt combination
- **Sequential Chains**: Chain multiple operations
- **Router Chains**: Dynamic routing based on input

```python
from langchain.chains import LLMChain
chain = LLMChain(llm=llm, prompt=prompt)
```

#### 4. Agents and Tools
```python
from langchain.agents import initialize_agent, Tool

tools = [
    Tool(name="Calculator", func=calculator, description="For math"),
    Tool(name="Search", func=search, description="For current info")
]

agent = initialize_agent(tools, llm, agent="zero-shot-react-description")
```

#### 5. Memory
- **ConversationBufferMemory**: Stores conversation history
- **ConversationSummaryMemory**: Summarizes old conversations
- **Vector Store Memory**: Retrieves relevant past conversations

### LangChain Use Cases
- **Chatbots**: Conversational interfaces
- **Document Q&A**: Query document collections
- **Data Analysis**: Natural language data queries
- **Code Generation**: Programming assistants
- **Content Generation**: Automated writing

---

## Retrieval-Augmented Generation (RAG)

### Definition
**RAG**: A technique that combines retrieval of relevant documents with language generation to provide more accurate, up-to-date, and contextually relevant responses.

### RAG Architecture
```
Query → Retrieval System → Relevant Documents → 
LLM (Query + Documents) → Generated Response
```

### Components

#### 1. Document Processing
1. **Ingestion**: Load documents (PDF, text, web pages)
2. **Chunking**: Split into manageable pieces
3. **Embedding**: Convert to vector representations
4. **Storage**: Save in vector database

#### 2. Retrieval System
- **Dense Retrieval**: Semantic similarity using embeddings
- **Sparse Retrieval**: Keyword-based (BM25, TF-IDF)
- **Hybrid Retrieval**: Combine dense and sparse methods

#### 3. Generation
- **Context Injection**: Add retrieved docs to prompt
- **Response Generation**: LLM generates answer
- **Source Attribution**: Cite relevant documents

### RAG Implementations

#### Basic RAG Pipeline
```python
# 1. Create embeddings
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(docs, embeddings)

# 2. Create retriever
retriever = vectorstore.as_retriever()

# 3. Create RAG chain
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=retriever
)
```

#### Advanced RAG Techniques
- **HyDE**: Hypothetical Document Embeddings
- **Multi-Query**: Generate multiple query variations
- **Re-ranking**: Score and reorder retrieved documents
- **Iterative Retrieval**: Multiple retrieval rounds

### RAG vs Fine-tuning
| Aspect | RAG | Fine-tuning |
|--------|-----|-------------|
| Knowledge Updates | Real-time | Requires retraining |
| Computational Cost | Lower | Higher |
| Domain Adaptation | Easy | Complex |
| Transparency | High (shows sources) | Low |
| Accuracy | Good with quality data | Can be higher |

---

## Vector Databases and Embeddings

### Text Embeddings
**Definition**: Dense numerical representations of text that capture semantic meaning in high-dimensional space.

### Popular Embedding Models
- **OpenAI**: text-embedding-ada-002
- **Sentence Transformers**: all-MiniLM-L6-v2
- **Google**: Universal Sentence Encoder
- **Cohere**: embed-english-v2.0

### Vector Databases
**Purpose**: Store, index, and search high-dimensional vectors efficiently

#### Popular Vector Databases
1. **Pinecone**: Managed, scalable vector database
2. **Weaviate**: Open-source with GraphQL API
3. **Chroma**: Simple, open-source option
4. **Milvus**: Highly scalable, open-source
5. **Qdrant**: Fast similarity search engine
6. **FAISS**: Facebook's similarity search library

#### Key Features
- **Similarity Search**: Find most similar vectors
- **Filtering**: Combine vector search with metadata filters
- **Indexing**: Efficient retrieval algorithms (HNSW, IVF)
- **Scalability**: Handle billions of vectors

### Vector Operations
- **Cosine Similarity**: Measure angle between vectors
- **Euclidean Distance**: Straight-line distance
- **Dot Product**: Projection similarity
- **Approximate Nearest Neighbor (ANN)**: Fast similarity search

---

## Fine-tuning and Optimization

### Fine-tuning Approaches

#### 1. Full Fine-tuning
- **Process**: Update all model parameters
- **Use Case**: Task-specific adaptation
- **Requirements**: Large datasets, significant compute

#### 2. Parameter-Efficient Fine-tuning (PEFT)
**LoRA (Low-Rank Adaptation)**:
- Add small trainable matrices to existing weights
- Reduce trainable parameters by 99%
- Maintain model performance

**Other PEFT Methods**:
- **Adapter Layers**: Insert small networks between transformer layers
- **Prefix Tuning**: Learn task-specific prefix vectors
- **P-tuning v2**: Continuous prompt optimization

#### 3. In-Context Learning
- **Few-shot**: Provide examples in the prompt
- **Zero-shot**: Task description only
- **Instruction Following**: Natural language instructions

### Model Optimization

#### Quantization
- **8-bit**: Reduce memory usage by 50%
- **4-bit**: Further reduction with minimal quality loss
- **Dynamic Quantization**: Runtime optimization

#### Knowledge Distillation
- **Teacher-Student**: Large model trains smaller model
- **Model Compression**: Maintain performance with fewer parameters
- **Deployment Efficiency**: Faster inference

#### Pruning
- **Structured**: Remove entire layers or attention heads
- **Unstructured**: Remove individual weights
- **Gradual Pruning**: Iterative weight removal

---

## Evaluation Metrics

### Text Generation Metrics

#### 1. Perplexity
- **Definition**: Measures how well model predicts text
- **Formula**: PPL = exp(-1/N * Σ log P(word_i))
- **Lower is better**: Better language modeling

#### 2. BLEU Score
- **Purpose**: Machine translation and text generation
- **Method**: N-gram overlap between generated and reference text
- **Range**: 0-100, higher is better

#### 3. ROUGE Score
- **Variants**: ROUGE-1, ROUGE-2, ROUGE-L
- **Purpose**: Summarization and text generation
- **Method**: Recall-based overlap metrics

#### 4. BERTScore
- **Advantage**: Semantic similarity using embeddings
- **Method**: Cosine similarity between BERT embeddings
- **Better**: Captures semantic meaning vs surface overlap

### LLM-Specific Evaluations

#### 1. Human Evaluation
- **Helpfulness**: Does it answer the question?
- **Harmlessness**: Is it safe and appropriate?
- **Honesty**: Is it truthful and acknowledges limitations?

#### 2. Automated Benchmarks
- **MMLU**: Massive Multitask Language Understanding
- **HellaSwag**: Common sense reasoning
- **HumanEval**: Code generation capability
- **TruthfulQA**: Truthfulness evaluation

#### 3. Task-Specific Metrics
- **Accuracy**: Classification tasks
- **F1 Score**: Balanced precision and recall
- **Exact Match**: Question answering
- **CodeBLEU**: Code generation quality

---

## Deployment and Production

### Model Serving

#### 1. API Endpoints
- **REST APIs**: HTTP-based model access
- **GraphQL**: Flexible query language
- **gRPC**: High-performance RPC
- **WebSocket**: Real-time bidirectional communication

#### 2. Serving Frameworks
- **FastAPI**: Python web framework
- **TorchServe**: PyTorch model serving
- **TensorFlow Serving**: TensorFlow model serving
- **Hugging Face Inference Endpoints**: Managed serving

#### 3. Cloud Platforms
- **OpenAI API**: GPT models as service
- **Azure OpenAI**: Enterprise OpenAI access
- **AWS Bedrock**: Multiple model providers
- **Google Vertex AI**: Google's ML platform
- **Anthropic API**: Claude model access

### Scaling Considerations

#### 1. Inference Optimization
- **Batching**: Process multiple requests together
- **Caching**: Store frequent responses
- **Model Quantization**: Reduce memory usage
- **Hardware Acceleration**: GPUs, TPUs, custom chips

#### 2. Load Management
- **Rate Limiting**: Control request frequency
- **Queue Management**: Handle high traffic
- **Auto-scaling**: Dynamic resource allocation
- **Circuit Breakers**: Prevent system overload

#### 3. Monitoring and Logging
- **Performance Metrics**: Latency, throughput, errors
- **Model Metrics**: Quality, bias, drift detection
- **User Analytics**: Usage patterns, satisfaction
- **Cost Tracking**: Resource utilization costs

---

## Ethics and Limitations

### Ethical Considerations

#### 1. Bias and Fairness
- **Training Data Bias**: Historical and societal biases
- **Algorithmic Bias**: Model amplifies existing biases
- **Mitigation**: Diverse training data, bias detection, fairness constraints

#### 2. Privacy and Security
- **Data Privacy**: Training data may contain personal information
- **Model Inversion**: Extracting training data from models
- **Adversarial Attacks**: Malicious inputs to manipulate outputs

#### 3. Misinformation and Hallucination
- **Hallucination**: Generating false but plausible information
- **Confidence Calibration**: Model confidence doesn't match accuracy
- **Source Attribution**: Difficulty tracing information sources

### Technical Limitations

#### 1. Context Length
- **Fixed Context Window**: Limited by model architecture
- **Long Document Processing**: Requires chunking strategies
- **Context Overflow**: Information loss with long inputs

#### 2. Reasoning Limitations
- **Logical Reasoning**: Struggles with complex logic
- **Mathematical Computation**: Arithmetic errors
- **Temporal Reasoning**: Understanding time sequences

#### 3. Multimodal Challenges
- **Cross-Modal Understanding**: Connecting text and images
- **Grounding**: Relating abstract concepts to real world
- **Consistency**: Maintaining coherence across modalities

### Responsible AI Practices

#### 1. Model Cards and Documentation
- **Model Purpose**: Intended use cases
- **Training Data**: Data sources and characteristics
- **Limitations**: Known weaknesses and failure modes
- **Evaluation Results**: Performance on benchmarks

#### 2. Safety Measures
- **Content Filtering**: Remove harmful outputs
- **Robustness Testing**: Adversarial and edge cases
- **Human Oversight**: Human-in-the-loop systems
- **Gradual Deployment**: Phased rollouts with monitoring

---

## Common Interview Questions

### Conceptual Questions

1. **Explain the difference between discriminative and generative models.**
   - Discriminative: P(y|x) - classify or predict
   - Generative: P(x,y) or P(x) - generate new samples

2. **What makes transformer architecture special for language modeling?**
   - Self-attention mechanism
   - Parallel processing
   - Long-range dependencies
   - Transfer learning capabilities

3. **How does attention mechanism work?**
   - Query-Key-Value computation
   - Scaled dot-product attention
   - Multi-head attention for different relationships

### Technical Implementation

4. **How would you implement a RAG system?**
   - Document processing and chunking
   - Vector embedding and storage
   - Retrieval mechanism
   - LLM integration for generation

5. **What are the trade-offs between fine-tuning and prompt engineering?**
   - Computational cost vs flexibility
   - Task-specific performance vs generalizability
   - Data requirements and update frequency

6. **How do you handle hallucination in LLMs?**
   - Source attribution and citations
   - Confidence scoring and calibration
   - Retrieval-augmented approaches
   - Human verification loops

### Practical Applications

7. **Design a chatbot system for customer support.**
   - Intent recognition and classification
   - Knowledge base integration (RAG)
   - Conversation memory and context
   - Escalation to human agents

8. **How would you evaluate the quality of generated text?**
   - Automated metrics (BLEU, ROUGE, BERTScore)
   - Human evaluation criteria
   - Task-specific metrics
   - A/B testing with users

9. **What considerations are important for production deployment?**
   - Latency and throughput requirements
   - Cost optimization strategies
   - Monitoring and observability
   - Safety and content filtering

### Advanced Topics

10. **Explain different types of prompt engineering techniques.**
    - Few-shot learning with examples
    - Chain-of-thought reasoning
    - Tool usage and ReAct
    - Constitutional AI principles

11. **How do you handle long documents that exceed context length?**
    - Chunking strategies (semantic, fixed-size)
    - Hierarchical summarization
    - MapReduce-style processing
    - Retrieval-based approaches

12. **What are the current limitations of LLMs and how might they be addressed?**
    - Reasoning capabilities and formal logic
    - Factual accuracy and knowledge updates
    - Computational efficiency and scaling
    - Alignment and safety concerns

---

## Quick Reference

### Key Terms Glossary
- **Autoregressive**: Generates output sequentially, each token depends on previous ones
- **Transformer**: Neural network architecture using self-attention mechanisms
- **Fine-tuning**: Adapting pre-trained model to specific tasks
- **Embedding**: Dense vector representation of text capturing semantic meaning
- **Temperature**: Parameter controlling randomness in generation (0=deterministic, 1=creative)
- **Top-k/Top-p**: Sampling strategies for controlling output diversity
- **RLHF**: Reinforcement Learning from Human Feedback for alignment
- **Constitutional AI**: Training approach using principles and self-critique

### Model Comparison
| Model Type | Best For | Limitations |
|------------|----------|-------------|
| GPT-4 | General reasoning, coding | Expensive, slower |
| Claude 3 | Long context, safety | Less creative |
| Gemini | Multimodal tasks | Newer, less proven |
| Open Source (LLaMA) | Customization, cost | Requires technical expertise |

### Performance Optimization Checklist
- [ ] Use appropriate model size for task
- [ ] Implement request batching
- [ ] Cache frequent responses
- [ ] Monitor token usage and costs
- [ ] Set appropriate temperature and sampling
- [ ] Implement timeout and retry logic
- [ ] Use structured output formats when possible
- [ ] Monitor for quality degradation

---

*This document serves as a comprehensive reference for Generative AI interview preparation. Focus on understanding concepts deeply rather than memorizing definitions, and be prepared to discuss practical implementation challenges and trade-offs.*
