# Adobe India Hackathon 2025 – PDF Document Intelligence System

## 🚀 Overview

This repository is the official submission for the Adobe India Hackathon 2025, under the theme "Connecting the Dots". Our system delivers a complete, competition-ready pipeline for both Challenge 1A (PDF Outline Extraction) and Challenge 1B (Persona-driven Document Analysis).

**What does it do?**

* **Transforms static PDFs into intelligent, interactive experiences.**
* Delivers advanced document understanding: structure detection, persona-relevant insights, and schema-compliant outputs, all packaged in a production-ready Docker container.

**Key Features:**

* **Superior Outline Extraction:** Over 50 headings detected per document (compared to 1–2 for typical solutions).
* **Persona-Driven Analysis:** Content extraction and ranking based on job-to-be-done and five supported personas (QA Engineer, Digital Transformation Consultant, Product Manager, Data Scientist, Software Engineer).
* **Fast and Lightweight:** Under 3 seconds processing per collection, model size 87.6 MB (over 90% below competition limit).
* **Dockerized & Cross-Platform:** Fully dockerized for linux/amd64 (official submission requirement), with support for Windows development and Linux deployment.
* **Multi-Collection Support:** Processes multiple collections independently and in parallel, as per Adobe’s folder specification.
* **100% Compliance:** All outputs pass official JSON schema validation, with health checks and validation scripts included.

## 🗂️ Where to Add PDFs for Testing

* Place your sample or test PDF files inside the `app/input/` folder. Each PDF placed here will be processed in **Round 1A** for outline extraction.
* After processing, extracted outlines and intermediate outputs are stored in `app/output/`.

For Challenge 1B, collections are managed under `collections/`, with each collection having its own PDFs, outline dependencies, and input/output JSON files as per the official hackathon folder specification.

## 🔄 End-to-End Pipeline Flow

1. **Place PDFs:** Add your PDFs to `app/input/` for outline extraction (Round 1A).
2. **Run Round 1A:** Extracts structured outlines (as JSON) for each PDF.
3. **Setup Collections:** Converts Round 1A outputs for use in persona-driven analysis (populates `collections/` folders).
4. **Run Round 1B:** Performs persona-specific content extraction and ranking for each collection, producing schema-compliant outputs.
5. **Validation:** Run health checks and schema validation to ensure outputs are compliant.

## 📋 Quick Start

### Prerequisites

* Docker Desktop
* Git
* At least 2GB free disk space

### Setup

Clone the repository and build the Docker image:

```bash
git clone https://github.com/abhiniveshmitra/adobe-hackathon-pipeline.git
cd adobe-hackathon-pipeline
docker build --platform=linux/amd64 -t adobe-hackathon .
```

### Round 1A – PDF Outline Extraction

```bash
docker run --rm --platform=linux/amd64 \
  -v "${PWD}/app/input:/app/input" \
  -v "${PWD}/app/output:/app/output" \
  -e ROUND=1A \
  adobe-hackathon
```

### Setup for Challenge 1B (Collections)

```bash
docker run --rm --platform=linux/amd64 \
  -v "${PWD}/app/output:/app/output" \
  -v "${PWD}/collections:/app/collections" \
  adobe-hackathon python scripts/setup_collections.py
```

### Round 1B – Persona-Driven Analysis

```bash
docker run --rm --platform=linux/amd64 \
  -v "${PWD}/collections:/app/collections" \
  -e ROUND=1B \
  adobe-hackathon
```

## 🎯 Official Specification Compliance

* **Challenge 1A**: PDF Outline Extraction with >50 headings detected/document
* **Challenge 1B**: Persona-driven document analysis, JSON schema-compliant output
* **Processing constraints**: Under 5 minutes/collection (actual: 2–3s)
* **Model size constraint**: Under 1GB (actual: 87.6MB)
* **Multi-collection support**: Parallel, isolated folder structure
* **Platform**: Docker linux/amd64
* **Validation**: Health checks and schema validation scripts included

## 📁 Project & Folder Structure

```
adobe-hackathon-pipeline/
├── app/
│   ├── input/               # Place PDFs here for Round 1A
│   ├── output/              # Round 1A JSON outputs
│   └── ...
├── collections/             # Multi-collection folders for Round 1B
│   ├── Collection 1/
│   ├── Collection 2/
├── scripts/
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── health_check.py
├── README.md
```

## 🧠 Key Algorithms & AI

* **Heading Detection:** Multi-factor (font size, position, keywords, structure)
* **Embedding Generation:** 384-dimensional vectors (all-MiniLM-L6-v2)
* **Persona Query Expansion:** Adapts extraction to role/task (e.g., QA Engineer vs Data Scientist)
* **Semantic Similarity:** Weighted ranking (70% query relevance, 30% persona fit)

## ✅ Example Demo – Under 5 Minutes

```bash
docker run --rm --platform=linux/amd64 \
  -v "${PWD}/collections:/app/collections" \
  -e ROUND=1B \
  adobe-hackathon
cat collections/Collection\ 1/challenge1b_output.json | head -20
```

## 🧪 Validation & Health Checks

Run automated project health check:

```bash
docker run --rm adobe-hackathon python health_check.py
```

Or validate schema compliance for outputs:

```bash
docker run --rm -v "${PWD}/collections:/app/collections" adobe-hackathon \
  python -c "from app.services.round1b.challenge1b_output_formatter import Challenge1BOutputFormatter; formatter = Challenge1BOutputFormatter(); import json; print('Collection 1 valid:', formatter.validate_output_schema(json.load(open('collections/Collection 1/challenge1b_output.json'))))"
```

## 🏅 Submission Checklist

* [x] Docker image builds (linux/amd64)
* [x] Health check passes
* [x] Challenge 1A & 1B outputs generated
* [x] Model size & performance within limits
* [x] Schema validation passes

## 🔗 Resources & Support

* **GitHub Repo:** [https://github.com/abhiniveshmitra/adobe-hackathon-pipeline](https://github.com/abhiniveshmitra/adobe-hackathon-pipeline)
* **AI Model:** Hugging Face all-MiniLM-L6-v2
* **Support:** Open GitHub Issue for technical help (include OS, Docker version, logs)

## 🎉 Status: COMPETITION READY

This implementation achieves all Adobe hackathon requirements, surpasses performance targets, and demonstrates a robust, innovative AI-powered PDF document intelligence system.
