# Pi Coding Agent 沙箱镜像
# 基于 oven/bun 官方镜像（自带 Bun 运行时）
# 用途：隔离环境中运行 Pi 生成代码

FROM oven/bun:1.2

# 安装基础工具
RUN apt-get update && apt-get install -y \
    git \
    curl \
    ca-certificates \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# 在公共路径安装 Pi（避免 bun install -g 写入 /root/ 导致权限问题）
RUN mkdir -p /opt/pi-app && cd /opt/pi-app && \
    bun init -y && \
    bun add @earendil-works/pi-coding-agent && \
    ln -sf /opt/pi-app/node_modules/.bin/pi /usr/local/bin/pi && \
    chmod -R a+rX /opt/pi-app

# 验证安装（root 下）
RUN pi --version && bun --version

# 创建非 root 用户
RUN useradd -m -s /bin/bash pi && \
    echo "pi ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 配置 git
RUN git config --global user.name "Pi Agent" && \
    git config --global user.email "pi@sandbox.local"

USER pi
WORKDIR /home/pi/workspace
RUN mkdir -p /home/pi/.pi/agent

ENTRYPOINT ["pi"]
CMD ["--help"]
