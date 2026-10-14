# phasor 知识库：给 Agent 的约定

这是 phasor 回测引擎（代码在 [nivalume/vbt](https://github.com/nivalume/vbt)）的中文源码知识库，用 Clio 生成网站。
Clio 的通用工作流程在 `skills/clio/SKILL.md`；这份文件写这个知识库的约定，两者冲突时以这里为准。
页面类型、frontmatter、链接和代码引用的完整写法在 `README.md`。

## 命令

```bash
bun install
bun run check        # 必须 0 错误；也应保持 0 警告（锚点、待写笔记）
bun run build
bun run dev          # http://localhost:4321
```

## 容易出错的规则

- 页面 id 就是文件名（英文 kebab-case），全库唯一；目录决定类型（`guides/ crates/ concepts/ algorithms/ reference/ tooling/`）。
- 概念页和算法页必须有 `rel.part-of`，指向一个 crate 页或工具链页；`rel.depends-on` 只用在 crate 页，照抄 `Cargo.toml`。
- 页面之间用相对 Markdown 链接（`../algorithms/size-cascade.md#小节`），不用 `[[ ]]`：这样在 GitHub 上也能点。
- 代码位置写成链接：`` [`crates/…/x.rs:12-30`](https://github.com/nivalume/vbt/blob/<提交>/crates/…/x.rs#L12-L30) ``。
  全库只用一个提交（见 `README.md`「代码引用」），不要混用分支名或别的提交。
- 行号必须在那个提交上核对过，包括表格里写成纯文字的行号（如「`build` 263–314」）。代码变了就重新核对，并整体替换提交号。
- crate 页的小节顺序固定；概念和算法页以 `## 位置` 开头、`## 相关` 结尾。模板在 `templates/`。
- 原始 HTML 会被丢弃；Mermaid 标签里的 `<br/>` 在代码块里，不受影响。

## 每次修改之后

- `bun run check` 没有错误和警告。
- 在 `log.md` 追加 `## [YYYY-MM-DD] edit | 标题`。
