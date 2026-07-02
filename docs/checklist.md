# 执行清单

| 阶段 | 状态 | 证据 |
|---|---|---|
| Phase 0：仓库与工程准备 | 已完成 | uv/CUDA 验证通过；GitHub `main` 已推送 |
| Phase 1：数据与 bigram baseline | 未开始 | 测试、loss、样例生成 |
| Phase 2：attention 与 block | 未开始 | mask/shape/对齐测试 |
| Phase 3：GPT 与训练闭环 | 未开始 | checkpoint、曲线、生成 |
| Phase 4：训练与性能优化 | 未开始 | benchmark 对比表 |
| Phase 5：GPT-2 对齐与微调 | 未开始 | logits/权重映射验证 |
| Phase 6：复盘与闭卷重建 | 未开始 | 最终报告与演示 |

## 每次实验最少记录

- 日期、Git commit、硬件与 seed；
- 数据集与 tokenizer；
- 模型配置、训练配置、参数量；
- train/val loss、tokens/s、峰值显存；
- 本次只改变了什么、观察到什么、下一步是什么。
