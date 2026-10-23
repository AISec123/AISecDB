# AISecDB

AI 安全知识图谱数据集。企业、开源项目、文章、标准与数据集，以及它们之间的标签关系。

[AISec123](https://aisec123.github.io/) 是它的第一个消费方，但数据本身不绑定任何前端：
`entries.json` 是一个纯 JSON 文件，任何项目都可以直接拿去用。

## 数据地址

```
https://raw.githubusercontent.com/AISec123/AISecDB/main/entries.json
```

`raw.githubusercontent.com` 带 `access-control-allow-origin: *`，可以直接在浏览器里 fetch。
中国大陆网络访问 raw 可能被阻断，正式站点建议在发布时同步一份到自己的域名下，
而不是在运行时跨域取。

## 结构

```
entries.json    数据本体
schema.json     JSON Schema（draft-07），字段定义与约束
```

`entries.json` 只有四层：

```jsonc
{
  "updated": "2026-10-21",   // 内容最后修改日期
  "sections": [              // 板块，顺序即展示顺序
    { "key": "companies", "zh": "AI 安全公司", "en": "Companies", "color": "#F72585", ... }
  ],
  "entries": [               // 实体
    {
      "name": "Protect AI",
      "url": "https://protectai.com",
      "section": "companies", // 指向 sections[].key
      "tags": ["模型安全", "LLM 防护"],  // 图谱的边
      "desc": "模型扫描 + LLM 运行时防护的 AI/ML 安全平台。",
      "desc_en": "Model scanning plus LLM runtime protection.",
      "added": "2026-10-21"
    }
  ]
}
```

完整字段见 [`schema.json`](./schema.json)。几个约定：

- **`section` 必须在 `sections` 里存在**，否则该条会被丢弃。
- **`logo` 只接受 `assets/logos/` 开头的本地路径**。数据集不引用第三方图片 URL——
  运行时现取 favicon 经常回一张通用占位图，三家不同公司顶上出现同一个假 logo，
  比首字母字标更糟。不填就退回首字母。
- **`tags` 是关系，不是分类**。一个实体可以有多个标签，跨板块共用同一套标签词汇。
  想让两个实体产生关联，给它们同一个标签，而不是新造一个近义词。

## 收录口径

人工核对过才收，不看 star 数、不看融资额、不收钱。

- 公司条目要求有实际产品，不是只有一个 landing page。
- 开源条目要求能自己跑起来。
- 文章只收技术拆解、威胁分析和行业报告，不收新闻稿和软文。
- 收录与任何商业展示位无关。

漏了或写错了，[开 issue](https://github.com/AISec123/AISecDB/issues/new) 或直接提 PR。

## 消费

```bash
curl -s https://raw.githubusercontent.com/AISec123/AISecDB/main/entries.json | jq '.entries | length'
```

按标签找关联实体（这才是「图谱」的部分）：

```bash
curl -s https://raw.githubusercontent.com/AISec123/AISecDB/main/entries.json \
  | jq -r '.entries[] | select(.tags | index("MCP")) | .name'
```

## 许可

待定。在定下来之前，本仓库内容归 AISec123 所有，未经许可请勿再分发。

## 相关

- [aisec123.github.io](https://aisec123.github.io/) —— 基于本数据集的站点
- [Scanners-Box](https://github.com/We5ter/Scanners-Box) —— 每日推荐专栏
