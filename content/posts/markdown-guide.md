---
title: "写作指南：Markdown 排版速查"
date: 2026-11-08T09:20:00+08:00
draft: false
tags: ["Markdown", "写作"]
categories: ["指南"]
summary: "本站支持的 Markdown 语法与排版效果速查，写新文章时可以对照这份示例。"
---

这篇文章既是写作说明，也是排版样式的预览。需要时可以直接复制格式。

## 标题层级

文章内标题从 `##` 开始，右侧会自动生成锚点，正文上方会显示目录。

### 三级标题

#### 四级标题

## 强调与链接

**加粗**、*斜体*、~~删除线~~、`行内代码`，以及[一个链接](https://gohugo.io/)。

## 列表

无序列表：

- 第一项
- 第二项
  - 嵌套项
  - 另一个嵌套项
- 第三项

有序列表：

1. 先做这件事
2. 再做那件事
3. 最后收尾

任务列表：

- [x] 搭建站点
- [x] 写第一篇文章
- [ ] 绑定自定义域名

## 引用

> 好的文字，是把复杂的事情说简单，而不是把简单的事情说复杂。
>
> —— 某位记不清名字的作者

## 代码

行内代码写作 `` `const a = 1` ``。

代码块支持语法高亮与右上角复制按钮：

```javascript
export async function getPosts(tag) {
  const res = await fetch(`/index.json`);
  if (!res.ok) throw new Error("加载失败");

  const posts = await res.json();
  return posts.filter((p) => p.title.includes(tag));
}
```

```bash
hugo new content posts/new-post.md
hugo server -D
```

## 表格

| 语法 | 作用 | 示例 |
| --- | --- | --- |
| `**文本**` | 加粗 | **重点** |
| `*文本*` | 斜体 | *提示* |
| `` `代码` `` | 行内代码 | `hugo` |
| `[文字](链接)` | 超链接 | [官网](https://gohugo.io/) |

## 图片

```markdown
![图片说明](/images/example.png "可选标题")
```

图片建议放在 `static/images/` 目录下，引用时以 `/images/` 开头，构建时会原样复制。

## 分隔线

---

## 小提示

- 每篇文章的 `summary` 会显示在列表页，写一两句话概括即可；
- 太长的文章记得多用二级标题，方便生成目录；
- 草稿保持 `draft: true`，加 `-D` 本地预览时才会出现。
