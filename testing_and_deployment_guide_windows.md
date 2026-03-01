# 🚀 Windows (WSL2) Environment Setup & Testing Guide
> Руководство по развертыванию платформы SGR-Kernel и интеграции рабочих процессов (Playbook) на машине с ОС Windows и видеокартой NVIDIA.

Этот документ написан для **MLOps Engineer**, использующего десктопную Windows для локального тестирования или разработки AI-агентов. Важно: SGR-Kernel требует Linux-контейнеры для изоляции (Sandboxed Execution), поэтому мы используем WSL2.

> [!WARNING]
> **WSL2 не предназначен для Production!**
> Для продакшн-развертывания агентов с высоким уровнем доступа строго рекомендуется использовать нативный Linux. Windows/WSL2 допускается только для локальной разработки и тестирования (Dev/Staging), так как использует виртуализацию и может не обеспечивать нативный уровень изоляции ядра (eBPF, namespaces), на который SGR-Kernel рассчитывает для безопасности песочниц.

## Шаг 1: Подготовка Windows и GPU
Для проброса видеокарты внутрь контейнеров нам нужен WSL2 и Docker Desktop.

1. **Обновление драйверов NVIDIA:**
   Скачайте и установите последние драйверы NVIDIA Game Ready или Studio для вашей видеокарты (GTX/RTX) для Windows. *Дополнительные драйверы Linux внутри WSL ставить НЕ НУЖНО.*

2. **Установка WSL2 (Windows Subsystem for Linux):**
   Откройте PowerShell от имени администратора:
   ```powershell
   wsl --install
   ```
   Перезагрузите компьютер. После перезагрузки настройте пользователя Ubuntu (откроется консоль).

3. **Установка Docker Desktop:**
   Скачайте и установите [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/).
   - В настройках Docker Desktop перейдите в `Settings -> Resources -> WSL Integration`. Убедитесь, что галочка включена для вашего дистрибутива (Ubuntu).

4. **Проверка работы GPU в WSL2:**
   Откройте терминал Ubuntu (WSL) и выполните:
   ```bash
   docker run --rm --gpus all nvidia/cuda:11.8.0-base-ubuntu22.04 nvidia-smi
   ```
   Вы должны увидеть таблицу с вашей Windows-видеокартой прямо из Linux-контейнера.

## Шаг 2: Клонирование репозиториев (Внутри Ubuntu WSL)
Всю разработку и запуск нужно вести **строго внутри файловой системы WSL** для максимальной производительности (не на диске `/mnt/c/`!).

```bash
# Откройте терминал Ubuntu
mkdir ~/ml_platform && cd ~/ml_platform

# Клонирование оркестратора SGR-Kernel
git clone https://github.com/scarseze/SGR-Kernel-R7.git orchestrator

# Клонирование нашего Playbook
git clone https://github.com/your-org/ai_ml_playbook.git playbook

# Настройка виртуального окружения
sudo apt update && sudo apt install -y python3-venv python3-pip
cd orchestrator
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Шаг 3: Как запустить и протестировать первый эксперимент
Повторяет стандартный флоу Playbook.

1. **Создание манифеста (MLOps):**
   Возьмите шаблон `playbook/templates/manifest_template.json` и сохраните его в `orchestrator/experiments/llama_eval.json`. Оставьте `"gpu_required": true`.
2. **Запуск песочницы (Orchestrator Sandbox)**
   Из корня SGR-Kernel-R7 в терминале WSL:
   ```bash
   python main.py --experiment experiments/llama_eval.json
   ```
   *Даже на Windows, SGR-Kernel поднимет изолированный Docker-контейнер и пробросит в него вашу видеокарту для исполнения LLM.*

   **Совет:** Вы можете заранее проверить доступность GPU изнутри Sandbox, добавив в `manifest.json` тестовый шаг с `entrypoint`:
   ```bash
   python -c "import torch; print(f'CUDA Available: {torch.cuda.is_available()}')"
   ```

## Устранение неполадок (Windows Specific)
- **Нет видеопамяти (VRAM OOM):** На Windows (в отличие от серверов Ubuntu) часть видеопамяти съедает отображение рабочего стола и браузер. Если вы запускаете 8B модель на карте с 8GB VRAM — закройте другие программы или уменьшите `batch_size` в `manifest.json`.
- **Медленная работа файловой системы:** Проверьте, что SGR-Kernel клонирован в `~/ml_platform/` (диск WSL ext4), а не в `/mnt/c/Users/` или на "Рабочий Стол". Работать из `/mnt/c/` при ML задачах с тысячами файлов категорически запрещено из-за 10-кратного падения скорости IO.
