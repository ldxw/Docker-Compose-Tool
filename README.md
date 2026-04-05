## Docker-Compose-Tool 🐳
—— 让你的 Docker 项目拥有专属的“精装修单间”。

本工具的核心目的在于解决 Docker 部署中挂载路径碎片化、管理混乱的痛点。通过规范化的 “一容器一目录” 转换逻辑，将分散的 docker run 命令或原始 Compose 配置，重构为易备份、易迁移的标准化目录结构。

🏗️ 核心哲学：图纸与材料“同屋”存放
在传统的 Docker 部署中，挂载卷（Volumes）往往散落在系统的各个角落（如 /var/lib/docker/volumes 或各种 /etc/xxx）。当你想备份或迁移某个项目时，找齐这些“装修材料”简直是噩梦。

本工具强制推行以下部署规范：

统一大目录：所有 Docker 项目挂载卷统一存放在服务器的一个主目录下（如 /opt/docker）。

容器子目录：以 容器名 为子目录，存放该项目的所有数据。

图纸与材料合一：建议将生成的 docker-compose.yml（图纸）也放在这个子目录下。

这样做的意义：

无损恢复：备份时只需打包这一个子目录。迁移到新服务器后，只需 cd 进入子目录并执行 docker compose up -d，容器即可带着所有数据“无损复活”。

路径清晰：一眼就能看出哪个文件夹对应哪个容器，不再有无主的数据卷。

💎 特色功能：自定义卷映射重构
本工具针对上述哲学，对 Volume 卷映射 进行了深度优化：

自动对齐容器名：工具会自动提取 container_name 作为服务名和目录名，确保生成的挂载路径与你的项目结构高度一致。

绝对路径规范化：自动识别并将各种凌乱的挂载参数转化为标准的 YAML 数组格式。

命名卷（Named Volumes）自动声明：遇到命名卷时，工具会自动在 YAML 末尾补全根级声明，确保配置文件是完整、可直接运行的“终稿”。

路径安全包裹：自动为所有路径添加双引号，防止特殊字符（如空格、@ 等）导致 Compose 无法启动。

🚀 为什么选择它？
生产环境预设：默认勾选 unless-stopped 自动重启逻辑，符合 VPS 长期运维的最佳实践。

极致隐私安全：纯 HTML + JS 静态实现，无后端交互。你的路径信息、API Key、环境变量仅在浏览器本地处理，绝不上传。

零门槛自建：项目仅包含 index.html 和 volumes.js（或 volume-parser.js），无需安装任何运行环境。

🛠️ 使用指南（下载即用）
获取文件：下载本仓库中的 index.html 和相应的 JS 脚本。

本地开启：确保文件同目录，用浏览器打开 index.html。

转换逻辑：

粘贴你从项目文档中看到的 docker run 命令。

根据你的“统一大目录”规划，在生成的配置中确认卷映射。

点击“转换为 Compose 配置”并一键复制。

部署：在你的统一目录下创建同名文件夹，将代码存为 docker-compose.yml，然后执行 up -d。

📄 转换对比示例
输入：
docker run -d --name vaultwarden -v /my_docker_root/vaultwarden/data:/data -p 8080:80 vaultwarden/server:latest

生成的“图纸”：

YAML
version: '3.8'
services:
  vaultwarden:  # 自动同步容器名
    image: vaultwarden/server:latest
    container_name: vaultwarden
    volumes:
      - "/my_docker_root/vaultwarden/data:/data"  # 路径、图纸、材料完美闭环
    ports:
      - "8080:80"
    restart: unless-stopped                   # 预设生产重启策略
📄 开源协议
本项目采用 MIT License 协议开源。

如果您认同这种“一容器一目录”的部署哲学，请点一个 Star ⭐️ 鼓励作者持续优化！
