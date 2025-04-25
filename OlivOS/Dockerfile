FROM python:3.10.15-slim

# 设置中国时区
ENV TZ=Asia/Shanghai
RUN apt-get update && \
    apt-get install -y tzdata git wget && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    rm -rf /var/lib/apt/lists/*

# 初始化应用目录
WORKDIR /app
RUN git clone https://github.com/OlivOS-Team/OlivOS.git . && \
    pip install --no-cache-dir -r requirements310.txt

# 下载核心插件
RUN mkdir -p plugin/app && \
    wget -q -P plugin/app \
    https://github.com/OlivOS-Team/OlivaDiceCore/releases/latest/download/OlivaDiceCore.opk \
    https://github.com/OlivOS-Team/OlivaDiceJoy/releases/latest/download/OlivaDiceJoy.opk \
    https://github.com/OlivOS-Team/OlivaDiceLogger/releases/latest/download/OlivaDiceLogger.opk \
    https://github.com/OlivOS-Team/OlivaDiceMaster/releases/latest/download/OlivaDiceMaster.opk \
    https://github.com/OlivOS-Team/ChanceCustom/releases/latest/download/ChanceCustom.opk \
    https://github.com/OlivOS-Team/OlivaDiceOdyssey/releases/latest/download/OlivaDiceOdyssey.opk

# 创建备份目录
RUN cp -r /app /opt/backup-app

# 创建脚本
RUN echo '#!/bin/bash\n\
# 源代码修复\n\
if [ ! -f "/app/main.py" ]; then\n\
    echo "Restoring core files from backup..."\n\
    cp -rf /opt/backup-app/* /app/\n\
else\n\
    for file in main.py requirements310.txt; do\n\
        if [ ! -f "/app/${file}" ]; then\n\
            cp -f "/opt/backup-app/${file}" "/app/${file}"\n\
        fi\n\
    done\n\
fi\n\
\n\
# 插件文件恢复\n\
PLUGIN_FILES=(\n\
    "OlivaDiceCore.opk"\n\
    "OlivaDiceJoy.opk"\n\
    "OlivaDiceLogger.opk"\n\
    "OlivaDiceMaster.opk"\n\
    "ChanceCustom.opk"\n\
    "OlivaDiceOdyssey.opk"\n\
)\n\
mkdir -p /app/plugin/app\n\
for plugin in "${PLUGIN_FILES[@]}"; do\n\
    if [ ! -f "/app/plugin/app/${plugin}" ]; then\n\
        echo "Restoring missing plugin: ${plugin}"\n\
        cp -f "/opt/backup-app/plugin/app/${plugin}" "/app/plugin/app/"\n\
    fi\n\
done\n\
\n\
exec python3 main.py "$@"' > /usr/local/bin/olivos-entrypoint && \
    chmod +x /usr/local/bin/olivos-entrypoint

# 设置工作目录和入口点
WORKDIR /app
ENTRYPOINT ["olivos-entrypoint"]
