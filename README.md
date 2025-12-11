# VisionSeek

## Overview
VisionSeek is a production-grade multimodal search system designed for jewelry e-commerce platforms. It allows users to search for jewelry using screenshots or images. The system relies on a fine-tuned vision-language model with distributed vector search to deliver fast and accurate product recommendations.

## Key Features
- **Visual Search**: Search using screenshots or product images  
- **Multimodal Embeddings**: Fine-tuned Jina CLIP model specialized for jewelry  
- **Distributed Architecture**: Scalable vector database sharding for high-performance search  
- **Cloud-Native Deployment**: Runs on AWS EKS with auto scaling and load balancing  
- **Production Ready**: Designed for reliability, performance, and scalability  

---

## System Architecture

VisionSeek is structured into three major phases.

### 1. Preprocessing Phase (Offline)
Prepares a rich and diverse dataset for model training.

**Data Augmentation**
- Creates multiple variations of product photos  
- Simulates screenshots and lifestyle contexts  

**Dataset Composition**
- Prong variations  
- Stone shapes  
- Jewelry types such as rings, necklaces, earrings, bracelets  
- On-person and lifestyle shots  
- Simulated screenshot inputs  

---

### 2. Fine-tuning Phase (Offline)
Handles specialized model training and embedding generation.

**Base Model**
- Jina CLIP multimodal vision-language model  

**Fine-tuning Strategy**
- Parameter Efficient Fine Tuning  
- Text encoder stays frozen  
- Vision encoder trained for jewelry features  

**Embedding Workflow**
- Generate embeddings for all catalog items  
- Store embeddings across database shards for scalability  

---

### 3. Inference Phase (Runtime)
Real-time search pipeline deployed on AWS.

**Request Flow**
User Request → Amazon ELB → Docker Services → Embedding Generator → Vector DB Shards → Result Aggregator

**Core Components**
- **Amazon ELB**: Routes traffic to inference services  
- **AWS EKS**: Kubernetes for container orchestration  
- **Docker Containers**: Multiple replicas of the inference pipeline  
- **Jina CLIP Service**: Converts images into embedding vectors  
- **Distributed Vector Database**: FAISS or Milvus partitions data across nodes  
- **Result Aggregator**: Merges top results from each shard  

---

## Technology Stack

### Machine Learning
- Jina CLIP for multimodal embeddings  
- FAISS or Milvus for vector search  
- PyTorch for training and inference  

### Infrastructure
- AWS EKS  
- Docker  
- Amazon ELB  
- VPC for isolated networking  

### Orchestration
- Kubernetes HPA for auto scaling  
- CPU and memory based resource management  

---

## Scalability Features

### Horizontal Scaling
- Amazon ELB distributes load  
- Kubernetes HPA increases or decreases replicas  
- Sharded vector DB for parallel querying  

### Vertical Scaling
- Configurable CPU and RAM per pod  
- Kubernetes manages resource usage  

### High Availability
- Multiple replicas for inference services  
- Automatic restarts on failure  
- Secure communication inside VPC  

---

## How It Works

### Search Flow
1. User uploads a screenshot or image  
2. Amazon ELB routes the request to an inference container  
3. Jina CLIP model generates an embedding vector  
4. Embedding sent to all DB shards in parallel  
5. Each shard performs approximate nearest neighbor search  
6. Aggregator merges and re ranks results  
7. Final top K products returned to the user  

---

## Vector Database Sharding Strategy
- Catalog split evenly into shards  
- All shards queried at the same time for low latency  
- Results merged and ranked together  
- New shards can be added as catalog size grows  

---

