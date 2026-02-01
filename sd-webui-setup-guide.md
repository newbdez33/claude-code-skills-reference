# Stable Diffusion WebUI Forge 安装指南 (Windows)

## 当前状态

| 检查项 | 状态 | 备注 |
|--------|------|------|
| SD WebUI 安装 | ✅ 已安装 | C:\SD-WebUI-Forge\ (1.8GB, cu121_torch231) |
| WebUI 运行 | ⏸️ 未运行 | 需手动启动 |
| API 启用 | ✅ 已配置 | --api 参数已添加 |
| Python | ✅ 已安装 | Python 3.10.6 (内置) / 3.12.7 (系统) |
| MCP Server | ⚠️ 待启用 | 配置已创建，需在 Claude Code 中启用 |

**最后检查时间**: 2026-02-01

### 系统信息

- **GPU**: NVIDIA GeForce RTX 4090 (24GB VRAM)
- **PyTorch**: 2.3.1+cu121
- **默认模型**: realisticVisionV51_v51VAE.safetensors

### 安装日志

1. ✅ 下载 webui_forge_cu121_torch231.7z (1.8GB)
2. ✅ 解压到 C:\SD-WebUI-Forge\ (6.4GB 解压后)
3. ✅ 修改 webui-user.bat 添加 --api --listen 参数
4. ✅ 首次启动 WebUI，下载默认模型 (1.99GB)
5. ✅ API 验证通过 (/sdapi/v1/options 返回正常)
6. ✅ 克隆并构建 image-gen-mcp (~/image-gen-mcp)
7. ✅ 配置 MCP Server (~/.claude/mcp-configs/sd-webui.json)
8. ✅ 创建输出目录 (~/Pictures/SD-Output)

---

## MCP Server 配置

**位置**: `~/.claude/mcp-configs/sd-webui.json`

```json
{
  "mcpServers": {
    "image-gen": {
      "command": "node",
      "args": ["~/image-gen-mcp/build/index.js"],
      "env": {
        "SD_WEBUI_URL": "http://127.0.0.1:7860",
        "SD_OUTPUT_DIR": "~/Pictures/SD-Output"
      }
    }
  }
}
```

### 启用 MCP Server

MCP 配置文件已创建，但需要在 Claude Code 中手动启用：

```bash
# 方法 1: 使用 CLI 添加 MCP Server
claude mcp add image-gen node ~/image-gen-mcp/build/index.js \
  -e SD_WEBUI_URL=http://127.0.0.1:7860 \
  -e SD_OUTPUT_DIR=~/Pictures/SD-Output

# 方法 2: 从配置文件导入
claude mcp add-json image-gen "$(cat ~/.claude/mcp-configs/sd-webui.json | jq '.mcpServers[\"image-gen\"]')"
```

验证 MCP 是否启用：
```bash
claude mcp list   # 查看已启用的 MCP servers
/mcp              # 在 Claude Code 会话中查看状态
```

### 使用方法

1. 确保 SD WebUI 正在运行（带 --api 参数）
2. 启用 MCP Server（见上方）
3. 重启 Claude Code 会话以加载 MCP
4. 使用自然语言请求生成图片，例如："生成一张猫的图片"

### MCP 提供的工具

| 工具名 | 功能 | 参数 |
|--------|------|------|
| `generate_image` | 生成图片 | prompt, negative_prompt, steps, width, height, cfg_scale, seed 等 |
| `get_sd_models` | 列出可用模型 | 无 |
| `set_sd_model` | 切换模型 | model_name |
| `get_sd_upscalers` | 列出放大器 | 无 |
| `upscale_images` | 放大图片 | images[], upscaler_1, upscaling_resize 等 |

### 启动 WebUI 命令

```powershell
cd C:\SD-WebUI-Forge\webui
$env:SKIP_VENV=1
$env:PATH = "C:\SD-WebUI-Forge\system\python;C:\SD-WebUI-Forge\system\python\Scripts;C:\SD-WebUI-Forge\system\git\bin;$env:PATH"
C:\SD-WebUI-Forge\system\python\python.exe launch.py --api --listen
```

或者双击 `C:\SD-WebUI-Forge\run.bat`（需要先编辑 webui-user.bat 添加 --api）

---

## 系统要求

- **显卡**: NVIDIA GPU，建议 8GB+ 显存（4GB 可用但较慢）
- **硬盘**: 至少 20GB 可用空间
- **Python**: 3.10.x（安装包会自动处理）

---

## 安装步骤

### 方法 1: 一键安装包（推荐）

1. **下载 Forge WebUI**

   访问: https://github.com/lllyasviel/stable-diffusion-webui-forge/releases

   下载最新的 `webui_forge_cu124_torch24.7z` (NVIDIA CUDA 版本)

2. **解压到目标目录**

   建议解压到: `C:\SD-WebUI-Forge\`

   ```
   C:\SD-WebUI-Forge\
   ├── webui\
   ├── system\
   ├── run.bat
   └── update.bat
   ```

3. **首次运行**

   双击 `run.bat`

   首次启动会自动下载必要组件（约 5-10 分钟）

4. **下载模型**

   将模型文件（.safetensors）放入:
   ```
   C:\SD-WebUI-Forge\webui\models\Stable-diffusion\
   ```

   推荐模型（从 https://civitai.com 下载）:
   - Realistic Vision V5.1
   - DreamShaper XL
   - SDXL Base 1.0

---

### 方法 2: Git 安装（高级用户）

```powershell
# 1. 安装 Python 3.10
winget install Python.Python.3.10

# 2. 克隆仓库
cd C:\
git clone https://github.com/lllyasviel/stable-diffusion-webui-forge.git
cd stable-diffusion-webui-forge

# 3. 运行
.\webui.bat
```

---

## 启用 API 模式（MCP 必需）

编辑 `webui-user.bat`，添加 `--api` 参数:

```bat
set COMMANDLINE_ARGS=--api --listen
```

或者直接运行:

```powershell
.\webui.bat --api --listen
```

---

## 验证安装

1. 启动 WebUI 后，浏览器会自动打开 `http://127.0.0.1:7860`

2. 检查 API 是否可用:
   ```
   http://127.0.0.1:7860/docs
   ```

   应该能看到 API 文档页面

---

## 常见问题

### Q: 显存不足 (Out of Memory)
```bat
set COMMANDLINE_ARGS=--api --listen --medvram
# 或者更低显存:
set COMMANDLINE_ARGS=--api --listen --lowvram
```

### Q: 启动很慢
首次启动需要下载模型和依赖，请耐心等待

### Q: 找不到模型
确保模型文件放在 `models\Stable-diffusion\` 目录

---

## 下一步

安装完成后，运行以下命令启动 WebUI（带 API）:

```powershell
cd C:\SD-WebUI-Forge
.\run.bat --api
```

然后告诉我，我会帮你配置 MCP 连接。

---

## 参考链接

- Forge WebUI: https://github.com/lllyasviel/stable-diffusion-webui-forge
- AUTOMATIC1111: https://github.com/AUTOMATIC1111/stable-diffusion-webui
- 模型下载: https://civitai.com
