# 🔄 Fine-Tuning Flow

Процесс тонкой настройки LLM с использованием платформы SGR-Kernel.

## 1. Инициация (AI Product Owner)
- Запрос на дообучение (открывается задача).
- Ожидаемые метрики: Улучшение точности на X%, снижение задержки на Y%.
- Утвержденный финансовый лимит (Cost / Compute Time).

## 2. Подготовка Данных (Data Engineer)
- Сбор размеченных данных из Feedback Loop или синтетических датасетов (RLHF/DPO).
- Форматирование данных в JSONL с парами `prompt/completion` или `messages`.
- PII check и загрузка в S3 с версионированием.

## 3. Настройка Эксперимента (MLOps Engineer)
- Создание `manifest.json` в SGR-Kernel с параметрами обучения (Learning Rate, Batch Size, Epochs, LoRA config).
- Выделение специализированных Node-работников с GPU.
- Изоляция Sandboxed-окружения для предотвращения утечек данных из-за выполнения произвольного кода.

## 4. Запуск и Мониторинг (LLM Evaluator / MLOps)
- Запуск пайплайна обучения через SGR-Kernel.
- Экспорт логов в Weights & Biases / MLflow.
- Мониторинг функции потерь (Loss) и валидационных метрик в реальном времени.

## 5. Валидация модели (LLM Evaluator)
- Развертывание модели (Staging). 
- Проведение бенчмарков, A/B тестов и Red Teaming (проверка на отравление знаний, Backdoors).
- Сравнение с базовой версией по метрикам и стоимости инференса.
- Оформление `experiment_report.md`.

## 6. Релиз (MLOps/Product Owner)
- Утверждение Approval Gate.
- Слияние весов (Merge LoRA weights).
- Регистрация модели в Model Registry (Production).
