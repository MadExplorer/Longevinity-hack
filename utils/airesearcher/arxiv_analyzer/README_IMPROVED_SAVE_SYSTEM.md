# 📂 Улучшенная Система Сохранения Отчетов

## 🌟 Обзор

Реализована полностью переработанная система сохранения отчетов и управления состоянием с:

- ✅ **Структурированные каталоги** по датам
- ✅ **Гибкая конфигурация** путей сохранения  
- ✅ **Автоматическое резервное копирование**
- ✅ **Метаданные сохранения** в каждом отчете
- ✅ **Настраиваемые пути** для разных проектов
- ✅ **Очистка старых резервных копий**

## 📁 Структура Каталогов

### Базовая структура (по умолчанию):
```
output/
├── reports/           # Финальные отчеты анализа
│   └── 2025-07-21/   # Структура по датам
│       ├── demo_quick_analysis.json
│       ├── demo_full_analysis.json
│       └── arxiv_analysis_20250721_143022.json
├── state/            # Состояние системы (сессии, кэш)
│   ├── sessions.json
│   ├── analyzed_papers.json
│   ├── queries_cache.json
│   └── rankings_history.json
└── logs/             # Логи (зарезервировано)
    └── 2025-07-21/
```

### С пользовательским каталогом:
```
my_research_output/
├── reports/2025-07-21/
├── state/
└── logs/2025-07-21/
```

## ⚙️ Конфигурация

### Основные настройки в `config.py`:

```python
# Структура директорий
OUTPUT_BASE_DIR = "output"           # Базовая директория
REPORTS_DIR = "reports"              # Подкаталог для отчетов
STATE_DIR = "state"                  # Подкаталог для состояния
LOGS_DIR = "logs"                    # Подкаталог для логов

# Структура по датам
USE_DATE_STRUCTURE = True            # Создавать папки по датам
DATE_FORMAT = "%Y-%m-%d"            # Формат дат: YYYY-MM-DD
TIMESTAMP_FORMAT = "%Y%m%d_%H%M%S"   # Формат временных меток

# Имена файлов
REPORT_FILENAME_TEMPLATE = "arxiv_analysis_{timestamp}.json"
DEMO_QUICK_FILENAME = "demo_quick_analysis.json"
DEMO_FULL_FILENAME = "demo_full_analysis.json"

# Настройки сохранения
SAVE_FULL_RESULTS = False           # Исключать большие данные
BACKUP_OLD_REPORTS = True           # Создавать резервные копии
MAX_BACKUPS = 5                     # Максимум резервных копий
```

### Функции конфигурации:

```python
from config import get_output_paths, create_output_structure

# Получить пути с базовыми настройками
paths = get_output_paths()

# Получить пути с пользовательским каталогом
paths = get_output_paths("my_project")

# Создать структуру каталогов
create_output_structure("my_project")
```

## 🔧 Использование

### 1. Базовое использование:

```python
from main import ArxivAnalyzer

# Стандартные пути (output/)
analyzer = ArxivAnalyzer(enable_state_tracking=True)

# Сохранение с автоматическим именем
results = await analyzer.run_full_analysis()
saved_path = await analyzer.save_results(results)

# Сохранение с пользовательским именем
saved_path = await analyzer.save_results(results, "my_analysis.json")
```

### 2. Пользовательский каталог:

```python
# Использование пользовательского каталога
analyzer = ArxivAnalyzer(
    enable_state_tracking=True, 
    custom_output_dir="longevity_research"
)

results = await analyzer.run_full_analysis()
saved_path = await analyzer.save_results(results)
# Сохранится в: longevity_research/reports/2025-07-21/arxiv_analysis_*.json
```

### 3. Использование demo.py:

```bash
# Стандартная демонстрация
python demo.py --quick

# С пользовательским каталогом
python demo.py --quick --output-dir "my_research"

# Показать структуру файлов
python demo.py --show-structure

# Показать структуру пользовательского каталога
python demo.py --show-structure --output-dir "my_research"
```

## 📋 Метаданные Сохранения

Каждый сохраненный отчет содержит секцию `save_metadata`:

