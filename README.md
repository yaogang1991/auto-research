# Auto Research — ERNIE-Image Prompt 优化

基于 karpathy/autoresearch 方法论，用 AI 自动迭代优化文生图 prompt。

## 分支策略
- `topic/puff-mascot` — 主题主分支（存放最佳状态）
- `exp/puff-mascot-apr21-v1` — 实验分支
- 实验成功 → merge 回主分支

## 文件结构
```
├── program.md          # 实验指令（agent 执行规范）
├── prompt.json         # 唯一被 agent 修改的文件
├── criteria.json       # 评判标准（初始化时生成，锁定）
├── attempts.md         # 实验笔记（agent 维护）
├── results.tsv         # 结构化结果
├── rounds/             # 每轮产物（图 + prompt + 评判）
└── .git/
```

## 技术栈
- **生图**: ernie-image-turbo (ComfyUI)
- **评判**: qwen3.5-27b (LM Studio)
- **评分**: 百分制，每图评 3 次取中位数
- **环境**: WSL2 → Windows ComfyUI + LM Studio
