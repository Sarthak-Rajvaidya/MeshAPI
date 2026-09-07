# MeshAPI Feature Tour

## Overview

MeshAPI is an **LLM Gateway** that provides a unified API for interacting with multiple AI models and providers.

Instead of integrating separately with different model providers, an application can communicate with MeshAPI through one API.

The overall architecture is:

```text
                    ┌──────────────────────┐
                    │     Application      │
                    │ Python / Backend / UI│
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       MeshAPI        │
                    │     LLM Gateway      │
                    └──────────┬───────────┘
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
        ┌─────────┐       ┌─────────┐       ┌─────────┐
        │ OpenAI  │       │ Mistral │       │ Other   │
        │ Models  │       │ Models  │       │Providers│
        └─────────┘       └─────────┘       └─────────┘



1. Core LLM Features
1.1 Chat Completions

Chat Completions are the basic way to send a conversation to an LLM and receive a response.

A request normally contains messages such as:

system message
user message
assistant message

Example flow:

User Prompt
     │
     ▼
  MeshAPI
     │
     ▼
 Selected LLM
     │
     ▼
 AI Response
Why it is useful

Chat Completions are useful for:

chatbots
question answering
AI assistants
content generation
summarization
general LLM applications
1.2 Streaming

Streaming allows the model response to be received progressively instead of waiting for the complete response.

Without streaming:

User
 │
 ▼
LLM generates entire response
 │
 ▼
Complete response returned

With streaming:

User
 │
 ▼
LLM
 │
 ├── token
 ├── token
 ├── token
 ├── token
 └── ...
       │
       ▼
   User sees response
   being generated
Why it is useful

Streaming improves the perceived responsiveness of applications.

It is especially useful for:

chat applications
AI assistants
long responses
interactive applications
2. Tool Calling & Structured Responses
2.1 Tool / Function Calling

Tool calling allows an LLM to request that the application execute a specific function.

The model does not directly execute the function.

Instead:

User Question
      │
      ▼
     LLM
      │
      │ "I need this tool"
      ▼
Application Tool
      │
      ▼
Tool Result
      │
      ▼
     LLM
      │
      ▼
Final Answer

For example, an application could provide tools such as:

search_database()
get_weather()
search_knowledge_base()
calculate_price()

The LLM decides when a tool is useful and provides the required arguments.

Why it is useful

Tool calling allows an LLM to interact with external systems instead of only generating text.

It is an important building block for:

AI agents
RAG systems
database assistants
automation
API-connected assistants
2.2 Structured Outputs

Structured outputs make the model return information in a predefined structure instead of unrestricted text.

For example:

{
  "name": "Sarthak",
  "age": 21,
  "student": true
}

Instead of receiving:

Sarthak is a 21 year old student.

the application receives predictable fields.

Flow
User Request
     │
     ▼
    LLM
     │
     ▼
Structured Schema
     │
     ▼
Application
Why it is useful

Structured outputs are useful when the application needs to process the model response programmatically.

Examples:

JSON APIs
database insertion
classification
extracting information
agent workflows
3. Model Management
3.1 Model Comparison

MeshAPI allows different models to be compared.

Models can differ in:

price
speed
capabilities
context window
supported features

This helps developers choose an appropriate model for a particular task.

Example:

                Application
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       Model A    Model B    Model C
          │          │          │
          └──────────┼──────────┘
                     ▼
              Compare Results
3.2 Model Discovery

Model discovery provides information about models available through MeshAPI.

The model catalog can be used to determine which models support particular capabilities.

For example:

Model Catalog
     │
     ├── Model name
     ├── Provider
     ├── Pricing
     ├── Context size
     ├── Capabilities
     └── Feature support

This is particularly useful before selecting a model for a production application.

3.3 Auto Router

The Auto Router allows MeshAPI to automatically select an appropriate model instead of the application hardcoding one specific model.

Conceptually:

             User Request
                   │
                   ▼
             MeshAPI Router
                   │
          ┌────────┼────────┐
          ▼        ▼        ▼
       Model A  Model B   Model C
          │        │        │
          └────────┼────────┘
                   ▼
                Response
Why it is useful

It can simplify model selection and allow routing decisions to be handled by the gateway.

Important:

auto does not mean free.

It refers to model routing.

4. API Types
4.1 Responses API

The Responses API provides another interface for interacting with models.

It can support richer interactions than a simple chat-completion request.

Conceptually:

Application
     │
     ▼
Responses API
     │
     ▼
MeshAPI
     │
     ▼
LLM
     │
     ▼
Response

It can be used as an alternative API style depending on the application requirements.

5. Embeddings & RAG
5.1 Embeddings

Embeddings convert text into numerical vectors representing its semantic meaning.

For example:

"Pro plan includes 2TB storage"
                 │
                 ▼
          Embedding Model
                 │
                 ▼
     [0.12, -0.43, 0.77, ...]

Similar meanings produce vectors that are close to each other in vector space.

Why embeddings are useful

They are commonly used for:

semantic search
RAG
document retrieval
recommendation systems
similarity search
5.2 Built-in RAG

MeshAPI provides built-in RAG functionality where documents can be uploaded and searched.

The system handles the underlying retrieval process.

Conceptually:

             Documents
                 │
                 ▼
              Upload
                 │
                 ▼
        Chunking / Embeddings
                 │
                 ▼
          Vector Storage
                 │
                 │
User Question ──┘
       │
       ▼
 Semantic Search
       │
       ▼
Relevant Documents
       │
       ▼
      LLM
       │
       ▼
   Final Answer
Traditional RAG vs Built-in RAG

Traditional RAG:

Application
   │
   ├── Chunk documents
   ├── Generate embeddings
   ├── Manage vector database
   └── Retrieve documents

Built-in RAG:

Application
      │
      ▼
   MeshAPI
      │
      ├── Document processing
      ├── Embeddings
      ├── Storage
      └── Search

This can reduce the amount of infrastructure the application needs to manage.

6. Multimodal Generation
6.1 Image Generation

MeshAPI can provide access to image-generation models through the gateway.

Basic flow:

Text Prompt
     │
     ▼
  MeshAPI
     │
     ▼
Image Generation Model
     │
     ▼
 Generated Image

Image generation can be useful for:

creative applications
prototypes
visual content
AI applications requiring generated images
6.2 Video Generation

MeshAPI also exposes video-generation capabilities.

Conceptually:

Prompt
  │
  ▼
MeshAPI
  │
  ▼
Video Model
  │
  ▼
Generation Task
  │
  ▼
Generated Video

Video generation can be asynchronous, meaning a task is created first and its status can be checked later.

Possible lifecycle:

Task Created
     │
     ▼
 Processing
     │
     ├── Succeeded → Video
     │
     └── Failed
6.3 Text-to-Speech

Text-to-Speech converts written text into spoken audio.

Text
 │
 ▼
MeshAPI
 │
 ▼
TTS Model
 │
 ▼
Audio

It can be used for:

voice assistants
accessibility
narration
audio applications
6.4 Audio Translation

Audio translation allows spoken content to be converted into another language.

Conceptually:

Input Audio
     │
     ▼
MeshAPI
     │
     ▼
Speech / Translation Model
     │
     ▼
Translated Output
6.5 Realtime Speech-to-Speech

Realtime speech-to-speech enables two-way voice interaction.

Instead of sending one complete request and waiting for one complete response, audio can be exchanged continuously.

          User
           │
       Microphone
           │
           ▼
       MeshAPI
           │
           ▼
      Voice Model
           │
           ▼
        Audio
           │
           ▼
        Speaker
           │
           └──────────────► User

This is useful for building realtime voice assistants.

7. Safety & Moderation
7.1 Moderations

Moderation checks content for potentially unsafe material.

MeshAPI evaluates content across multiple safety categories and provides flags and confidence information.

Basic flow:

User Content
     │
     ▼
  Moderation
     │
     ▼
Safety Categories
     │
     ▼
Flags + Confidence

Moderation is a classification/safety layer.

It is different from an LLM that generates the final answer.

It can be applied to:

text
images
8. Reliability & Cost
8.1 Automatic Retry & Fallback

MeshAPI can automatically retry requests and fall back to another provider or similar model when required.

Conceptually:

Application
     │
     ▼
  MeshAPI
     │
     ▼
 Provider A
     │
     ├── Success ──────────────► Response
     │
     └── Failure / Slow
              │
              ▼
           Retry
              │
              ▼
          Provider B
              │
              ▼
           Response

The application does not need to manually implement every fallback path.

This improves reliability.

8.2 Response Caching

Identical deterministic requests can be cached.

For supported requests:

First Request
     │
     ▼
Cache MISS
     │
     ▼
LLM Response
     │
     ▼
Cached

A repeated identical request can then become:

Second Request
     │
     ▼
Cache HIT
     │
     ▼
Cached Response

Caching can improve:

response speed
efficiency
cost

The X-Cache response header can indicate whether a request was served from cache.

8.3 Batch API

The Batch API allows multiple requests to be submitted together as a background job.

It is intended for workloads where immediate responses are not required.

             Multiple Requests
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       Request 1 Request 2 Request 3
          │         │         │
          └─────────┼─────────┘
                    ▼
               Batch Job
                    │
                    ▼
               Processing
                    │
                    ▼
                 Results
Why use Batch API?

Batch processing is useful when:

many requests need to be processed
results are not needed immediately
background processing is acceptable
lower cost is preferred

Not every model supports batching, so the model catalog should be checked for supports_batching.

9. Prompt & Workflow Helpers
9.1 Prompt Templates

Prompt Templates allow reusable prompts to be stored on the server.

Instead of repeatedly sending the complete prompt, the application can reference a stored template and provide the required variables.

Example template:

You are a customer support assistant.

Answer the question about {{topic}}.

Context:
{{context}}

Question:
{{question}}

The application supplies values for:

topic
context
question
Flow
Application
     │
     │ Template ID + Variables
     ▼
  MeshAPI
     │
     ▼
Stored Prompt Template
     │
     ▼
Filled Prompt
     │
     ▼
    LLM
     │
     ▼
  Response
Why it is useful

Prompt Templates provide:

prompt reuse
centralized prompt management
dynamic variables
cleaner application code
easier prompt updates

They are especially useful for applications containing multiple AI workflows or agents.

For example:

Researcher Prompt
Writer Prompt
Critic Prompt

can each be maintained as reusable templates.

10. MeshAPI as an LLM Gateway

All these features can be viewed as parts of the same gateway architecture.

                         APPLICATION
                              │
                              ▼
                    ┌───────────────────┐
                    │      MeshAPI      │
                    │    LLM Gateway    │
                    └─────────┬─────────┘
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ▼                      ▼                      ▼
   LLM Models             Embeddings             RAG
       │                      │                      │
       ▼                      ▼                      ▼
   Chat / Tools          Vector Search          Documents
       │
       ├───────────────┐
       │               │
       ▼               ▼
   Image/Video       Audio
       │               │
       └───────┬───────┘
               ▼
          AI Application

Additional Gateway Capabilities
────────────────────────────────
• Model Discovery
• Model Comparison
• Auto Routing
• Structured Outputs
• Moderation
• Retry / Fallback
• Response Caching
• Batch Processing
• Prompt Templates
11. What We Have Learned

Through this feature tour, the main MeshAPI capabilities covered are:

Category	Features
Core LLM	Chat Completions, Streaming
Agents	Tool / Function Calling
Output Control	Structured Outputs
Models	Model Comparison, Model Discovery, Auto Router
API	Responses API
RAG	Embeddings, Built-in RAG
Generation	Image, Video
Audio	Text-to-Speech, Audio Translation, Realtime Speech-to-Speech
Safety	Moderation
Reliability	Retry, Provider Fallback
Cost / Performance	Response Caching, Batch API
Prompt Management	Prompt Templates
12. What MeshAPI Solves

Without an LLM Gateway, an application may need to integrate separately with multiple AI providers:

Application
   │
   ├────────► Provider A API
   │
   ├────────► Provider B API
   │
   ├────────► Provider C API
   │
   └────────► Provider D API

This creates additional work for:

authentication
API formats
model selection
provider failures
fallback logic
monitoring
cost management

With MeshAPI:

                         ┌──► Provider A
                         │
Application ──► MeshAPI ├──► Provider B
                         │
                         ├──► Provider C
                         │
                         └──► Provider D

The application communicates with one gateway, while MeshAPI handles the underlying model/provider interaction.

13. MeshAPI in Our RAG + Multi-Agent Project

We also used MeshAPI as the foundation for a RAG + Multi-Agent workflow.

Our architecture was:

                         USER
                           │
                           ▼
                    ┌─────────────┐
                    │ Researcher  │
                    │   Agent     │
                    └──────┬──────┘
                           │
                           ▼
                    Knowledge Base
                           │
                           ▼
                       Pinecone
                           │
                           ▼
                    Research Notes
                           │
                           ▼
                    ┌─────────────┐
                    │   Writer    │
                    │   Agent     │
                    └──────┬──────┘
                           │
                           ▼
                         Draft
                           │
                           ▼
                    ┌─────────────┐
                    │   Critic    │
                    │   Agent     │
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    ▼             ▼
                  PASS          REVISE

MeshAPI provided the model access layer for this workflow.

LangChain was used to build the agent workflow, while Pinecone was used as the vector database.

14. Important Features Still Outside This Tour

This feature tour intentionally does not cover every possible MeshAPI capability.

MCP + MeshAPI

MCP integration will be studied separately.

The planned concept is:

AI Agent
    │
    ▼
 MeshAPI
    │
    ▼
 MCP
    │
    ├── Tools
    ├── Resources
    └── External Services

This will be covered later as a separate topic.

Final Understanding

The simplest way to remember MeshAPI is:

MeshAPI is an LLM Gateway that gives an application one interface for accessing multiple AI models and AI capabilities while also providing features such as routing, RAG, tools, moderation, reliability, caching, batching, and prompt management.

The architecture can therefore be remembered as:

                 YOUR APPLICATION
                        │
                        ▼
               ┌─────────────────┐
               │     MeshAPI     │
               │   LLM Gateway   │
               └────────┬────────┘
                        │
       ┌────────────────┼────────────────┐
       │                │                │
       ▼                ▼                ▼
     Models            RAG             Tools
       │                │                │
       ▼                ▼                ▼
    Chat/AI         Embeddings        Agents
       │
       ├──────────────┬──────────────┐
       ▼              ▼              ▼
    Images          Video          Audio

       + Reliability
       + Moderation
       + Caching
       + Batch Processing
       + Prompt Templates
       + Model Routing