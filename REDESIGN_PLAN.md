# DGrid AI Docs 改版规划（Redesign Plan）

> 本文档是完整的实施规划，面向执行改版的 AI 模型/开发者。目标：参考 https://aspen.mintlify.app/ 的风格，做出干净、高级、专业的文档站。
>
> 仓库：`/Users/dxqian/Desktop/mintlify-docs/`（Mintlify，theme: aspen）
> 验证方式：`npx mintlify@latest dev` → http://localhost:3000
>
> ⚠️ 重要经验：MDX 中**不能出现未转义/未闭合的原生 HTML 标签**（如 `<wbr>`、裸 `<DGRID_API_KEY>` 出现在表格正文等），会导致整页 404。所有尖括号内容必须放进反引号内联代码或代码块中。改完每个文件后用 `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/<slug>` 验证返回 200。

---

## 一、信息架构重组（docs.json）

### 1.1 改为 Tabs 导航（核心变更）

现状：单一侧边栏塞了 Introduction / Design / Economic System / Core Products（深层嵌套 Model API 9 页）/ DGrid Premium / Appendix，产品文档和 API 参考混在一起，层级深、显得杂乱。

目标：拆成两个 Tab —— **Documentation**（产品/概念/指南）和 **API Reference**（纯接口参考）。这是 Stripe/OpenAI/Vercel 等一线开发者文档的通行做法，也是「专业感」的最大来源。

`navigation` 替换为（保持 `languages` 外层结构，便于以后加 zh-TW/kr）：

```json
"navigation": {
  "languages": [
    {
      "language": "en",
      "default": true,
      "tabs": [
        {
          "tab": "Documentation",
          "icon": "book-open",
          "groups": [
            {
              "group": "Get Started",
              "pages": ["index", "quickstart", "what-we-do", "background"]
            },
            {
              "group": "Core Concepts",
              "pages": ["dgrid-solution", "node-operators"]
            },
            {
              "group": "AI Gateway",
              "pages": [
                "ai-gateway/overview",
                "ai-gateway/integrations",
                "ai-gateway/supported-regions"
              ]
            },
            {
              "group": "AI Arena",
              "pages": ["ai-arena/overview", "ai-arena/arena-for-agent"]
            },
            {
              "group": "Ecosystem",
              "pages": ["dori", "dclaw"]
            },
            {
              "group": "$DGAI Token",
              "pages": [
                "what-is-dgai",
                "core-functions",
                "circulation-mechanism",
                "token-distribution"
              ]
            },
            {
              "group": "Genesis Premium",
              "pages": [
                "premium/program-overview",
                "premium/exclusive-benefits",
                "premium/purchase-and-activation",
                "premium/incentive-mechanism-and-rewards",
                "premium/compliance-and-regulatory",
                "premium/legal-disclaimer"
              ]
            },
            {
              "group": "Resources",
              "pages": ["roadmap", "academic-research", "ai-gateway/terms-of-service"]
            }
          ]
        },
        {
          "tab": "API Reference",
          "icon": "code",
          "groups": [
            {
              "group": "Getting Started",
              "pages": ["ai-gateway/model-api/overview", "management-api-keys"]
            },
            {
              "group": "Model API",
              "pages": [
                "ai-gateway/model-api/chat",
                "ai-gateway/model-api/completions",
                "ai-gateway/model-api/embeddings",
                "ai-gateway/model-api/images",
                "ai-gateway/model-api/audio",
                "ai-gateway/model-api/realtime",
                "ai-gateway/model-api/moderations",
                "ai-gateway/model-api/models"
              ]
            },
            {
              "group": "x402 API",
              "pages": ["x402/overview", "x402/api-reference"]
            }
          ]
        }
      ]
    }
  ]
}
```

要点：
- **不需要移动任何文件**，所有 slug 不变，站内链接全部继续有效。
- Model API 排序从字母序改为**使用频率序**（Chat 最常用排第一）。
- `quickstart` 是新增页面（见 §3.1）；如果决定不做新页，从 `pages` 里去掉这一项即可。
- `management-api-keys`（含 Management API 端点文档）归入 API Reference。

### 1.2 docs.json 其他升级

在顶层增加/修改以下字段（每项独立，可单独取舍）：

```json
"appearance": {
  "default": "system"
},
"background": {
  "decoration": "gradient"
},
"icons": {
  "library": "lucide"
},
"contextual": {
  "options": ["copy", "view", "chatgpt", "claude"]
}
```