```json
{
  "timestamp": "2025-07-21T14:30:22.123456",
  "statistics": { ... },
  "top_papers": [ ... ],
  "save_metadata": {
    "saved_at": "2025-07-21T14:30:25.456789",
    "save_path": "/full/path/to/report.json",
    "save_config": {
      "full_results_included": false,
      "date_structure_used": true
    },
    "full_results_removed": true
  }
}
```

## 🔄 Резервное Копирование

### Автоматическое создание:
- При перезаписи файла создается резервная копия
- Копии сохраняются в подпапке `backups/`
- Формат имени: `filename_backup_YYYYMMDD_HHMMSS.json`

### Пример структуры:
```
output/reports/2025-07-21/
├── demo_full_analysis.json
└── backups/
    ├── demo_full_analysis_backup_20250721_143022.json
    ├── demo_full_analysis_backup_20250721_144115.json
    └── demo_full_analysis_backup_20250721_145203.json
```

### Автоочистка:
- Автоматически удаляются старые копии свыше `MAX_BACKUPS`
- Сохраняются только последние N копий

## 🧪 Тестирование

### Запуск тестов:

```bash
# Полный тест системы
python test_improved_structure.py

# Тест с очисткой тестовых файлов
python test_improved_structure.py --cleanup

# Тест с сохранением тестовых файлов
python test_improved_structure.py --no-cleanup
```

### Что тестируется:
1. ✅ Создание структуры каталогов
2. ✅ Конфигурации путей (с датами/без дат)
3. ✅ Функциональность сохранения
4. ✅ Резервное копирование
5. ✅ Загрузка конфигурации

## 📊 Команды demo.py

### Новые команды:

| Команда | Описание |
|---------|----------|
| `--show-structure` | Показать структуру выходных файлов |
| `--output-dir DIR` | Использовать пользовательский каталог |
| `--show-progress` | Показать прогресс + структуру файлов |

### Примеры:

```bash
# Показать структуру файлов
python demo.py --show-structure

# Быстрая демо в пользовательский каталог
python demo.py --quick --output-dir "biotech_analysis"

# Показать прогресс определенного проекта
python demo.py --show-progress --output-dir "biotech_analysis"

# Очистить состояние проекта
python demo.py --clear-state --output-dir "biotech_analysis"
```

## 🔧 Миграция со Старой Системы

### Обратная совместимость:
- ✅ Старые вызовы `save_results()` работают
- ✅ StateManager принимает старые пути
- ✅ Существующие файлы не затрагиваются

### Рекомендации:
1. **Постепенный переход**: используйте новый API для новых проектов
2. **Тестирование**: запустите `test_improved_structure.py`
3. **Резервные копии**: важные данные скопируйте перед миграцией

## 🚀 Преимущества

### До:
```
demo_quick_results.json          # В корне проекта
demo_full_results.json           # В корне проекта
analysis_state/                  # Перемешано с отчетами
├── sessions.json
├── analyzed_papers.json
└── ...
```

### После:
```
output/
├── reports/2025-07-21/          # Отчеты по датам
│   ├── demo_quick_analysis.json
│   ├── demo_full_analysis.json
│   └── backups/                 # Автоматические резервные копии
├── state/                       # Четкое разделение
│   ├── sessions.json
│   └── analyzed_papers.json
└── logs/2025-07-21/            # Готово для логирования
```

### Ключевые улучшения:
- 🎯 **Организация**: четкое разделение типов файлов
- 📅 **Структура по датам**: легко находить файлы по времени
- 🔄 **Резервные копии**: защита от потери данных
- ⚙️ **Настраиваемость**: гибкая конфигурация путей
- 📊 **Метаданные**: полная информация о сохранении
- 🧹 **Очистка**: автоматическое управление старыми файлами

## 🤝 Совместимость

### Поддерживаемые операции:
- ✅ `ArxivAnalyzer()` - как раньше
- ✅ `save_results(results)` - как раньше  
- ✅ `save_results(results, "filename.json")` - как раньше
- ✅ Новое: `ArxivAnalyzer(custom_output_dir="custom")`
- ✅ Новое: `save_results(results, "file.json", custom_dir="custom")`

### Обновленные модули:
- ✅ `config.py` - новые настройки
- ✅ `main.py` - улучшенный `save_results()`
- ✅ `demo.py` - новые команды
- ✅ Новый: `test_improved_structure.py` 