# 环境准备

## 推荐环境

- Windows + PowerShell
- Python 3.11 或 3.12（本机目前只有 3.13，建议另装 3.12 以减少生态兼容摩擦）
- NVIDIA GPU、近期驱动；本机检测为 RTX 3070 Laptop 8 GB
- Git；GitHub CLI（`gh`）用于创建远程仓库与认证

## 创建环境

安装 Python 3.12 后，在仓库根目录运行：

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

先依据 [PyTorch 官方安装选择器](https://pytorch.org/get-started/locally/) 安装与当前驱动匹配的
CUDA 版本，再安装本项目：

```powershell
python -m pip install -e ".[dev,tokenizers]"
```

## 验证

```powershell
python -c "import torch; print(torch.__version__, torch.cuda.is_available(), torch.cuda.get_device_name())"
pytest
ruff check .
```

预期：CUDA 可用、识别到 NVIDIA GPU、测试与静态检查通过。

## 训练产物约定

- 原始/处理后数据放 `data/`；
- checkpoint 放 `out/<experiment>/`；
- 配置和简短实验结论提交到 Git；
- 大文件不要直接提交，确需版本化再评估 Git LFS 或模型托管服务。