- `background.decoration: "gradient"`：aspen 主题支持的页面背景装饰，干净的渐变质感（备选 `"grid"` / `"windows"`，gradient 最稳）。
- `icons.library: "lucide"`：lucide 线条图标比 fontawesome 更现代克制，与「高级感」匹配。**注意**：换库后需检查现有 `icon` 值在 lucide 中是否存在（如 `"bolt"`→lucide 叫 `"zap"`，`"circle-info"`→`"info"`，`"store"`→`"store"` 存在，`"gem"`→`"gem"` 存在，`"coins"`→`"coins"` 存在，`"trophy"`→`"trophy"` 存在，`"compass"`→`"compass"` 存在，`"globe"`→`"globe"` 存在）。
- `contextual`：在每页代码块/页面右上提供 “Copy / Open in ChatGPT / Claude” 按钮，AI 时代开发者文档的标配细节。

navbar 调整（更动作导向）：

```json
"navbar": {
  "links": [
    { "label": "Website", "href": "https://dgrid.ai/" },
    { "label": "Support", "href": "https://telegram.me/dgrid_ai" }
  ],
  "primary": {
    "type": "button",
    "label": "Get API Key",
    "href": "https://dgrid.ai/"
  }
}
```

footer 扩充为三栏（更像正式产品站）：

```json
"footer": {
  "socials": {
    "x": "https://x.com/dgrid_ai",
    "telegram": "https://telegram.me/dgrid_ai"
  },
  "links": [
    {
      "header": "Products",
      "items": [
        { "label": "AI Gateway", "href": "/ai-gateway/overview" },
        { "label": "AI Arena", "href": "/ai-arena/overview" },
        { "label": "Dori", "href": "/dori" }
      ]
    },
    {
      "header": "Developers",
      "items": [
        { "label": "API Reference", "href": "/ai-gateway/model-api/overview" },
        { "label": "x402 API", "href": "/x402/overview" },
        { "label": "Integrations", "href": "/ai-gateway/integrations" }
      ]
    },
    {
      "header": "Company",
      "items": [
        { "label": "Roadmap", "href": "/roadmap" },
        { "label": "Terms of Service", "href": "/ai-gateway/terms-of-service" }
      ]
    }
  ]
}
```

### 1.3 Logo 待办（人工确认项）

当前 light/dark 共用一张 `/logo/dgrid-logo.png`。如果该 logo 在深色背景下辨识度差，需要设计一张深色模式专用 logo（`/logo/dgrid-logo-dark.png`）并改 `"logo": { "light": ..., "dark": ... }`。实现模型无法自行产出资产，**在交付说明里向用户提出此项即可，不阻塞其他工作**。

---

## 二、首页重做（index.mdx）—— 对标 aspen demo

### 2.1 aspen demo 首页的实际结构（已抓取源码确认）

1. Hero：`text-4xl` 大标题 + 灰色副标题 + 胶囊形 "Get started →" 按钮（带箭头 SVG）。
2. 整页用 `<div className="px-5 divide-y divide-gray-100 dark:divide-white/10">` 包裹，**每个 section 之间自动出现一条极细分隔线**——这就是它「干净高级」的关键排版手法。
3. 区块 2：小节标题 + 一句说明 + `CardGroup cols={3}` 图片/内容卡片。
4. 区块 3（Resources）：`Columns cols={3}` + 自定义 icon 的 `Card`。

### 2.2 新 index.mdx 完整骨架（可直接采用，替换全文件）

要点：
- frontmatter 加 `mode: "wide"`（隐藏右侧 TOC，首页更像 landing page）。
- **删除现有的双 logo `<img>` 和重复的 `# DGrid AI` H1**（frontmatter title 已渲染标题，现状是双标题，不专业）。
- 文案全部用现有内容改写，不杜撰新事实。

