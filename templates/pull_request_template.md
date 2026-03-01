## Описание (Description)
Опишите, какие изменения вносятся этим Pull Request (PR).
- Если это новый промпт: Какую метрику он улучшает? На сколько вырос Confidence Score?
- Если это инфраструктура: Настроен ли `manifest.json` SGR-Kernel?
- Укажите номер задачи (Issue / Epic): Fixes #123

## Обязательный Чек-лист (PR Checklist)
Пожалуйста, убедитесь, что все пункты выполнены перед призывом Reviewer'а (MLOps / Evaluator):

### 🛡️ Безопасность и Данные
- [ ] Любые новые логи и телеметрия проходят через слой **PII очистки (Анонимизация)**.
- [ ] Приватные ключи и API токены не закоммичены (используются Secrets Manager).
- [ ] Модель не имеет доступа к данным, не разрешенным через **Capability Enforcement (ACLs)** SGR-Kernel.

### 💰 Экономика
- [ ] Задан правильный Target Model (`gpt-4`, `llama-3.1-8b`, etc.) для соблюдения баланса "цена/качество".
- [ ] Стоимость инференса на проде оценена и не превышает лимит бюджета (Resource Quotas).

### 🧪 Тестирование и Воспроизводимость
- [ ] `manifest.json` (если изменялся) проверен на корректность сборки в Sandboxed Environment.
- [ ] Unit Baseline тесты проходят локально (`pytest` на Golden Dataset).
- [ ] Опционально: Добавлен `experiment_report.md` в соответствующий Epic.

## Ревью (Reviewers)
- @MLOps_Engineer (Cost / Infrastructure check)
- @LLM_Evaluator (Security / Hallucination Eval)
