# 最终验收标准

## 1. 原理

- [ ] 从输入 token IDs 写出各层 shape，直至 logits；
- [ ] 推导 scaled dot-product attention，解释为何除以 `sqrt(d_head)`；
- [ ] 解释 causal mask、pre-norm、residual、MLP、weight tying；
- [ ] 解释 teacher forcing、next-token cross-entropy 和自回归采样；
- [ ] 比较 temperature、top-k 对生成分布的影响。

## 2. 实现

- [ ] 独立实现 tokenizer/data loader、attention、block、GPT、generate；
- [ ] 单元测试覆盖 shape、mask、有限值、梯度和参数量；
- [ ] 一个 batch 可被稳定过拟合；
- [ ] 能保存与恢复 model、optimizer、step、配置和随机状态；
- [ ] 能加载或映射 GPT-2 small 权重。

## 3. 训练工程

- [ ] 可配置 train/validation split、batch、context、模型规模和 seed；
- [ ] validation 不参与梯度，训练/评估模式切换正确；
- [ ] 掌握 AdamW、warmup/cosine、clipping、accumulation；
- [ ] 掌握 mixed precision、SDPA/Flash attention、compile 的适用边界；
- [ ] 至少记录一次 tokens/s、峰值显存、train/val loss。

## 4. 调试与解释

- [ ] 能系统排查 NaN、loss 不降、OOM、速度慢和生成退化；
- [ ] 能识别数据泄漏、mask 错误、off-by-one 和错误的 loss 对齐；
- [ ] 能向有算法基础的人在 20 分钟内讲清整个训练流程；
- [ ] 能在不看参考实现时重建核心 Transformer block。

## 5. 完成定义

“完成”不是复现 GPT-2 的昂贵训练结果，而是：核心路径完全由自己实现和理解，有小规模实验证据，
并能把同样方法迁移到新的语料、模型尺寸和训练预算。

