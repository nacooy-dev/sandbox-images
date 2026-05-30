# Pi Coding Agent 沙箱镜像 — 轻量版 (Alpine)
# 来源：microsandbox/claude-code 模式
# 用途：隔离环境中运行 Pi 生成代码

FROM alpine:3.21

# 安装基础工具
RUN apk add --no-cache \
    git \
    curl \
    ca-certificates \
    bash

# 安装 Bun（官方安装脚本）
RUN curl -fsSL https://bun.sh/install | bash
ENV PATH="/root/.bun/bin:${PATH}"

# 全局安装 Pi
RUN bun install -g @earendil-works/pi-coding-agent

# 验证安装
RUN pi --version && bun --version

# 创建非 root 用户
RUN adduser -D -s /bin/bash pi

# 配置 git
RUN git config --global user.name "Pi Agent" \
    && git config --global user.email "pi@sandbox.local"

USER pi
WORKDIR /home/pi/workspace

RUN mkdir -p /home/pi/.pi/agent

ENTRYPOINT ["pi"]
CMD ["--help"]
