FROM python:3.11

# 定义构建参数
ARG OPENAI_API_KEY
ARG OPENAI_API_BASE
ARG OPENAI_BASE_URL

# 设置环境变量
ENV OPENAI_API_KEY=${OPENAI_API_KEY} \
    OPENAI_API_BASE=${OPENAI_API_BASE} \
    OPENAI_BASE_URL=${OPENAI_BASE_URL} \
    PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=0 \
    DISPLAY=:99
    
# 配置apt源并安装所需软件
RUN echo "deb http://mirrors.aliyun.com/debian/ bookworm main non-free contrib\n\
deb-src http://mirrors.aliyun.com/debian/ bookworm main non-free contrib\n\
deb http://mirrors.aliyun.com/debian-security bookworm-security main\n\
deb-src http://mirrors.aliyun.com/debian-security bookworm-security main\n\
deb http://mirrors.aliyun.com/debian/ bookworm-updates main non-free contrib\n\
deb-src http://mirrors.aliyun.com/debian/ bookworm-updates main non-free contrib" > /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libpq-dev \
        libcurl4-openssl-dev \
        libssl-dev \
        poppler-utils \
        xvfb \
        libnss3 \
        libxss1 \
        libgconf-2-4 \
        libasound2 \
        fonts-liberation \
        libappindicator3-1 \
        xdg-utils \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 创建并复制需求文件
COPY requirements.txt .

# 安装 Python 库
RUN pip install --upgrade pip
RUN pip install --no-cache-dir --trusted-host pypi.org --trusted-host files.pythonhosted.org -r requirements.txt

# 安装 Playwright 浏览器
RUN playwright install chromium \
    && playwright install-deps chromium

# 创建启动脚本
COPY start.sh /start.sh
RUN chmod +x /start.sh

# 创建工作目录
WORKDIR /app

# 复制agent_browser.py
COPY agent_browser.py .

ENTRYPOINT ["/start.sh"]