```mdx
---
title: "DGrid AI Documentation"
description: "Explore guides and API references for building on the decentralized smart network of AI."
mode: "wide"
---

<div className="px-5 divide-y divide-gray-100 dark:divide-white/10">
  <div className="w-full flex flex-col pb-10">
    <div className="flex flex-col items-start justify-center w-full max-w-4xl text-left">
      <div className="text-gray-900 dark:text-gray-200 text-4xl tracking-tight">
        DGrid AI Documentation
      </div>

      <p className="text-md text-gray-600 dark:text-gray-400 text-left mt-3 mb-4">
        Access 200+ leading AI models through one Web3-native gateway. Explore guides,
        API references, and the $DGAI economy powering the decentralized smart network of AI.
      </p>

      <div className="flex flex-row gap-4 mt-5">
        <a href="/quickstart">
          <button type="button" className="px-5 flex items-center font-medium text-sm rounded-full py-2 shadow-sm text-white dark:text-gray-900 bg-primary-dark dark:bg-primary-light hover:opacity-[0.9] justify-center">
            Get started
          </button>
        </a>
        <a href="/ai-gateway/model-api/overview">
          <button type="button" className="px-5 flex items-center font-medium text-sm rounded-full py-2 border border-gray-300 dark:border-white/20 text-gray-900 dark:text-gray-200 hover:bg-gray-50 dark:hover:bg-white/5 justify-center">
            API Reference
          </button>
        </a>
      </div>
    </div>
  </div>

  <div className="flex flex-col w-full py-8">
    <div className="text-gray-900 dark:text-gray-200 text-xl">Start building</div>
    <p className="text-gray-600 dark:text-gray-400 text-md mb-4 mt-2">
      Make your first model call in minutes.
    </p>

    <CardGroup cols={3}>
      <Card title="Quickstart" icon="rocket" href="/quickstart">
        Create an API key and send your first request in under 5 minutes.
      </Card>
      <Card title="Model API" icon="code" href="/ai-gateway/model-api/overview">
        OpenAI-, Claude-, and Gemini-compatible endpoints for chat, images, audio, and more.
      </Card>
      <Card title="Integrations" icon="plug" href="/ai-gateway/integrations">
        Plug DGrid into Claude Code, Moltbot, and other tools you already use.
      </Card>
    </CardGroup>
  </div>

  <div className="flex flex-col w-full py-8">
    <div className="text-gray-900 dark:text-gray-200 text-xl">Explore the platform</div>
    <p className="text-gray-600 dark:text-gray-400 text-md mb-4 mt-2">
      Products and primitives that make up the DGrid ecosystem.
    </p>

    <CardGroup cols={3}>
      <Card title="AI Gateway" icon="zap" href="/ai-gateway/overview">
        One API for 200+ models with Web3-native payments and routing.
      </Card>
      <Card title="AI Arena" icon="trophy" href="/ai-arena/overview">
        Blind model battles that turn community votes into routing intelligence.
      </Card>
      <Card title="Dori" icon="compass" href="/dori">
        A natural-language advisor that picks the optimal model for every task.
      </Card>
      <Card title="x402 Payments" icon="credit-card" href="/x402/overview">
        Pay-per-inference with the x402 protocol — no account balance required.
      </Card>
      <Card title="DClaw" icon="bot" href="/dclaw">
        An agent runtime built on CoPaw for autonomous on-chain workflows.
      </Card>
      <Card title="Genesis Premium" icon="gem" href="/premium/program-overview">
        The Genesis Pass NFT: resource access, governance, and dividends.
      </Card>
    </CardGroup>
  </div>

  <div className="flex flex-col w-full py-8">
    <div className="text-gray-900 dark:text-gray-200 text-xl">Understand the economy</div>
    <p className="text-gray-600 dark:text-gray-400 text-md mb-4 mt-2">
      How $DGAI aligns incentives across the network.
    </p>

    <CardGroup cols={3}>
      <Card title="What is $DGAI" icon="coins" href="/what-is-dgai">
        The utility token behind value exchange and governance.
      </Card>
      <Card title="Token Distribution" icon="chart-pie" href="/token-distribution">
        Allocation, vesting, and circulation mechanics.
      </Card>
      <Card title="Roadmap" icon="map" href="/roadmap">
        Where the network is headed next.
      </Card>
    </CardGroup>
  </div>

  <div className="flex flex-col w-full pt-8">
    <div className="text-gray-900 dark:text-gray-200 text-xl mb-4">Resources</div>

    <Columns cols={3}>
      <Card title="Official Website" icon="globe" href="https://dgrid.ai/">
        Explore the live platform at dgrid.ai
      </Card>
      <Card title="Follow on X" icon="twitter" href="https://x.com/dgrid_ai">
        Product updates and announcements
      </Card>
      <Card title="Telegram Community" icon="send" href="https://telegram.me/dgrid_ai">
        Get help and meet other builders
      </Card>
    </Columns>
  </div>
</div>
```

实现注意：
- 若 `icons.library` 未切到 lucide，把上面 icon 名换回 fontawesome 等价名（`zap`→`bolt`、`rocket`→`rocket`、`credit-card`→`credit-card`、`bot`→`robot`、`chart-pie`→`chart-pie`、`send`→`paper-plane`、`twitter`→`x-twitter`）。
- `mode: "wide"` 若在 aspen 主题下表现异常（标题重复渲染等），退化方案：去掉 mode，保留正文结构。
- 渲染后务必同时检查 light/dark 两种模式（颜色 class 都写了 dark: 变体）。

---

## 三、新增/调整页面

### 3.1 新增 `quickstart.mdx`（强烈建议）

「高级专业」的开发者文档必有 5 分钟 Quickstart。内容**全部从现有页面取材改写**，不新造事实：

- 来源 A：`management-api-keys.mdx` 的「Create a Management API Key」`<Steps>` 部分（创建 key 流程）。
- 来源 B：`ai-gateway/model-api/overview.mdx` 的 Quickstart curl 示例（`POST /v1/chat/completions`，模型 `openai/gpt-4o`）。

页面结构：

