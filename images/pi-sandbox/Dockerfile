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

# 创建非 root 用户
RUN useradd -m -s /bin/bash pi && \
    echo "pi ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 以 pi 用户安装 Pi（bun install -g 会装到 ~/.bun/bin/）
USER pi
RUN bun install -g @earendil-works/pi-coding-agent
ENV PATH="/home/pi/.bun/bin:${PATH}"

# 验证安装
RUN pi --version && bun --version

# 配置 git
RUN git config --global user.name "Pi Agent" && \
    git config --global user.email "pi@sandbox.local"

WORKDIR /home/pi/workspace

RUN mkdir -p /home/pi/.pi/agent

ENTRYPOINT ["pi"]
CMD ["--help"]
