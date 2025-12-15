# Hi there, I'm Rajeev Ranjan Chaurasia 👋

<p align="left">
  <a href="https://www.linkedin.com/in/rajeev-ranjan-chaurasia/" target="_blank">
    <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/>
  </a>
  <a href="https://rajeev-chaurasia.vercel.app/" target="_blank">
    <img src="https://img.shields.io/badge/Portfolio-3423A6?style=for-the-badge&logo=vercel&logoColor=white" alt="Portfolio"/>
  </a>
  <a href="mailto:rajeevchaurasia.dev@gmail.com">
    <img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail"/>
  </a>
</p>

### Software Engineer | MS in Software Engineering @ SJSU

I am a graduate student with **2.5 years of production experience** building scalable backend systems. Previously at **Tekion Corp**, I helped engineer financial pipelines that processed millions of transactions. 

Currently, I am deep-diving into **Distributed Systems** and **AI**, building projects like **low-latency trading engines** and **real-time telemetry streams**. I am passionate about writing clean, concurrency-safe code and am looking for **Internship / New Grad** opportunities to apply these skills at scale.

---

## 🛠️ Engineering Toolkit

I believe in choosing the right tool for the job. Here is the stack I use to build reliable, high-performance software:

| **Domain** | **Stack & Patterns** |
| :--- | :--- |
| **Distributed Systems** | ![Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=flat-square&logo=apache-kafka&logoColor=white) ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white) ![gRPC](https://img.shields.io/badge/gRPC-244c5a?style=flat-square&logo=grpc&logoColor=white) ![Flink](https://img.shields.io/badge/Apache_Flink-E6526F?style=flat-square&logo=apacheflink&logoColor=white) <br> *Event-Driven Architecture, Pub/Sub, Protobuf* |
| **Core Languages** | ![Java](https://img.shields.io/badge/Java_17-ED8B00?style=flat-square&logo=openjdk&logoColor=white) ![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white) ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) <br> *Object-Oriented Design, Concurrency, Algorithms* |
| **AI & ML Engineering** | ![CrewAI](https://img.shields.io/badge/CrewAI-1A73E8?style=flat-square&logo=google&logoColor=white) ![LangChain](https://img.shields.io/badge/LangChain-FFFFFF?style=flat-square&logo=langchain&logoColor=black) ![VertexAI](https://img.shields.io/badge/Google_Vertex-4285F4?style=flat-square&logo=googlecloud&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI_Vision-412991?style=flat-square&logo=openai&logoColor=white) <br> *Multi-Agent Systems, RAG, LLM Integration* |
| **Data & Storage** | ![Postgres](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white) ![InfluxDB](https://img.shields.io/badge/InfluxDB-22ADF6?style=flat-square&logo=influxdb&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) ![Elastic](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white) <br> *Time-Series, Spatial Indexing, Caching Strategies* |
| **Infrastructure** | ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![CI/CD](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white) ![NeoN3](https://img.shields.io/badge/Neo_N3_Blockchain-00E599?style=flat-square&logo=neo&logoColor=black) |

---

## 🏗️ Featured Projects

### 🏎️ [VoltStream - Real-Time EV Telemetry Engine](https://github.com/rajeev-chaurasia/voltstream)
> **"Processing 10,000 events/second with sub-5ms latency."**

A distributed system designed to ingest and visualize massive sensor data streams from electric vehicle fleets.
* **Architecture:** **Go** workers consume **gRPC** streams and fan-out to **Apache Kafka**.
* **Scale:** Handles 1,000+ concurrent connections using buffered channels and worker pools.
* **Data:** Persists high-velocity data to **InfluxDB** for time-series analysis.
* **Frontend:** **Next.js** dashboard with Server-Sent Events (SSE) for live tracking.

### 📉 [High-Performance Limit Order Book](https://github.com/rajeev-chaurasia/limit-order-book)
> **"Zero-GC, Lock-Free Concurrency."**

A production-grade Central Limit Order Book (CLOB) designed for financial markets, prioritizing microsecond latency over convenience.
* **Performance:** Achieved **0 bytes/op allocation rate** on the hot path using **Object Pooling**.
* **Concurrency:** Implemented **Lock-Free** data structures (StampedLock) and atomic primitives.
* **Optimization:** Custom intrusive linked lists to eliminate JVM object overhead.

### ⛓️ [GigSmartPay - Decentralized Escrow Platform](https://github.com/Escrow-Bros/smart-pay)
> **"Where Blockchain meets Computer Vision."**

A trustless gig-economy platform combining Smart Contracts with AI verification to automate payments.
* **Blockchain:** **Neo N3** Smart Contracts hold funds in escrow until verification.
* **AI Verification:** **GPT-4 Vision** analyzes "Proof of Work" photos to auto-approve releases.
* **Reliability:** Exponential backoff WebSocket handling and startup recovery for pending transactions.
* **Tech:** Python (FastAPI), Solidity/Boa3, Next.js, Docker.

### 🤖 [TravelSwarm - Multi-Agent AI Orchestrator](https://github.com/rajeev-chaurasia/travel-swarm-app)
> **"Orchestrating AI Agents like microservices."**

A distributed planning system where specialized AI agents (Food, Transport, Activity) collaborate to build itineraries.
* **Design:** Decoupled **Spring Boot** (Core) and **Python** (AI) services via **RabbitMQ**.
* **AI:** Uses **CrewAI** and **Google Vertex** to manage hierarchical agent delegation.
* **Validation:** Implemented strict Pydantic models to force LLMs to output structured, valid JSON.

---

## 📊 GitHub Activity

<p align="center">
  <img src="https://github-readme-activity-graph.vercel.app/graph?username=rajeev-chaurasia&theme=tokyo-night&hide_border=true&area=true" alt="Rajeev's Activity Graph" width="100%"/>
</p>

<p align="left">
  <img src="https://github-readme-stats.vercel.app/api?username=rajeev-chaurasia&show_icons=true&theme=tokyonight&hide_border=true&count_private=true" alt="Rajeev's GitHub Stats" height="150"/>
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=rajeev-chaurasia&layout=compact&theme=tokyonight&hide_border=true&langs_count=6" alt="Top Languages" height="150"/>
</p>
