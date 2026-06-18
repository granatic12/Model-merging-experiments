# Model merging experiments

Экспериментальное исследование методов слияния моделей (model merging) на примере задач ICT-домена.

## О работе

В работе исследуются методы слияния моделей в сценарии, характерном для крупных ICT-компаний: наличие обновлённой посредством **Continued Pre-Training (CPT)** базовой модели и нескольких специализированных **LoRA-адаптеров** для прикладных задач - детектирования аномалий в логах и генерации ответов на технические вопросы.

**Ключевые исследовательские вопросы:**
- Как влияет слияние специализированных задач на качество в каждой области?
- Какие методы слияния наиболее эффективны?
- Как корректно совмещать CPT и FT модели?


## Структура репозитория

```
Model-merging-experiments /  
│  
├── 1. Подготовка данных /  
│ ├── Подготовка датасета для задачи QA.ipynb  
│ ├── Подготовка датасета для задачи обнаружения аномалий в логах.ipynb  
│ └── Подготовка_датасета_для_CPT.ipynb  
│  
├── 2. Fine-tuning и CPT моделей /  
│ ├── CPT модели.ipynb  
│ ├── Fine-tuning модели на задаче QA.ipynb  
│ └── Fine-tuning модели на задаче обнаружения аномалий в логах.ipynb  
│  
├── 3. Эксперименты слияния моделей /  
│ ├── Слияние_CPT+FT.ipynb  
│ └── Слияние_FT_моделей.ipynb  
│  
└── README.md
```

## Датасеты

### 1. Логи (детектирование аномалий)
**Источник:** [Loghub HDFS_v1](https://github.com/logpai/loghub)
- Логи HDFS (Hadoop Distributed File System)
- Разметка по блок-идентификаторам: OK/Anomaly 
- Используется сбалансированная тестовая выборка

### 2. QA (генерация ответов)
**Источник:** [HuggingFaceTB/github-issues-notebooks](https://huggingface.co/datasets/HuggingFaceTB/issues-kaggle-notebooks)
- Вопросы и обсуждения с технических форумов (Kaggle Notebooks issues)
- Подзадача: `issues`

**Загрузка:**
```python
from datasets import load_dataset
ds = load_dataset("HuggingFaceTB/issues-kaggle-notebooks", "issues")
```

### 3. CPT корпус (ICT-домен)  

Составной корпус из текстов ICT-домена (после фильтрации >50 токенов):

* [Stack Exchange](https://huggingface.co/datasets/ArmelR/stack-exchange-instruction) - технические вопросы и ответы IT-специалистов  
* [3GPP Documents](https://huggingface.co/datasets/dinho1597/3GPP-docs-100cs) - телекоммуникационные стандарты
* [smollm-corpus](https://huggingface.co/datasets/HuggingFaceTB/smollm-corpus) - корпус технических текстов
* [Loghub 1](https://huggingface.co/datasets/vaibhav2507/bgl-logs), [Loghub 2](https://huggingface.co/datasets/Kingslayer5437/BGL) - системные логи (исключая HDFS)

## Информация об экспериментах
### Базовая модель

Qwen3-4B-Instruct-2507 (unsloth/Qwen3-4B-Instruct-2507)

### Метрики оценки

Для задачи логов (бинарная классификация)
* Accuracy   
* Precision  
* Recall  
* F1-score

Для задачи QA (генерация)
* Semantic Similarity (семантическое сходство)
* Term Precision (точность ключевых технических терминов)
* BLEU, ROUGE

### Исследуемые методы слияния

- Task Arithmetic
- TIES-Merging
- DARE
- Sequential
- Weight Averaging (Model soups)



