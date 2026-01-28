## Hi, I'm Tien-Ru (Terri)

I'm a software engineer working across **search systems**, **machine learning infrastructure**, and **full-stack platforms**.

Looking for roles in **SWE** or **MLE**, especially in search, infra, or backend ML applications.


---


## 🎙️ Project: Multilingual Podcast Semantic Search

A full-stack podcast search engine for 147K+ English and Chinese episodes.  
Supports **hybrid retrieval (BM25 + dense vectors)**, **custom RRF ranking**, and **multilingual UI**.

🔗 **Live Demo**: [https://podvein.com](https://podvein.com)



### 🧱 System Overview
```
User ─▶ Frontend (Next.js)
       └─▶ API Server (Spring Boot)
             ├─▶ Search (Elasticsearch)
             └─▶ Embedding (Python)

Offline Pipeline:
Crawler ─▶ Cleaning ─▶ Embedding ─▶ Indexing ─▶ Search
```


### 🧠 Roles

#### SWE
- Designed backend APIs (Spring Boot) and deployed with Docker, Nginx, CI/CD
- Built microservices for crawling, indexing, querying
- Integrated rate limiting and fault handling

📂 Code:  
[API Server](https://github.com/ctienru/podcast-backend) · [Frontend](https://github.com/ctienru/podcast-frontend) 

#### MLE
- Implemented hybrid retrieval with BM25 + dense vectors
- Built custom Reciprocal Rank Fusion (RRF) logic
- Cleaned and embedded podcast text using multilingual heuristics

📂 Code:  
[Search & Embedding](https://github.com/ctienru/podcast-search)


### 🛠️ Stack Summary

| Layer | Tech |
|-------|------|
| Frontend | Next.js, TypeScript |
| Backend | Spring Boot, Java |
| Search/ML | Elasticsearch, Sentence-Transformers, BM25, RRF |
| Infra | Docker, Nginx, GitHub Actions, DigitalOcean |
