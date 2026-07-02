# nanoGPT from scratch

一个以“亲手实现、跑通、解释清楚”为目标的 7 天 GPT 工程学习项目。参考
[Karpathy/nanoGPT](https://github.com/karpathy/nanoGPT)，但核心实现不复制粘贴：先独立完成，
再逐段对照原版做差异分析和优化。

## 项目目标

一周后，我应当能够：

- 从张量形状出发实现 causal self-attention、MLP、Transformer block 和 GPT；
- 解释 tokenization、next-token prediction、交叉熵与自回归生成；
- 独立完成数据准备、训练、验证、checkpoint、恢复训练和采样；
- 使用并解释常见工程优化：AdamW 参数分组、学习率 warmup + cosine decay、梯度累积、
  gradient clipping、mixed precision、Flash/SDPA attention、`torch.compile`；
- 定位 loss 异常、过拟合、显存溢出、吞吐不足和不可复现等常见问题；
- 在小语料上从零训练，在 GPT-2 权重上完成加载或微调，并说明两条路径的差异。

详细验收口径见 [docs/acceptance.md](docs/acceptance.md)，逐日安排见
[docs/roadmap.md](docs/roadmap.md)。

## 设计原则

1. **核心代码自己写。** 每个模块先写公式、shape 和测试，再看参考实现。
2. **保持小而透明。** 不引入 Trainer 框架；训练主路径可以从头读到尾。
3. **先正确，再快。** 先用极小数据过拟合，再做性能优化；每次优化都记录速度和显存。
4. **配置与产物分离。** 配置进 Git；数据、权重和日志不进 Git。
5. **可证明地学会。** “能运行”只是中途站，最终要能脱离代码解释和重建。

## 目录规划

```text
configs/              # 可复现的实验配置
data/                 # 本地数据（Git 仅保留目录）
docs/                 # 路线、验收标准、实验记录
scripts/              # 数据准备、训练、采样入口
src/nanogpt/          # tokenizer / model / optimizer / trainer 核心实现
tests/                # shape、mask、梯度、过拟合与回归测试
```

当前是 **Phase 0：工程初始化**。后续实现文件会按路线逐步加入，而不是提前塞入一份看似完成、
实际没有经过自己推导的答案。

## 环境建议

本机已检测到 NVIDIA RTX 3070 Laptop（8 GB）。项目使用 uv 管理 Python 3.13、虚拟环境、
依赖与锁文件；按 [docs/setup.md](docs/setup.md) 安装与验证。8 GB 显存足以完成字符级小模型、
GPT-2 small 级别的受限实验和优化对比，但不以复现 GPT-2 预训练成本为目标。

## 工作方式

每个里程碑都遵循：

```text
推导 -> 最小实现 -> 单元测试 -> 小数据过拟合 -> 对照 nanoGPT -> 优化 -> 记录结论
```

进度使用 [docs/checklist.md](docs/checklist.md) 跟踪。实验结论写入 `docs/experiments/`，
不要只留在终端历史里。

## 致谢与边界

本项目受 Andrej Karpathy 的 nanoGPT、minGPT 与 Neural Networks: Zero to Hero 启发。
参考代码应在实验记录中注明出处；本仓库的学习实现采用 MIT License。
