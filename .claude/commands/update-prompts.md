---
name: update-prompts
description: 从 Supabase 拉取最新 prompts 数据，更新 README 统计信息和更新日志
---

# 更新 Prompts 数据

从 Supabase 数据库拉取最新的 trending prompts，更新本地文件和 README 中的统计信息。

## 步骤

### 1. 获取旧数据

读取 README.md 中 Changelog 表格的第一条数据行，提取上次的总数作为旧总数。

### 2. 运行更新脚本

```bash
python3 update-github-prompts.py
```

从输出中提取：
- 新的 prompt 总数（如 `Fetched 1542 records total`）
- 模型分布（如 `'nanobanana': 1295, 'gptimage': 247`）

脚本会自动更新 `prompts/prompts.json`、`README.md` 和 `README.zh-CN.md` 中 `<!-- PROMPTS_START -->` 标记之后的 prompt 列表内容。

### 3. 计算变化

- 新增数 = 新总数 - 旧总数
- 判断描述文字是否需要更新（总数跨越百位整数时，如 1,400+ → 1,500+）

### 4. 更新 README.md（英文）

同时更新以下四处内容：

**Badge 数字：**
```
Prompts-{旧数字,逗号分隔} → Prompts-{新数字,逗号分隔}
```
含 alt 文本中的数字一并更新。

**描述文字（仅跨百位时）：**
```
{旧百位}+ curated AI image prompts → {新百位}+ curated AI image prompts
```

**Stats 表格：**
```
| {新总数} | {NanoBanana数} | {GPTImage数} |
```
数字使用逗号分隔格式。

**Changelog 表格顶部新增一行：**
```
| {今天日期 YYYY-MM-DD} | Added {新增数} new prompts. Total: {新总数,逗号分隔}. |
```

### 5. 更新 README.zh-CN.md（中文）

与英文 README 相同的四处更新：

**Badge：** `提示词-{新数字}` 及 alt 文本

**描述文字（仅跨百位时）：** `{新百位}+ 精选 AI 图像提示词`

**Stats 表格：** 同英文

**Changelog：**
```
| {今天日期} | 新增 {新增数} 条提示词，总计 {新总数,逗号分隔} 条。 |
```

### 6. 展示摘要并等待确认

展示变更摘要（新增数、新旧总数、模型分布），然后询问用户是否需要提交并推送到 GitHub。

仅在用户明确要求时才执行 git commit 和 push。
