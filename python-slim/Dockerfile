FROM python:3.12-slim
ARG SJP_VERSION
ARG SJP_SHA256
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
ADD --checksum=sha256:${SJP_SHA256} https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/${SJP_VERSION}.tar.gz /tmp/sjp.tar.gz
RUN tar -xzf /tmp/sjp.tar.gz -C /opt && \
    mv /opt/StaticJinjaPlus-${SJP_VERSION} /StaticJinjaPlus && \
    rm /tmp/sjp.tar.gz
WORKDIR /StaticJinjaPlus
RUN python -m venv venv && ./venv/bin/pip install --no-cache-dir -r requirements.txt
ENTRYPOINT ["/StaticJinjaPlus/venv/bin/python", "/StaticJinjaPlus/main.py"]
CMD ["--help"]
