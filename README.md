# 🚀 LLM Gateway with Mesh API

A small learning project exploring **Mesh API as an LLM Gateway** and understanding how a single API interface can be used to access and route requests across multiple Large Language Models (LLMs).

The goal of this project is to understand the architecture, features, routing capabilities, and practical usage of an LLM gateway using **Mesh API**.

---

## 📌 What is Mesh API?

**Mesh API** is a unified LLM gateway that provides access to a large number of AI models through a **single OpenAI-compatible API**.

Instead of integrating separately with OpenAI, Anthropic, Google, Meta, Mistral, DeepSeek, Alibaba, and other providers, an application can communicate with Mesh API through one interface.

```text
                    ┌─────────────────────┐
                    │     My Application   │
                    │                     │
                    │  Chat / AI / Agent  │
                    └──────────┬──────────┘
                               │
                               │ API Request
                               ▼
                    ┌─────────────────────┐
                    │      Mesh API       │
                    │    LLM Gateway      │
                    └──────────┬──────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
           ┌────────┐    ┌──────────┐   ┌──────────┐
           │ OpenAI │    │Anthropic │   │  Google  │
           └────────┘    └──────────┘   └──────────┘
                │              │              │
                └──────────────┼──────────────┘
                               │
                               ▼
                         LLM Response
```

---

## 🎯 Objective

The main objectives of this project are:

* Understand the concept of an **LLM Gateway**
* Explore the **Mesh API architecture**
* Learn how multiple LLM providers can be accessed through one API
* Understand model selection and routing
* Experiment with OpenAI-compatible API requests
* Understand fallback and reliability mechanisms
* Explore usage, latency, token and cost information
* Build a foundation for integrating Mesh API into larger AI applications

---

# 🧠 Why Use an LLM Gateway?

Without a gateway, an application may need separate integrations:

```text
Application
    │
    ├── OpenAI API
    │
    ├── Anthropic API
    │
    ├── Google API
    │
    ├── Mistral API
    │
    └── DeepSeek API
```

This can result in:

* Multiple API integrations
* Different authentication mechanisms
* Different request/response formats
* More provider-specific code
* Difficult model switching
* More complicated monitoring

With Mesh API:

```text
                    ┌──────────────┐
                    │ Application  │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   Mesh API   │
                    │   Gateway    │
                    └──────┬───────┘
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
          Provider A    Provider B    Provider C
```

The application communicates with a **single gateway interface**, while the gateway handles communication with the underlying model providers.

---

# ⚙️ Key Features Explored

## 1. Unified API

Mesh API provides a common API interface for interacting with different LLM providers.

The API follows an **OpenAI-compatible interface**, making it easier to integrate with existing applications and SDKs.

For example:

```text
Application
     │
     │ OpenAI-compatible request
     ▼
https://api.meshapi.ai/v1
     │
     ▼
Mesh API
     │
     ├── OpenAI
     ├── Anthropic
     ├── Google
     ├── Meta
     ├── Mistral
     └── Other models
```

---

## 2. Multiple Model Access

One of the major advantages is access to a large model ecosystem through a single API.

Examples include models from:

* OpenAI
* Anthropic
* Google
* Meta
* Mistral
* DeepSeek
* Alibaba
* Other supported providers

This allows an application to experiment with different models without implementing a completely separate integration for every provider.

---

## 3. Model Routing

The gateway can route requests to the selected model.

Conceptually:

```text
                User Request
                     │
                     ▼
              ┌─────────────┐
              │  Mesh API   │
              │   Gateway   │
              └──────┬──────┘
                     │
              Model Selection
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
      Model A      Model B      Model C
        │            │            │
        └────────────┼────────────┘
                     ▼
                 Response
```

This provides flexibility when experimenting with different models for different workloads.

---

## 4. Fallback

A useful gateway feature is **provider/model fallback**.

If the preferred provider becomes unavailable or fails during a request, a configured backup model can be used.

```text
                  Request
                     │
                     ▼
               Primary Model
                     │
                ┌────┴────┐
                │ Success?│
                └────┬────┘
                     │
              ┌──────┴──────┐
              │             │
             YES            NO
              │             │
              ▼             ▼
           Response      Fallback
                           Model
                             │
                             ▼
                          Response
```

This improves application reliability and reduces dependency on a single provider.

Mesh API currently documents automatic fallback to configured backup models when a provider goes down mid-request.

---

# 🔌 API Architecture

The basic architecture explored in this project is:

```text
┌───────────────────────────────┐
│          Client App           │
│                               │
│ Python / Node.js / React /    │
│ Backend / AI Agent            │
└───────────────┬───────────────┘
                │
                │ HTTPS
                │
                ▼
┌───────────────────────────────┐
│          Mesh API             │
│                               │
│       LLM Gateway Layer       │
│                               │
│  • Authentication             │
│  • Model Routing              │
│  • Provider Abstraction       │
│  • Fallback                   │
│  • Usage Tracking             │
└───────────────┬───────────────┘
                │
       ┌────────┼────────┐
       │        │        │
       ▼        ▼        ▼
   OpenAI   Anthropic  Google
       │        │        │
       └────────┼────────┘
                │
                ▼
             Response
```

---

# 🧪 Basic API Request

Because Mesh API exposes an OpenAI-compatible API, an application can send a request using the familiar chat-completions structure.

Example:

