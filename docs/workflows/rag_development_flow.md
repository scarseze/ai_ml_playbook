# 🔄 RAG Development Flow

This document describes the RAG (Retrieval-Augmented Generation) system development pipeline within the SGR-Kernel platform.

## 1. 🧹 Data Collection and Cleaning (Data Engineer)
1. **Source:** Parsing or exporting documents.
2. **PII Removal:** Running data through a PII removal script (Sandboxed).
3. **Storage:** Saving RAW and CLEANED data into buckets (S3), versioning.

## 2. 🧩 Chunking & Embeddings (RAG Specialist)
1. **Chunking Strategy:** Writing a script to break texts into chunks. Fixing chunk size and overlap.
2. **Embedding Model:** Choosing the optimal embeddings model.
3. **Execution:** Initializing a Job inside SGR-Kernel, which runs CLEANED documents through the Embedding Model. The result is uploaded to the vector database (Milvus).

## 3. 📝 Prompt Development (RAG Specialist & MLOps)
1. Creating a base prompt in `templates/prompt_template.json`.
2. Describing the Capability for SGR-Kernel R7. The Agent is given Read-access to the vector base.

## 4. ⚖️ Evaluation (LLM Evaluator & AI Product Owner)
1. Running a `manifest.json` experiment with a test dataset.
2. Calculating metrics: Context Relevance, Answer Faithfulness (using a strong model as a judge).
3. Quality and cost acceptance (AI Product Owner).

## 5. 🚀 Deployment (MLOps)
1. Merging a pull request with the new prompt and weights into `main`.
2. SGR-Kernel-R7 updates the Swarm for Production.

---

# Russian Section / Русская Секция 🇷🇺

# 🔄 RAG Development Flow

Этот документ описывает пайплайн разработки системы RAG (Retrieval-Augmented Generation) в рамках платформы SGR-Kernel.

## 1. 🧹 Сбор и очистка данных (Data Engineer)
1. **Source:** Парсинг или экспорт документов.
2. **PII Removal:** Прогон данных через скрипт очистки от PII (Sandboxed).
3. **Storage:** Сохранение RAW и CLEANED данных в бакеты (S3), версионирование.

## 2. 🧩 Chunking & Embeddings (RAG Specialist)
1. **Chunking Strategy:** Написание скрипта для разбивки текстов на чанки. Фиксация размера чанка и overlap.
2. **Embedding Model:** Выбор оптимальной модели эмбеддингов. 
3. **Execution:** Инициализация Job внутри SGR-Kernel, который прогоняет CLEANED документы через Embedding Model. Результат заливается в векторную базу данных (Milvus).

## 3. 📝 Разработка Промпта (RAG Specialist & MLOps)
1. Создание базового промпта в `templates/prompt_template.json`.
2. Описание Capability для SGR-Kernel R7. Агенту дается Read-доступ к векторной базе.

## 4. ⚖️ Evaluation (LLM Evaluator & AI Product Owner)
1. Прогон `manifest.json` эксперимента с тестовым датасетом.
2. Подсчет метрик: Context Relevance, Answer Faithfulness (используя сильную модель как судью).
3. Приемка качества и стоимости (AI Product Owner).

## 5. 🚀 Deployment (MLOps)
1. Вливание pull request с новым промптом и весами в `main`.
2. SGR-Kernel-R7 обновляет Swarm для Production.