```
frontmatter: title "Quickstart", description "Make your first DGrid API call in under 5 minutes"
<Steps>
  Step 1: Get your API key —— 引导到 dashboard，摘自来源 A
  Step 2: Make your first request —— <CodeGroup> cURL / Python / JavaScript（从 overview 的 curl 改写出三语言版本，base URL https://api.dgrid.ai）
  Step 3: Explore next —— 3 张 Card：Model API Reference / Integrations / x402 Payments
</Steps>
```

### 3.2 sidebarTitle 微调（可选）

过长的侧边栏条目用 frontmatter `sidebarTitle` 缩短（不改 title 本身）：

| 文件 | sidebarTitle |
| --- | --- |
| `premium/incentive-mechanism-and-rewards.mdx` | `Incentives & Rewards` |
| `premium/purchase-and-activation.mdx` | `Purchase & Activation`（如已是则跳过） |
| `premium/compliance-and-regulatory.mdx` | `Compliance` |
| `ai-gateway/model-api/overview.mdx` | `Overview` |
| `x402/api-reference.mdx` | `API Reference`（已是则跳过） |
| `ai-arena/overview.mdx` | `Overview` |
| `ai-gateway/overview.mdx` | `Overview` |

---

## 四、存量页面统一打磨（全站 pass）

对全部 37 个 mdx 做一次轻量整理，规则如下（**只动形式，不改内容事实**）：

1. **消灭重复 H1**：frontmatter `title` 已渲染页面大标题，正文中若再出现同名 `# 标题` 一律删除（index.mdx 已知存在，其他页逐一检查）。
2. **截图统一包 `<Frame>`**：所有内容图片（`/images/*.jpeg|png`）改为：
   ```mdx
   <Frame caption="简短说明">
     <img src="/images/xxx.jpeg" alt="..." />
   </Frame>
   ```
   涉及页面：`ai-arena/overview.mdx`、`ai-arena/arena-for-agent.mdx`、`dgrid-solution.mdx`、`token-distribution.mdx` 等含图页面（grep `/images/` 找全）。
3. **裸链接列表卡片化**：页面里成段的「链接 + 一句话说明」列表（典型如 `ai-gateway/overview.mdx` 底部、`what-we-do.mdx`），改写成 `<CardGroup cols={2}>`。每页最多一处，避免满屏卡片。
4. **尖括号安全检查**（防 404）：`grep -rnE '<[A-Za-z][a-zA-Z]*>' --include="*.mdx" .` 排查所有非 Mintlify 组件的裸 HTML 标签；表格/正文里的占位符（如 `<DGRID_API_KEY>`）必须在反引号里。
5. **frontmatter 完整性**：每页必须有 `title` + `description`（description 会用于 SEO 和搜索摘要），缺失的补齐（从首段提炼一句）。
6. **API 参考页不动**：`ai-gateway/model-api/*.mdx`、`x402/api-reference.mdx` 结构已统一（http 块 + 元数据表 + RequestExample/ResponseExample），本次不要改动其内部结构，只做上面第 4、5 条检查。

---

## 五、实施顺序与验收

### 推荐顺序（每步可独立验证）

1. **docs.json**：tabs 重构 + appearance/background/icons/contextual + navbar/footer（§1）。验证：dev server 起来后两个 Tab 正常、所有页面可达。
2. **index.mdx 重写**（§2.2）。验证：light/dark 双模式下 hero、分隔线、卡片渲染正常。
3. **quickstart.mdx 新建**（§3.1）+ 加入导航。
4. **sidebarTitle 微调**（§3.2）。
5. **全站打磨 pass**（§4），按文件逐个提交、逐个 curl 验证。

### 验收清单

- [ ] `npx mintlify@latest dev` 启动无 error/warning（`✗`/`error` 关键字）。
- [ ] 全部页面（原 37 + quickstart = 38）`curl` 返回 200，特别是改动过的页面。
- [ ] docs.json 里每个 page 引用都有对应文件；没有文件游离在导航之外（可用脚本校验，逻辑：解析 navigation 收集 slug ↔ glob `**/*.mdx` 对比）。
- [ ] 首页在浅色/深色模式下视觉正常，无双标题。
- [ ] 两个 Tab 切换正常，Model API 按 Chat→…→Models 顺序排列。
- [ ] 站内链接抽查：`/ai-gateway/model-api/overview`、`/premium/program-overview`、`/x402/api-reference` 从其他页面点击可达。

### 明确不做（本次范围外）

- zh-TW / kr 多语言迁移（用户已明确推迟，等英文版定稿后再做）。
- OpenAPI spec 自动生成 API playground（现有手写 MDX 参考页已够用，引入 openapi.json 是更大的工程，可作为后续提案）。
- 深色模式专用 logo 资产（需要设计产出，见 §1.3，仅向用户提出）。
