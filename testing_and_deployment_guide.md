# 🚀 Environment Setup & Testing Guide
> Руководство по развертыванию платформы SGR-Kernel и интеграции рабочих процессов (Playbook) на новом "железе" (GPU-машина).

Этот документ написан для роли **MLOps Engineer**. Он содержит инструкцию, как взять "чистый" сервер с видеокартами, подготовить его и запустить эталонный процесс тестирования LLM-агентов.

## Шаг 1: Подготовка ВМ (Virtual Machine)
Вам потребуется физический или облачный сервер с ОС Ubuntu (20.04/22.04 LTS) и установленными GPU (например, NVIDIA A100/H100/V100).

1. **Обновление системы и установка базовых утилит:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y git wget curl build-essential python3-venv
   ```

2. **Установка драйверов NVIDIA и CUDA:**
   (Следуйте официальной инструкции Nvidia для вашей карты. Проверка установки: `nvidia-smi`).

3. **Установка Docker и NVIDIA Container Toolkit:**
   SGR-Kernel-R7 требует строгого изолирования окружений (Sandboxed Code Execution).
   ```bash
   # Установка Docker
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh

   # Установка NVIDIA Container Toolkit
   curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo apt-key add -
   curl -s -L https://nvidia.github.io/libnvidia-container/ubuntu22.04/libnvidia-container.list | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
   sudo apt-get update
   sudo apt-get install -y nvidia-container-toolkit
   sudo systemctl restart docker
   ```
   *Проверка:* `sudo docker run --rm --gpus all nvidia/cuda:11.8.0-base-ubuntu22.04 nvidia-smi`

## Шаг 2: Клонирование репозиториев
На сервере необходимо создать структуру, объединяющую Оркестратор и код Playbook'а:

```bash
mkdir ~/ml_platform && cd ~/ml_platform

# Клонирование оркестратора SGR-Kernel
git clone https://github.com/scarseze/SGR-Kernel-R7.git orchestrator

# Клонирование нашего Playbook (ваша структура)
# Замените на реальный репозиторий:
git clone https://github.com/your-org/ai_ml_playbook.git playbook

cd orchestrator
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Шаг 3: Как запустить и протестировать первый эксперимент
Давайте пройдем полный Task-driven цикл: от гипотезы до графиков.

### 1. Роль: AI Product Owner
1. Заходит в Jira/Github и создает таску по шаблону `playbook/templates/task_template.md`. 
   > Цель: Оценить качество RAG-ответчика на локальной модели Llama-3-8B. Бюджет: $5 на тесты.

### 2. Роль: RAG Specialist
1. Подготавливает тестовый промпт на основе `playbook/templates/prompt_template.json` (кладет его в `playbook/prompts/test_llama_v1.json`).

### 3. Роль: Data Engineer
1. Собирает тестовый датасет (Golden Dataset) с вопросами и идеальными ответами (`golden.jsonl`). Прогоняет скрипт PII-очистки. Данные копируются в папку `orchestrator/data/`.

### 4. Роль: MLOps Engineer (ВЫ)
1. Вы создаете манифест эксперимента на основе `playbook/templates/manifest_template.json` и сохраняете его как `orchestrator/experiments/llama_eval.json`.
   В манифесте указываете:
   - Исходный образ: `docker_image: sgr-llm-base:v2.1`
   - Requirements: `"gpu_required": true`
   - Approval gates: `"max_cost_usd_limit": 5.00`

2. **Запуск песочницы (Orchestrator Sandbox)**
   Вы стартуете исполнение из корня SGR-Kernel-R7:
   ```bash
   python main.py --experiment experiments/llama_eval.json
   ```
   *Kernel автоматически:*
   - Поднимет Docker контейнер с пробросом GPU (благодаря NVIDIA toolkit).
   - Запустит `evaluate_rag.py`.
   - Заблокирует доступ к сети (если нет Capability `EXECUTE_HTTP`).
   - Сохранит WAL (Write-Ahead Log), чтобы при падении эксперимент можно было продолжить.

### 5. Роль: LLM Evaluator
1. Результаты бенчмарка (metrics.json) выгружаются из контейнера.
2. Evaluator заполняет `playbook/templates/experiment_report.md` (Метрика Faithfulness выросла на 12%). Отдает на аппрув "Product Owner".

## Устранение неполадок (Troubleshooting)
- **Сборка упала с OOM (Out Of Memory):** Перепишите `manifest.json`, уменьшив `batch_size`. SGR-Kernel отловит ошибку и запишет её в WAL.
- **Модель выдает PII:** Data Engineer должен обновить скрипт регулярных выражений и запустить процесс (Pipeline) заново. Благодаря `manifest.json` эксперимент 100% воспроизводим.
