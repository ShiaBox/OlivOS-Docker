FROM python:3.10.15-slim

ENV TZ=Asia/Shanghai
RUN apt-get update && \
    apt-get install -y tzdata git && \ 
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/OlivOS-Team/OlivOS.git /app

WORKDIR /app

RUN pip install --no-cache-dir -r requirements310.txt

CMD ["python3", "main.py"]
