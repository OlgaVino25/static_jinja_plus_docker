# Базовый образ Ubuntu 24.04
FROM ubuntu:24.04

# Аргументы: версия StaticJinjaPlus и её SHA256
ARG SJP_VERSION
ARG SJP_SHA256

# Отключаем интерактивные запросы
ENV DEBIAN_FRONTEND=noninteractive

# Устанавливаем зависимости
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 \
        python3-venv \
        ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Скачиваем исходники с проверкой контрольной суммы
ADD --checksum=sha256:${SJP_SHA256} https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/${SJP_VERSION}.tar.gz /tmp/sjp.tar.gz
RUN tar -xzf /tmp/sjp.tar.gz -C /opt && \
    mv /opt/StaticJinjaPlus-${SJP_VERSION} /StaticJinjaPlus && \
    rm /tmp/sjp.tar.gz

# Создаём виртуальное окружение и устанавливаем зависимости
WORKDIR /StaticJinjaPlus
RUN python3 -m venv venv && \
    ./venv/bin/pip install --no-cache-dir -r requirements.txt

# Точка входа и команда по умолчанию
ENTRYPOINT ["/StaticJinjaPlus/venv/bin/python", "/StaticJinjaPlus/main.py"]
CMD ["--help"]
