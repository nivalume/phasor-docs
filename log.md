# 日志

只追加。格式：`## [YYYY-MM-DD] 操作 | 标题`，下面列出改动的笔记。

## [2026-10-14] init | 创建知识库

## [2026-10-14] import | 从 vbt 的 wiki/content 迁入 84 页
- 来源：nivalume/vbt 提交 81a1d5f（barq 改名为 phasor）的 `wiki/content/`，正文不变。
- frontmatter：`kind` → `type`，`parent` / `depends` / `related` → `rel` 的 `part-of` / `depends-on` / `see-also`。
- 指向仓库文件的链接改成 GitHub 地址；12 张图复制到 `assets/img/`；426 处代码引用改成指向 81a1d5f 的链接。
- 修正改名后移动的行号：phasor-py、phasor-analysis、sweep-aggregation。
- research-protocol 的状态图：`[:12]` 写成 `[#58;12]`（标签里第二个冒号让 Mermaid 11 解析失败）。
