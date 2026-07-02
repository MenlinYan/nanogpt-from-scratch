# 7 天路线

默认每天约 4–6 小时。顺序比日期更重要；如果某天卡住，优先保住“正确性 + 能解释”，
不要用复制参考代码换取虚假的进度。

## Day 0 — 工程与基线（当前）

- 初始化仓库、目录、环境、远程与首次提交；
- 写下目标、验收标准和实验记录模板；
- 通读 nanoGPT README，只画模块图，不抄实现。

产出：干净仓库、GPU 环境可验证、公开或私有远程可访问。

## Day 1 — 数据、语言模型与最小生成

- 字符级 tokenizer：`encode/decode`、词表、train/val split；
- batch 构造：输入 `x` 与右移目标 `y`；
- 从 bigram baseline 开始，跑通 loss、反向传播和采样。

验收：在 tiny Shakespeare 或更小文本上过拟合一个 batch；解释每个 tensor shape。

## Day 2 — Attention 与 Transformer block

- 手写 single-head，再扩展 multi-head causal self-attention；
- 实现 embedding、position embedding、LayerNorm、MLP、residual、Transformer block；
- 针对 causal mask、shape、梯度和未来信息泄漏写测试。

验收：不用看代码画出完整数据流；attention 与 PyTorch 参考结果在容差内一致。

## Day 3 — GPT 与完整训练闭环

- 堆叠 blocks，加入 LM head、交叉熵、weight tying、初始化；
- 实现 train/eval、checkpoint、resume、seed、文本采样；
- 配置一个 tiny 模型完成端到端训练。

验收：loss 明显下降；中断后可恢复；同 seed 的短跑结果可复现。

## Day 4 — 优化器与训练策略

- AdamW：理解动量、二阶矩、decoupled weight decay 与参数分组；
- warmup + cosine decay、gradient clipping、gradient accumulation；
- 记录每项策略对 loss 曲线、稳定性和吞吐的影响。

验收：能说明哪些参数不做 weight decay，以及累积梯度如何改变有效 batch size。

## Day 5 — GPU 性能工程

- mixed precision（bf16/fp16）、SDPA/Flash attention、`torch.compile`；
- 测量 tokens/s、step time、峰值显存，避免只凭感觉优化；
- 理解 DDP 的目的与关键机制；单卡项目可做代码阅读，不强求实跑。

验收：形成一张优化前后对比表，并解释速度或显存变化来自哪里。

## Day 6 — GPT-2 对齐、加载与微调

- 对齐 GPT-2 的结构、参数命名与权重转置；
- 加载 GPT-2 small 权重并验证 logits/生成；
- 在小数据集上微调，比较 from-scratch 与 pretrained。

验收：解释 vocab size、context length、Conv1D 权重转置和 weight tying。

## Day 7 — 重构、复盘与闭卷重建

- 对照 nanoGPT 的 `model.py`、`train.py`、数据脚本逐段做差异审查；
- 删除无价值抽象，补测试、README、实验表和故障排查记录；
- 不看实现，用 60–90 分钟重写核心 block 或做口头讲解。

最终产出：可训练、可恢复、可采样、可解释、可复现实验的精简 GPT 仓库。

## 时间不足时的裁剪顺序

可以裁剪：多机 DDP 实跑、复杂数据集、网页 Demo、超参数大规模搜索。

不能裁剪：causal mask 测试、单 batch 过拟合、checkpoint/resume、训练曲线、shape 推导、
优化前后测量、最终闭卷重建。

