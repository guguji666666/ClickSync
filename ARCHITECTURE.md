# ClickSync Architecture

## 文件职责

- `index.html`：主页面入口，加载样式、协议脚本和应用脚本。
- `assets/`：页面样式、图片和静态资源。
- `src/core/`：设备识别、连接流程、协议加载和主应用运行逻辑。
- `src/refactor/`：标准键、设备 profile、UI 渲染与能力开关。
- `src/protocols/`：不同品牌鼠标的 WebHID 协议实现。
- `src/tools/`：双击检测、轮询率检测、灵敏度匹配、角度校准等工具页逻辑。
- `Dockerfile`：使用 nginx 镜像托管仓库里的静态文件。
- `docker-compose.yml`：使用 Docker Hub 镜像 `guguji666/clicksync:latest` 启动 `clicksync-web` 服务，并把容器 80 端口映射到本机 18000。
- `.dockerignore`：排除 Git 元数据和本地记录文件，减少 Docker 构建上下文。

## 运行关系

浏览器访问 `http://localhost:18000/index.html`，请求先进入 Docker Compose 启动的 nginx 容器，再由 nginx 返回镜像里的静态文件。WebHID 授权和设备通信仍发生在用户本机浏览器里，容器只负责提供网页。

## 关键决定

- 项目是纯静态前端，没有构建步骤，所以 Docker 不引入 Node/Python 构建流程。
- Compose 使用已推送的 Docker Hub 镜像，方便不构建直接运行。
- 使用 `localhost` 访问，满足 WebHID 对安全上下文的要求。