```bash
curl https://api.meshapi.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MESH_API_KEY" \
  -d '{
    "model": "MODEL_NAME",
    "messages": [
      {
        "role": "user",
        "content": "Explain what an LLM gateway is."
      }
    ]
  }'
```

The important idea is that the application communicates with **one gateway endpoint** rather than implementing a separate API layer for every provider.

---

# 🔐 Authentication

The Mesh API key should be stored securely as an environment variable.

Example:

```env
MESH_API_KEY=your_api_key_here
```

Do **not** commit the actual API key to GitHub.

Recommended:

```text
.env
```

and add it to:

```text
.gitignore
```

Example:

```gitignore
.env
```

---

# 📊 Observability & Usage

An LLM gateway can also provide useful operational information around model usage.

Important metrics include:

* Model used
* Provider
* Token usage
* Request latency
* Cost
* Request status
* Fallback events

Conceptually:

```text
                  Mesh API
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
        Tokens      Cost      Latency
          │          │          │
          └──────────┼──────────┘
                     ▼
              Usage Analytics
```

This can help developers understand the performance and cost of their AI application.

---

# ⚡ Performance

Using a gateway introduces a small amount of additional network overhead because requests pass through the gateway before reaching the underlying provider.

Mesh API currently states that most requests add approximately **20–40 ms** of overhead, with automatic provider failover designed to happen within roughly **100 ms**.

The actual end-to-end latency still depends heavily on:

* Selected model
* Provider
* Prompt size
* Output length
* Network conditions
* Model processing time

---

# 🛡️ Data & Privacy

According to Mesh API's current documentation, **Zero Data Retention is enabled by default**. Prompts and completions are processed in memory and are not written to its logs; metadata such as model, token count, cost and latency is retained for billing and analytics.

> Always verify the current privacy and retention policy before using sensitive or production data.

---

# 🏗️ Project Architecture

The learning architecture for this project is:

```text
                     ┌──────────────────┐
                     │      User        │
                     └────────┬─────────┘
                              │
                              ▼
                     ┌──────────────────┐
                     │  Client / App    │
                     └────────┬─────────┘
                              │
                              │ API Request
                              ▼
                  ┌────────────────────────┐
                  │       Mesh API         │
                  │      LLM Gateway       │
                  ├────────────────────────┤
                  │ Model Routing          │
                  │ Provider Abstraction   │
                  │ Fallback                │
                  │ Usage / Cost Tracking  │
                  └────────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
         ┌─────────┐      ┌───────────┐    ┌──────────┐
         │ OpenAI  │      │ Anthropic │    │  Google  │
         └─────────┘      └───────────┘    └──────────┘
              │                │                │
              └────────────────┼────────────────┘
                               │
                               ▼
                        Final LLM Response
```

---

# 📚 What I Learned

Through this exploration, I understood:

1. What an **LLM Gateway** is
2. Why applications use gateways instead of directly integrating every LLM provider
3. How a unified API abstracts different providers
4. How model routing works
5. How fallback improves reliability
6. Why OpenAI-compatible APIs simplify integration
7. How token usage, latency and cost can be monitored
8. How gateway architecture can make multi-model applications easier to maintain
9. The importance of keeping API credentials secure
10. How Mesh API can act as an infrastructure layer between an application and multiple LLM providers

---

# 🔮 Future Scope

The next stage of this project can extend the gateway into a more complete AI application by adding:

* Intelligent model selection
* Cost-based routing
* Latency-based routing
* Automatic fallback strategies
* Request logging
* Token/cost dashboards
* Rate limiting
* Caching
* RAG integration
* Tool calling
* Agent workflows
* Streaming responses
* Multi-model evaluation

A possible future architecture:

```text
                         ┌──────────────┐
                         │     User     │
                         └───────┬──────┘
                                 │
                                 ▼
                         ┌──────────────┐
                         │ Application  │
                         └───────┬──────┘
                                 │
                                 ▼
                    ┌────────────────────────┐
                    │       LLM Gateway      │
                    ├────────────────────────┤
                    │ Authentication         │
                    │ Routing Engine         │
                    │ Cost Optimizer         │
                    │ Fallback Manager       │
                    │ Rate Limiter           │
                    │ Observability          │
                    └───────────┬────────────┘
                                │
              ┌─────────────────┼─────────────────┐
              ▼                 ▼                 ▼
           OpenAI           Anthropic          Google
              │                 │                 │
              └─────────────────┼─────────────────┘
                                ▼
                         Selected Response
```

---

# 🛠️ Technology

| Component      | Technology            |
| -------------- | --------------------- |
| LLM Gateway    | Mesh API              |
| API Style      | OpenAI-compatible     |
| Protocol       | HTTPS / REST          |
| Authentication | API Key               |
| LLM Providers  | Multiple              |
| Development    | API-based integration |
| Configuration  | Environment Variables |

---

# 📖 References

* [Mesh API](https://meshapi.ai/)
* [Mesh API Documentation](https://meshapi.ai/)
* [Mesh API Hackathon / Examples](https://hack.meshapi.ai/)

---

# ⚠️ Note

This repository is primarily a **learning and experimentation project** focused on understanding LLM gateway architecture and Mesh API capabilities.

The implementation and supported features may change as Mesh API evolves.

---

## ⭐ Key Takeaway

> **Mesh API acts as an abstraction layer between an application and multiple LLM providers, allowing developers to access, route, monitor and manage LLM requests through a unified API interface.**
