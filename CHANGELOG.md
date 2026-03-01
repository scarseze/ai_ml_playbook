# 📋 Changelog (История изменений Playbook)

Все значимые изменения в процессе разработки, ролях, инфраструктуре SGR-Kernel и манифестах документируются здесь.
Формат основан на [Keep a Changelog](https://keepachangelog.com/ru/1.0.0/), проект использует [Semantic Versioning](https://semver.org/lang/ru/).

## [Unreleased]
*Запланировано, но еще не внедрено.*
- Автоматическое тестирование промптов (Promptfoo / PromptBench CI).

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
