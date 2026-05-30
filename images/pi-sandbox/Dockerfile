# Pi Coding Agent 沙箱镜像
# 基于 oven/bun 官方镜像（自带 Bun 运行时 + glibc）
# 来源：microsandbox/claude-code 模式
# 用途：隔离环境中运行 Pi 生成代码

FROM oven/bun:1.2

# 安装基础工具
RUN apt-get update && apt-get install -y \
    git \
    curl \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 全局安装 Pi
RUN bun install -g @earendil-works/pi-coding-agent

# 验证安装
RUN pi --version && bun --version

# 创建非 root 用户
RUN useradd -m -s /bin/bash pi

# 配置 git
RUN git config --global user.name "Pi Agent" \
    && git config --global user.email "pi@sandbox.local"

USER pi
WORKDIR /home/pi/workspace

RUN mkdir -p /home/pi/.pi/agent

ENTRYPOINT ["pi"]
CMD ["--help"]
