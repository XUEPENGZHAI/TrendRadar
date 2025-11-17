# TrendRadar VSCode 配置说明

本目录包含 TrendRadar 子模块的 VSCode 配置，已自动检测并配置所有语言工具链。

## 检测到的技术栈

### Python (主要语言)

- **工具链**: uv (主要) + pip (备选)
- **配置文件**: `pyproject.toml`, `requirements.txt`
- **Python 版本**: 3.10+
- **Linter**: flake8 (max-line-length=120)
- **Formatter**: black (line-length=120)
- **类型检查**: mypy
- **项目类型**: MCP Server (Model Context Protocol)

## 快速开始

### Python 环境设置

#### 方案 1: 使用 uv (主要 - 推荐)

```bash
# 安装 uv
pip install uv
# 或访问: https://docs.astral.sh/uv/getting-started/installation/

# 创建虚拟环境并安装依赖
uv sync

# 激活虚拟环境
source .venv/bin/activate  # Linux/macOS
# 或
.venv\Scripts\activate  # Windows
```

**一键部署脚本** (推荐):

```bash

# Windows
setup-windows.bat

# Mac/Linux
bash setup-mac.sh
```

#### 方案 2: 使用 pip (备选)

```bash
# 创建虚拟环境
python -m venv .venv

# 激活虚拟环境
source .venv/bin/activate  # Linux/macOS
# 或
.venv\Scripts\activate  # Windows

# 安装依赖
pip install -r requirements.txt
```

**切换方式**: 编辑 `settings.json`

- 使用 uv: 保持当前配置 (默认)
- 使用 pip: 注释掉 uv 配置，取消注释 pip 配置部分

### 运行项目

```bash
# 启动 MCP Server
python mcp_server/server.py

# 或使用项目脚本
python main.py

# 或使用 HTTP 服务
bash start-http.sh  # Linux/macOS
start-http.bat      # Windows
```

## 配置说明

### 自动格式化

- **Python**: 保存时自动使用 black 格式化
- **导入排序**: Python 自动组织导入

### 排除的目录

以下目录已配置为隐藏，以减少编辑器负担:

- `__pycache__/`, `.pytest_cache/`, `.mypy_cache/`
- `dist/`, `build/`, `.egg-info/`
- `.venv/`, `venv/`
- `.env` 文件
- `output/` (爬虫输出目录)
- `.push_records/` (推送记录目录)

### 工具链切换

#### Python: uv ↔ pip

编辑 `settings.json` 中的 `python.defaultInterpreterPath`:

##### 使用 uv (默认)

```json
"python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
```

##### 使用 pip

```json
"python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
```

两者虚拟环境路径相同，区别在于创建和安装方式。

## 必需的 VSCode 扩展

### Python (必需)

- **Python** (ms-python.python) - 官方 Python 扩展，包含 Pylance 语言服务器

### 推荐

- **Git Graph** (mhutchie.git-graph) - Git 可视化
- **YAML** (redhat.vscode-yaml) - YAML 语法支持
- **Even Better TOML** (tamasfe.even-better-toml) - TOML 语法支持和格式化

## 项目结构

```text
TrendRadar/
├── main.py                 # 主程序入口
├── mcp_server/             # MCP Server 实现
│   ├── server.py          # 服务器主文件
│   ├── services/          # 业务逻辑服务
│   ├── tools/             # MCP 工具定义
│   └── utils/             # 工具函数
├── config/                # 配置文件
│   ├── config.yaml        # 主配置文件
│   └── frequency_words.txt # 频率词配置
├── docker/                # Docker 相关
├── requirements.txt       # 依赖列表
├── pyproject.toml        # 项目配置
└── .vscode/              # VSCode 配置 (团队共享)
```

## 常见问题

### Q: Python 解释器未找到

A: 确保已创建虚拟环境并激活。检查 `settings.json` 中的 `python.defaultInterpreterPath` 是否正确。

### Q: 导入错误

A: 确保已运行 `pip install -r requirements.txt` 安装所有依赖。

### Q: 格式化不工作

A: 确保已安装 Python 扩展，并在 VSCode 中启用格式化。

### Q: 如何配置爬虫参数

A: 编辑 `config/config.yaml` 文件，配置爬虫间隔、代理、通知渠道等参数。

### Q: 如何添加新的监控平台

A: 在 `config/config.yaml` 的 `platforms` 部分添加新的平台配置。

## 注意事项

1. **.vscode 文件夹已提交到 Git**，用于团队共享配置
2. **环境变量**: 使用 `.env` 文件配置敏感信息，不提交到 Git
3. **配置文件**: `config/config.yaml` 包含所有运行参数，可通过环境变量覆盖
4. **输出目录**: `output/` 目录存储爬虫结果，不提交到 Git
5. **Python 版本**: 推荐 Python 3.10+ (根据 pyproject.toml)

## 关键命令

```bash
# 使用 uv 安装依赖 (推荐)
uv sync

# 或使用 pip 安装依赖
pip install -r requirements.txt

# 启动 MCP Server
python -m mcp_server.server

# 启动主程序
python main.py

# 启动 HTTP 服务
bash start-http.sh  # Linux/macOS
start-http.bat      # Windows

# 查看版本
cat version

# 一键部署 (推荐)
setup-windows.bat   # Windows
bash setup-mac.sh   # Mac/Linux
```

## 环境变量

TrendRadar 支持通过环境变量覆盖配置文件中的设置：

```bash
# 爬虫配置
export ENABLE_CRAWLER=true
export CONFIG_PATH=config/config.yaml

# 通知配置
export FEISHU_WEBHOOK_URL=https://...
export DINGTALK_WEBHOOK_URL=https://...
export WEWORK_WEBHOOK_URL=https://...
export TELEGRAM_BOT_TOKEN=...
export TELEGRAM_CHAT_ID=...
export EMAIL_FROM=...
export EMAIL_PASSWORD=...
export EMAIL_TO=...

# 推送窗口
export PUSH_WINDOW_ENABLED=true
export PUSH_WINDOW_START=08:00
export PUSH_WINDOW_END=22:00
```

---

**最后更新**: 2025-11-17
