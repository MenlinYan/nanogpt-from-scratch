# 使用 uv 管理环境

本项目固定 Python 3.13，使用 `pyproject.toml` 声明依赖，使用 `uv.lock` 锁定精确版本，
虚拟环境位于仓库根目录的 `.venv/`。本机 GPU 为 RTX 3070 Laptop 8 GB，PyTorch 在 Windows
与 Linux 上固定从官方 CUDA 12.8 索引安装。

## 第一次安装

uv 会优先使用 `.python-version` 指定的 Python（缺失时可自动下载），并创建 `.venv`：

```powershell
uv sync --extra tokenizers
```

如果 uv 提示缓存与项目位于不同文件系统、无法创建 hardlink，可改用复制模式；这只影响安装方式，
不影响依赖内容：

```powershell
uv sync --extra tokenizers --link-mode copy
```

开发依赖组 `dev` 默认会安装，因此 pytest、coverage 和 Ruff 无需另装。`tokenizers` 是可选依赖，
这里显式启用以安装 `tiktoken`。

通常不必激活虚拟环境，直接让 uv 在正确环境中执行命令：

```powershell
uv run python --version
uv run pytest
uv run ruff check .
```

如果希望像传统 venv 一样激活：

```powershell
.\.venv\Scripts\Activate.ps1
```

退出用 `deactivate`。

## 常用命令

```powershell
# 安装/同步锁文件中的依赖
uv sync --extra tokenizers

# 在项目环境运行 Python、脚本或工具
uv run python
uv run python scripts/train.py
uv run pytest

# 添加、删除运行时依赖（会更新 pyproject.toml 和 uv.lock）
uv add <package>
uv remove <package>

# 添加开发依赖
uv add --dev <package>

# 查看依赖树
uv tree

# 检查锁文件是否与 pyproject.toml 一致
uv lock --check

# 升级所有依赖，或只升级一个包
uv lock --upgrade
uv lock --upgrade-package <package>
```

尖括号内容是占位符，不要原样执行。例如添加可视化依赖应写成 `uv add matplotlib`。

团队协作或拉取仓库后优先运行 `uv sync --extra tokenizers`，不要手工 `pip install`。`uv run`
会在执行前自动检查锁文件和环境是否同步；CI 中可用 `uv run --locked ...` 禁止隐式改锁文件。

## GPU 验证

```powershell
uv run python -c "import torch; print(torch.__version__); print(torch.cuda.is_available()); print(torch.cuda.get_device_name())"
```

预期：CUDA 为 `True`，设备名包含 `NVIDIA GeForce RTX 3070`。

## 训练产物约定

- 原始/处理后数据放 `data/`；
- checkpoint 放 `out/<experiment>/`；
- 配置、`uv.lock` 和简短实验结论提交到 Git；
- 大文件不要直接提交，确需版本化再评估 Git LFS 或模型托管服务。
