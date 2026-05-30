# Sandbox Images

> NUC 沙箱/容器使用的自定义镜像仓库。GitHub Actions 构建 → tar.gz 下载 → `docker load`。

## 用法

### 下载构建产物

1. 进入 [Actions → Build Sandbox Images](https://github.com/nacooy-dev/sandbox-images/actions)
2. 选择最新成功运行
3. 在 Artifacts 区域下载所需的 `.tar.gz` 文件
4. 传到 NUC/客户机器

### NUC 上加载

```bash
docker load -i pi-sandbox.tar.gz
msb pull pi-sandbox:latest   # 加载到微沙箱缓存
```

### 手动触发构建

在 GitHub Actions 页面点击 **Run workflow**，输入要构建的镜像名（如 `pi-sandbox`），或 `all` 构建全部。

## 镜像列表

| 镜像 | 基础 | 用途 | Dockerfile |
|------|------|------|------------|
| `pi-sandbox` | Alpine 3.21 | Pi Coding Agent 隔离执行环境，代码生成 | `images/pi-sandbox/Dockerfile` |
| `playwright` | - | Playwright 浏览器自动化沙箱 | `images/playwright/Dockerfile` |
| `chromium` | - | Headless Chromium 测试沙箱 | `images/chromium/Dockerfile` |
| `lightpanda` | - | Lightpanda 轻量浏览器沙箱 | `images/lightpanda/Dockerfile` |

## 添加新镜像

1. 在 `images/` 下新建目录，放入 `Dockerfile`
2. 修改 `.github/workflows/build-images.yml` 的 `matrix.image` 列表
3. 提交 PR / push 自动构建

## 客户部署

```bash
# 从 GitHub Actions 下载 .tar.gz
# 拷到 USB 移动硬盘
cp pi-sandbox.tar.gz /Volumes/USB/sandbox-images/

# 目标机器上
docker load -i pi-sandbox.tar.gz
docker tag pi-sandbox:latest pi-sandbox:latest  # tag 确认
```
