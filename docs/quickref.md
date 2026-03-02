# Quick Reference Card (Шпаргалка разработчика)
> Одностраничный гайд по повседневному Task-Driven Workflow в SGR-Kernel.

Держи этот листок открытым при разработке новых фичей или обновлениях RAG пайплайнов.

---

### 1. Получение Задачи 
- Убедись, что задача (Issue) описана по [task_template.md](../templates/task_template.md).
- В ней должны быть: **Бизнес-цель**, **Ограничение бюджета** и **Метрики приемки**.

### 2. Разработка Кода и Промпта (Local)
- Обнови код агента или промпт (`prompts/your_folder/prompt_vX.json`).
- Строго соблюдай правила "Prompts as Code". Не тестируй промпт в чужих Playground'ах, если там могут быть PII (данные клиентов).

### 3. Обновление Манифеста (The Contract)
- Открой (или создай) `manifest.json` эксперимента в репозитории SGR-Kernel.
- **Quad-Versioning check**: Зафиксированы ли там хэши Dataset-а, Модели, Промпта и Кода? (Чтобы если скрипт упадет, его смог переоткрыть MLOps на другом ПК).
- Убедись, что указаны нужные `capabilities_acl` (Например, `READ_VECTOR_DB_STAGING`). Запрос лишних прав заблокирует PR.

### 4.Запуск Песочницы (Run Sandbox)
Запускай прогон эксперимента через SGR-Kernel локально или на Dev-сервере в терминале (Docker поднимется автоматически):
```bash
python main.py --experiment /path/to/your/manifest.json
```
- Заметил OOM (Out of Memory)? -> Уменьши `batch_size`.
- Ядро упало или ты нажал Ctrl+C? -> Не бойся, **WAL** всё сохранил, запусти ту же команду еще раз, процесс продолжится с того же места.

### 5. Оценка Промптов (Prompt Evaluation)
- Перед пушем изменений прогони локальные тесты производительности в папке `evals/`:
  ```bash
  npx promptfoo eval
  npx promptfoo view
  ```
- Дождись завершения генерации метрик.
- Заполни [experiment_report.md](../templates/experiment_report.md) и прикрепи туда **Run ID** (Track ID) испытания.
- Передай отчет в роль **LLM Evaluator** и **Product Owner** на ревью. Выросли ли метрики качества? Не превышен ли бюджет? 

### 6. Pull Request (PR)
- Открой ветку, отметь чеклист по [pull_request_template.md](../templates/pull_request_template.md) (Очистка PII, Cost Check, Manifest Check).
- Жди аппрува от **MLOps Engineer**.

### 7. Деплой (Deployment)
- После мержа в `main`, SGR-Kernel (в Prod контуре) обновляет Swarm конфигурацию агентов.
- (Опционально) MLOps настраивает A/B роутинг — 10% трафика идёт на твой свежий промпт. 
- Наблюдаем за графиками Thumbs Up/Down и P95 Latency.

---

> **Не знаешь термин?** → [Glossary.md](../Glossary.md)

---

## English Version / Английская версия

> One-page guide to the daily Task-Driven Workflow in SGR-Kernel.

### 1. Get a Task
- Make sure the issue is described using [task_template.md](../templates/task_template.md).
- It must include: **Business goal**, **Budget cap**, and **Acceptance metrics**.

### 2. Develop Code & Prompt (Local)
- Update the agent code or prompt (`prompts/your_folder/prompt_vX.json`).
- Follow "Prompts as Code" rules — never test prompts in third-party Playgrounds if they may contain PII (customer data).

### 3. Update the Manifest (The Contract)
- Open (or create) the `manifest.json` for your experiment in the SGR-Kernel repo.
- **Quad-Versioning check**: Are the Dataset, Model, Prompt, and Code hashes locked? (So MLOps can reproduce a failed run on any machine.)
- Verify `capabilities_acl` is correct (e.g., `READ_VECTOR_DB_STAGING`). Over-requesting permissions will block your PR.

### 4. Run Sandbox
Run the experiment through SGR-Kernel locally or on a Dev server:
```bash
python main.py --experiment /path/to/your/manifest.json
```
- OOM (Out of Memory)? → Reduce `batch_size`.
- Kernel crashed or Ctrl+C? → No worries, **WAL** saved everything. Re-run the same command to resume from the same point.

### 5. Prompt Evaluation
- Before pushing your changes, run local prompt evaluations in the `evals/` folder:
  ```bash
  npx promptfoo eval
  npx promptfoo view
  ```
- Wait for metrics generation to complete.
- Fill in [experiment_report.md](../templates/experiment_report.md) and attach the **Run ID** (Track ID).
- Submit report to **LLM Evaluator** and **Product Owner** for review.

### 6. Pull Request
- Open a branch, fill the checklist from [pull_request_template.md](../templates/pull_request_template.md) (PII cleanup, Cost Check, Manifest Check).
- Wait for **MLOps Engineer** approval.

### 7. Deploy
- After merging to `main`, SGR-Kernel (in Prod) updates the Swarm agent configuration.
- (Optional) MLOps sets up A/B routing — 10% of traffic goes to your new prompt.
- Monitor Thumbs Up/Down and P95 Latency charts.

---

> **Unfamiliar term?** → [Glossary.md](../Glossary.md)
