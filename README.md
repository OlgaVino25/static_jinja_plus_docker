# StaticJinjaPlus Docker Port

Этот репозиторий содержит Dockerfile для сборки образов [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus) — генератора статических сайтов на основе Jinja2.

В отличие от форка, здесь нет исходников самого инструмента. При сборке образ скачивает нужную версию непосредственно из официального репозитория. Это позволяет:
- Легко обновляться до новых версий.
- Выбирать базовый образ (Ubuntu или python:slim).
- Собирать как стабильные версии, так и последние изменения из ветки `main`.

Готовые образы доступны на Docker Hub: [olgavino25/static-jinja-plus](https://hub.docker.com/r/olgavino25/static-jinja-plus)

## Структура репозитория

```text
├── ubuntu/
│ ├── Dockerfile            # для сборки стабильных версий на Ubuntu
│ └── Dockerfile.develop    # для сборки последних изменений (ветка main) на Ubuntu
├── python-slim/
│ ├── Dockerfile            # для сборки стабильных версий на python:slim
│ └── Dockerfile.develop    # для сборки последних изменений (ветка main) на python:slim
└── README.md
```


## Требования

- Установленный Docker

## Сборка образов

### Стабильные версии (по тегам)

**Для сборки образа на `Ubuntu` (например, версии `0.1.1`):**

```bash
cd ubuntu
docker build --build-arg SJP_VERSION=0.1.1 -t yourusername/static-jinja-plus:0.1.1-ubuntu .
```

**Для сборки на `python:slim` (той же версии):**

```bash
cd python-slim
docker build --build-arg SJP_VERSION=0.1.1 -t yourusername/static-jinja-plus:0.1.1-slim .
```

Замените `0.1.1` на нужную версию (например, `0.1.0`).

## Примеры использования собранных образов

**Запуск с локальными шаблонами**

```bash
docker run --rm \
  -v $(pwd)/templates:/templates \
  -v $(pwd)/build:/build \
  yourusername/static-jinja-plus:0.1.1-ubuntu \
  --srcpath /templates --outpath /build
```

- Шаблоны должны лежать в папке `templates` (файлы, не начинающиеся с `_` или `.`).
- Результат появится в папке `build`.
- Ассеты (CSS, JS) помещайте в `templates/assets` — они скопируются автоматически.

**Передача переменных окружения в шаблон**

```bash
docker run --rm \
  -e SJP_TITLE="Мой сайт" \
  -v $(pwd)/templates:/templates \
  -v $(pwd)/build:/build \
  yourusername/static-jinja-plus:0.1.1-slim \
  --srcpath /templates --outpath /build
```

В шаблоне переменная доступна как `{{ title }}`.

**Получение справки**

```bash
docker run --rm yourusername/static-jinja-plus:develop-ubuntu --help
```

## Загрузка образов на Docker Hub

После сборки вы можете загрузить образ в свой репозиторий:

```bash
docker push yourusername/static-jinja-plus:0.1.1-ubuntu
```

Для загрузки всех тегов сразу:

```bash
docker push yourusername/static-jinja-plus --all-tags
```

## Почему порт, а не форк?

- Актуальность: новые версии StaticJinjaPlus доступны сразу без ручного обновления.
- Прозрачность: исходный код скачивается напрямую из оригинала, исключая расхождения.
- Гибкость: можно собрать образ для любой версии (включая коммиты), просто указав соответствующий тег.