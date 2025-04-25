FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y \
    ca-certificates \
    openssl \
    wget \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

RUN wget -q https://github.com/ShiaBox/OlivOS-Docker/raw/refs/heads/main/go-cqhttp/config-example.yml -O config.yml

RUN wget -q https://github.com/ProtocolScience/AstralGocq/releases/download/v1.3.1-pre-8/AstralGocq_linux_amd64.tar.gz && \
    tar -xzf AstralGocq_linux_amd64.tar.gz && \
    rm AstralGocq_linux_amd64.tar.gz && \
    chmod +x AstralGocq

RUN echo '#!/bin/bash\n\
if [ -n "$LOGIN_UIN" ]; then\n\
    sed -i "s/uin: 123/uin: $LOGIN_UIN/" config.yml\n\
fi\n\
if [ -n "$LOGIN_PASSWD" ]; then\n\
    sed -i "s/password: \"\"/password: \"$LOGIN_PASSWD\"/" config.yml\n\
fi\n\
exec ./AstralGocq "$@"' > entrypoint.sh && \
    chmod +x entrypoint.sh

ENV LOGIN_UIN=""
ENV LOGIN_PASSWD=""

ENTRYPOINT ["./entrypoint.sh"]
