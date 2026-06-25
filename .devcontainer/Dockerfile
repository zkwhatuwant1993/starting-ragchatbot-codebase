FROM mcr.microsoft.com/devcontainers/python:1-3.13-bookworm

# 替换成国内源
RUN sed -i 's|^URIs: http://deb.debian.org/debian$|URIs: https://mirrors.tuna.tsinghua.edu.cn/debian|g' \
        /etc/apt/sources.list.d/debian.sources && \
    sed -i 's|^URIs: http://deb.debian.org/debian-security$|URIs: https://mirrors.tuna.tsinghua.edu.cn/debian-security|g' \
        /etc/apt/sources.list.d/debian.sources

RUN rm -f /usr/share/keyrings/yarn-archive-keyring.asc \
    && install -d /usr/share/keyrings \
    && curl -fsSL --retry 3 https://dl.yarnpkg.com/debian/pubkey.gpg \
       -o /usr/share/keyrings/yarn-archive-keyring.asc
    
# 3. 确保 yarn.list 使用 signed-by 指向 .asc 文件
RUN echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/yarn-archive-keyring.asc] https://dl.yarnpkg.com/debian/ stable main' \
    > /etc/apt/sources.list.d/yarn.list

# 基础工具安装
RUN apt-get update

# 安装 uv到全局
RUN curl -LsSf https://astral.sh/uv/install.sh | sh && \
    cp /root/.local/bin/uv /usr/local/bin/uv 2>/dev/null || \
    cp /root/.cargo/bin/uv /usr/local/bin/uv 2>/dev/null || true

# 提前创建重要目录已避免vscode用户权限问题
RUN mkdir -p /home/vscode/.cache /home/vscode/.local/bin && \
    chown -R vscode:vscode /home/vscode/.cache /home/vscode/.local

ENV PATH="/home/vscode/.local/bin:${PATH}"