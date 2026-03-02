# 📋 Changelog (История изменений Playbook)

Все значимые изменения в процессе разработки, ролях, инфраструктуре SGR-Kernel и манифестах документируются здесь.
Формат основан на [Keep a Changelog](https://keepachangelog.com/ru/1.0.0/), проект использует [Semantic Versioning](https://semver.org/lang/ru/).

## [1.1.0] - 2026-03-01
### Added (Добавлено)
- **Multi-Agent Swarm**: Добавлены описания Swarm-пайплайнов в рабочие процессы (`docs/workflows/`).
- **Bilingual Docs**: Все ключевые документы теперь содержат зеркальные секции на русском и английском языках.
- **Model Tier Update**: Актуализированы рекомендации по моделям (Qwen / DeepSeek вместо устаревших GPT-3.5 / Llama для рутинных задач).

## [1.0.0] - 2026-02-25
### Added (Добавлено)
- **Primary Manifest (`AGENTS.md`)**: Создан базовый регламент Task-driven разработки AI-агентов.
- **Роли Команды (`docs/agents/roles/team/`)**:
  - `ai_product_owner.md` (добавлено лицензирование моделей и проверка ROI).
  - `data_engineer.md` (добавлена защита PII и Data Lineage).
  - `rag_specialist.md` (добавлена защита от Prompt Injection).
  - `llm_evaluator.md` (Red Teaming, мониторинг Semantic Drift).
  - `mlops_engineer.md` (управление квотами и инфраструктурой SGR-Kernel).
- **Рабочие процессы (`docs/workflows/`)**:
  - `rag_development_flow.md`
  - `finetuning_flow.md`
  - `evaluation_loop.md` (Установлены правила A/B маршрутизации трафика).
  - `incident_response.md` (Изоляция при галлюцинациях в Prod).
  - `business_metrics_logging.md`
  - `user_feedback_flow.md` (RLHF-сбор "пальцев вниз").
  - `knowledge_base_update.md` (Поддержка векторной базы).
- **Комплаенс (`docs/compliance/`)**:
  - Политика проверки лицензий моделей `model_licenses.md`.
- **Инфраструктура и Руководства по Деплою**:
  - `testing_and_deployment_guide.md` (Ubuntu/Linux).
  - `testing_and_deployment_guide_windows.md` (Windows WSL2 & Docker Desktop).
  - Учебное пособие по быстрому старту `ONBOARDING.md`.
- **Шаблоны (`templates/`)**:
  - `task_template.md`
  - `pull_request_template.md`
  - `manifest_template.json` (внедрено обязательное Quad-Versioning).
  - `experiment_report.md`
- **Знания (`prompts/`)**: Архитектура хранения "Prompts as Code".
- **Справочники**: Глоссарий `Glossary.md` и шпаргалка разработчика `docs/quickref.md`.

---

## 🇺🇸 English

# 📋 Changelog (Playbook History)

All notable changes to the development process, roles, SGR-Kernel infrastructure, and manifests are documented here.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-03-01
### Added
- **Multi-Agent Swarm**: Added descriptions of Swarm pipelines to the workflow documentation (`docs/workflows/`).
- **Bilingual Docs**: All key documents now contain mirrored sections in Russian and English.
- **Model Tier Update**: Updated model recommendations (Qwen / DeepSeek replacing outdated GPT-3.5 / Llama for routine tasks).

## [1.0.0] - 2026-02-25
### Added
- **Primary Manifest (`AGENTS.md`)**: Created the baseline regulation for Task-driven AI agent development.
- **Team Roles (`docs/agents/roles/team/`)**:
  - `ai_product_owner.md` (added model licensing and ROI validation).
  - `data_engineer.md` (added PII protection and Data Lineage).
  - `rag_specialist.md` (added Prompt Injection defense).
  - `llm_evaluator.md` (Red Teaming, Semantic Drift monitoring).
  - `mlops_engineer.md` (quota management and SGR-Kernel infrastructure).
- **Workflows (`docs/workflows/`)**:
  - `rag_development_flow.md`
  - `finetuning_flow.md`
  - `evaluation_loop.md` (Established A/B traffic routing rules).
  - `incident_response.md` (Isolation during hallucinations in Prod).
  - `business_metrics_logging.md`
  - `user_feedback_flow.md` (RLHF "thumbs down" collection).
  - `knowledge_base_update.md` (Vector DB support).
- **Compliance (`docs/compliance/`)**:
  - Model license verification policy `model_licenses.md`.
- **Infrastructure & Deployment Guides**:
  - `testing_and_deployment_guide.md` (Ubuntu/Linux).
  - `testing_and_deployment_guide_windows.md` (Windows WSL2 & Docker Desktop).
  - Quick Start onboarding tutorial `ONBOARDING.md`.
- **Templates (`templates/`)**:
  - `task_template.md`
  - `pull_request_template.md`
  - `manifest_template.json` (mandated Quad-Versioning implementation).
  - `experiment_report.md`
- **Knowledge (`prompts/`)**: "Prompts as Code" storage architecture.
- **References**: Glossary `Glossary.md` and Developer Cheat Sheet `docs/quickref.md`.
