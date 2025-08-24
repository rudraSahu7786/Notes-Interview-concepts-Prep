# LangChain Interview Notes

**Essential Concepts for GenAI Engineers**

---

## 1. What is LangChain?

**Definition:** Python/JavaScript framework for building applications with Large Language Models (LLMs). Provides abstractions and tools to create complex AI workflows.

**Key Value Propositions:**

* **Data Connection:** Connect LLMs to data sources
* **Composability:** Chain multiple operations
* **Memory:** Add state and conversation history
* **Agents:** Let LLMs decide actions

**When to Use LangChain:**

* Building RAG (Retrieval-Augmented Generation) systems
* Creating conversational AI with memory
* Integrating multiple data sources
* Building autonomous agents
* Complex multi-step AI workflows

---

## 2. Core Components (Must Know)

### 2.1 LLMs and Chat Models

```python
from langchain.llms import OpenAI
llm = OpenAI(temperature=0.7, max_tokens=100)
response = llm("What is machine learning?")

from langchain.chat_models import ChatOpenAI
from langchain.schema import HumanMessage, AIMessage, SystemMessage

chat = ChatOpenAI(model_name="gpt-3.5-turbo")
messages = [SystemMessage(content="You are a helpful assistant"),
            HumanMessage(content="Explain neural networks")]
response = chat(messages)
```

**Interview Tip:** LLMs for text completion, Chat Models for conversations.

### 2.2 Prompts and Templates

```python
from langchain.prompts import PromptTemplate, ChatPromptTemplate
from langchain.prompts.few_shot import FewShotPromptTemplate

# Basic Prompt
prompt = PromptTemplate(input_variables=["product", "audience"], template="Write a marketing copy for {product} targeting {audience}")

# Chat Prompt
chat_prompt = ChatPromptTemplate.from_messages([("system", "You are an expert {role}"), ("human", "Please help me with: {question}")])
```

**Interview Question:** How to handle dynamic prompts? Use templates with conditional formatting or custom prompt classes.

### 2.3 Output Parsers

```python
from langchain.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field

class ProductReview(BaseModel):
    product_name: str = Field(description="Name of the product")
    rating: int = Field(description="Rating from 1-5")
    pros: list[str] = Field(description="List of positive aspects")
    cons: list[str] = Field(description="List of negative aspects")

parser = PydanticOutputParser(pydantic_object=ProductReview)
```

**Interview Question:** How to ensure structured outputs? Use output parsers with formatting instructions.

---

## 3. Chains

### 3.1 LLMChain

```python
from langchain.chains import LLMChain
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate

llm = OpenAI(temperature=0.9)
prompt = PromptTemplate(input_variables=["product"], template="What is a good name for a company that makes {product}?")
chain = LLMChain(llm=llm, prompt=prompt)
result = chain.run("colorful socks")
```

### 3.2 Sequential Chains

```python
from langchain.chains import SimpleSequentialChain, SequentialChain
# Simple and sequential chain examples
```

### 3.3 Router Chains

* Use different chains for different input types.
* **Interview Question:** When to use chain types? LLMChain for single-step, Sequential for multi-step, Router for input-based branching.

---

## 4. Memory Systems

```python
from langchain.memory import ConversationBufferMemory, ConversationSummaryMemory
buffer_memory = ConversationBufferMemory(memory_key="chat_history", return_messages=True)
summary_memory = ConversationSummaryMemory(llm=llm, memory_key="chat_history")
```

**Interview Tip:** Choose Buffer for short convos, Window for recent context, Summary for long convos, Summary Buffer for balance.

---

## 5. Document Processing & RAG

```python
from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
loader = PyPDFLoader("document.pdf")
documents = loader.load()
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
docs = text_splitter.split_documents(documents)
```

Vector stores: Chroma, FAISS, Pinecone.

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(docs, embeddings)
```

**Interview Tip:** RAG types – Stuff, Map-Reduce, Refine, Map-Rerank.

---

## 6. Agents and Tools

```python
from langchain.agents import initialize_agent, AgentType
from langchain.memory import ConversationBufferMemory
from langchain.tools import DuckDuckGoSearchRun

llm = OpenAI(temperature=0)
memory = ConversationBufferMemory(memory_key="chat_history")
tools = [DuckDuckGoSearchRun()]
agent = initialize_agent(tools=tools, llm=llm, agent=AgentType.CONVERSATIONAL_REACT_DESCRIPTION, memory=memory, verbose=True)
response = agent.run("Search AI news")
```

**Interview Tip:** Agents use ReAct pattern – Reason -> Act -> Observe.

---

## 7. Best Practices

* Error handling & retries
* Callbacks for monitoring
* Caching (InMemory, SQLite, Redis)
* Input validation & sanitization
* Rate limiting & async ops
* Load balancing for production

---

## 8. Common Interview Q\&A

* **LangChain vs OpenAI API:** LangChain adds abstractions, chains, memory, tools.
* **Context limits:** Use summarization, chunking, map-reduce.
* **Production-ready:** Error handling, caching, monitoring, rate limits.
* **Debugging:** verbose=True, custom callbacks, step-by-step testing.
* **Performance bottlenecks:** LLM API calls, vector similarity search, context length, memory operations.

---

## 9. Code Patterns to Remember

**RAG:** Setup → Load docs → Split → Create vector store → QA chain → Query.
**Conversational Agent:** Setup LLM → Memory → Tools → Initialize agent → Run.

---

## 10. Key Takeaways

* Understand abstractions: LLMs, Prompts, Chains, Memory, Agents
* Know which chain/memory/agent to use
* Production considerations: error handling, caching, monitoring, performance
* Common patterns: RAG, conversational AI, document processing
* Debugging & optimization skills

**Most Important Interview Topics:**

* RAG implementation & chain types
* Memory systems
* Agent patterns & tools
* Production deployment
* Performance optimization
