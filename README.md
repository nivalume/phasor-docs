# phasor 知识库

[phasor](https://github.com/nivalume/vbt) 回测引擎的源码知识库：每页讲一件事（一个 crate、一个概念或一个算法），页面之间的链接、所属、依赖和相关关系组成一张图。
用 [Clio](https://github.com/nivalume/Clio) 生成网站：https://phasor-docs.vercel.app

页面是普通 Markdown，在 GitHub 上直接可读，从 [index.md](index.md) 开始。

## 运行

需要 [Bun](https://bun.sh) 1.2 或更新版本。

```bash
bun install          # 安装 Clio
bun run dev          # 本地预览，改动后自动刷新：http://localhost:4321
bun run check        # 检查 frontmatter、链接、锚点
bun run build        # 生成静态网站到 .clio/site
```

推到 `main` 后 Vercel 自动部署（配置在 `vercel.json`：先 `check` 再 `build`，有错误时部署失败）。

## 页面

`<目录>/<id>.md`，文件名就是页面 id，全库唯一，用英文 kebab-case。目录决定页面类型：

| 目录 | `type` | 写什么 |
|---|---|---|
| `guides/` | `guide` | 怎么读：全景、约定、路线、练习 |
| `crates/` | `crate` | 一个 crate 一页 |
| `concepts/` | `concept` | 跨函数的约定或数据结构 |
| `algorithms/` | `algorithm` | 一个核心算法一页 |
| `reference/` | `reference` | 查表用的页面 |
| `tooling/` | `tooling` | 引擎之外的设施：研究协议、差分测试、基准 |

新建页面：`bunx clio new algorithm "数量换算链" --id size-cascade`，模板在 `templates/`。

Frontmatter：

```yaml
---
title: 数量换算链与 percent        # 正文第一行的 # 标题与它相同
type: algorithm
summary: 一句话，160 字以内。        # 用于搜索结果、悬停提示和图谱卡片
tags: [execution, sizing]
rel:
  part-of: phasor-exec             # concept / algorithm 必填：所属的 crate 或 tooling 页面
  depends-on: [phasor-core]        # 只用于 crate 页面：Cargo.toml 里的依赖
  see-also: [buy-leg, sell-leg]    # 参见，不分方向
sources:                           # 这页讲的代码，仓库根目录下的路径，可带 :行 或 :起-止
  - crates/phasor-exec/src/kernel.rs:610-677
order: 802                         # 同类页面里的顺序（首页目录是手写的，这个字段留作排序和查询用）
---
```

crate 页面的小节顺序固定：定位 → 文件与阅读顺序 → 类图 → 核心算法与概念 → 调用链 → 阅读陷阱 → 与参考实现的对应 → 延伸资料 → 自测。概念和算法页以 `## 位置`（代码位置）开头，以 `## 相关` 结尾。

## 链接

- 到另一页：相对路径，`[数量换算链](../algorithms/size-cascade.md)`，可以带锚点 `#小节标题`（GitHub 的规则：小写，空格变 `-`，去掉标点，中文保留）。这样在 GitHub 和网站上都能点；`clio check` 检查目标和锚点。
- 到 vbt 仓库的文件（`docs/ARCHITECTURE.zh-CN.md`、`docs/plan.md` 等）：完整的 GitHub 地址，指向下面的固定提交。
- 图片放在 `assets/img/`。现有的 12 张图来自 vbt 的 `docs/img/zh/`（由 `docs/img/generate.py` 生成），vbt 里重新生成后要复制过来。
- Mermaid 图写在 ```` ```mermaid ```` 代码块里，网站在浏览器里渲染。

## 代码引用

代码位置写成指向 GitHub 的链接，链接文字是行内代码：

```markdown
[`crates/phasor-exec/src/kernel.rs:144`](https://github.com/nivalume/vbt/blob/81a1d5f238f2d583bad187a9ab6e1e6170316f11/crates/phasor-exec/src/kernel.rs#L144)
```

所有代码链接都指向 vbt 的同一个提交 `81a1d5f238f2d583bad187a9ab6e1e6170316f11`（barq 改名为 phasor 的那次提交），页面里的行号（链接里的，以及表格里写成纯文字的）都按这个提交核对过。
代码改动后：按新提交重新核对行号，再把全部链接里的提交号一起替换掉，并更新 [index.md](index.md) 开头写的提交。

vbt 是私有仓库，没有权限的读者点开代码链接会看到 404。

## 来源

这些页面原来在 vbt 仓库的 `wiki/content/`，由一个 React 查看器渲染；2026-10-14 迁到这里，内容不变，改动只有：
frontmatter 的 `kind` / `parent` / `depends` / `related` 改成 Clio 的 `type` 和 `rel`（`part-of` / `depends-on` / `see-also`），
指向仓库文件的相对链接改成 GitHub 地址，行内代码引用改成显式链接；改名后 rustfmt 重新换行，`portfolio.rs` 和 `phasor-py/src/lib.rs` 后半部分的行号移动了，
引用它们的三页（phasor-py、phasor-analysis、扫描结果汇总）已按新行号修正。
研究协议页的状态图里，转移标签中的第二个冒号（`[:12]`）会让 Mermaid 11 解析失败，改写成实体 `[#58;12]`，显示不变。
