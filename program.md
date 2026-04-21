# Auto Research 实验规范

> 对标 karpathy/autoresearch 方法论。本文件定义 agent 的执行规范。

## Setup（初始化实验）
1. 创建实验分支
2. 读取 prompt.json（唯一被修改的文件）
3. 生成初始图，让评判模型定义评分标准 → 写入 criteria.json（锁定）
4. 用标准评判初始图，确保分数 ≤ 50
5. 记录 baseline 到 results.tsv

## Experimentation（每轮实验）

### Phase 1: Generate
- 修改 prompt.json，生成新图
- 图保存到 rounds/round-NN/

### Phase 2: Judge
- 3次评判（temp 0.1/0.5/0.8），取中位数
- 记录到 results.tsv

### Phase 3: Decide
- KEEP（分数提升）→ commit → next
- DISCARD（分数退步）→ rollback + 写 attempts.md → 换方向 → next
- EARLY_STOP（≥90）→ merge 回主分支

## GPU 生命周期
同一时刻只有一个大模型占 GPU：
- Generate: ComfyUI ON, LM Studio OFF
- Judge: ComfyUI OFF, LM Studio ON
