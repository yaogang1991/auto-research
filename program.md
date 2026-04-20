# Auto Research 实验规范

> 对标 karpathy/autoresearch 方法论。本文件定义 agent 的执行规范。

## Setup（初始化实验）

1. 从主题主分支创建实验分支：`puff-mascot/<date>-v<n>`
2. 读取 `prompt.json`（唯一被修改的文件）
3. 生成初始图，让评判模型定义评分标准 → 写入 `criteria.json`（锁定）
4. 用标准评判初始图，确保分数 ≤ 50（调整标准直到满足）
5. 记录 baseline 到 `results.tsv`
6. 确认后开始实验

## Experimentation（每轮实验）

### Phase 1: Generate
- 修改 `prompt.json`，生成新图
- 图保存到 `rounds/round-<nn>/image.png`

### Phase 2: Judge
- 用同一套 `criteria.json` 评判
- 每张图评 3 次（temperature 0.1 / 0.5 / 0.8），取中位数
- 结果保存到 `rounds/round-<nn>/judge.json`

### Phase 3: Decide
- **分数提升** → git commit (keep)，继续下一轮
- **分数退步** → git commit (discard)，回滚到上一个 keep 状态，写入 `attempts.md`
- **分数达标** → merge 回主分支，实验结束

## 约束
- `criteria.json` 只在初始化时生成，实验期间不可修改
- `program.md` 由人类维护，agent 只读
- 每轮必须有 git commit（keep 或 discard）
- `results.tsv` 追加记录
- `attempts.md` 记录失败尝试的教训
