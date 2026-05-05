# 网红作弊器 / Cheat on Content

[![Version](https://img.shields.io/badge/version-v0.1.0-orange)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-early%20product-yellow)](#早期产品警示)

一个给内容创作者用的 Claude Code skill 包——把"我感觉这条会爆"变成可校准的预测。

> ⚠️ **早期产品（v0.1.0）警示** <a id="早期产品警示"></a>
>
> 这是公开的第一个 release。**核心方法论稳定**（盲预测 + 校准循环 + immutable 日志），但**接口仍在快速迭代**：
>
> - **State schema 可能 breaking**：升级前建议 backup 你的 `<your-channel>/` 整个目录
> - **Adapter fragility**：抖音 / 小红书 adapter 依赖反爬绕过，平台改版可能 break
> - **暂无自动 migration**：每次升级 if `CHANGELOG.md` 标 `BREAKING` → 照 manual steps 走，或 wipe + 重 init
>
> 如果你不愿意接受这些风险，**等 v1.0 stable 再用**。
>
> 用了的话——欢迎 [开 issue](#) 反馈。每条反馈都会让下个版本更好。

> 🎯 **方法论通用，rubric 是循环的"内容"**
>
> **方法论**（打分 → 盲预测 → 发布 → 复盘 → 进化 rubric）适用任何能被量化的内容——视频 / 文章 / 播客 / Newsletter / 短文 / 教程 / Builder 号。
>
> **rubric 是循环的"内容"，不是循环本身**——当前内置一份观点视频 rubric（参考博主 25+ 视频拟合），其他形态可借这套**起步**，跑几篇后用 `/cheat-bump` 调权重 + 加减维度 → 适配你的形态。
>
> **强烈建议导入对标账号**作为初始信号源（`/cheat-learn-from`）—— init 时 rubric 没数据 anchor，全靠对标。导入 5-10 条对标样本后，工具的 rubric / pattern / 选题方向感都有了起点。
>
> 所有阶段统一格式预测，header 显示 confidence 等级（🔴/🟠/🟡/🟢/🔵），**不通过省略数字解决精度问题**。

一个给内容创作者用的 Claude Code skill 包——拒绝凭直觉发视频/写文章的人专用。

---

## 它在做什么

每个博主都知道一个事实：你发的内容大部分会扑街，少数能跑出来。但你不知道哪条是哪条——直到为时已晚。

**网红作弊器**把"我感觉这条会爆"变成可校准的预测：

1. **打分**：用你自己校准过的 rubric 给每篇稿子评分——不是凭空想出来的维度，是**从你过往数据反推出来**的、真正能预测你账号爆款的维度
2. **预测**：发布前给出播放量 bucket + 中枢 + 概率分布，**写下来就不许改**（hook 在 harness 层物理强制）
3. **复盘**：T+3d 抓数据，每条预测都会被判定为命中 / 高估 / 低估，每次失败都被记录
4. **进化**：当证据足够时，rubric 会被升级——从来不是凭感觉调权重，而是用整个校准池重打分**+ 跨模型独立审核**验证

是的，这基本就是作弊。计算器也是。Google 也是。每一个拼写检查器都是。

---

## 这次（v1）做了什么

从 v0 的"一份 SKILL.md + 君子协定"重建为按 [ARIS](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep) 工程惯例组织的 skill 包：

- ✅ **9 个子 skill** — 每一步执行都有明确入口（`/cheat-init` `/cheat-score` `/cheat-predict` `/cheat-publish` `/cheat-retro` `/cheat-bump` `/cheat-recommend` `/cheat-trends` `/cheat-status`）
- ✅ **5 份 shared-references 协议** — 三条原则的完整可执行规范（盲预测 / bump 验证 / 观察生命周期）
- ✅ **prediction-immutability hook** — 把"预测段不可改"从模型自律升级为 harness 阻塞（**端到端验证通过**）
- ✅ **跨模型 bump 审核** — `cheat-bump` 强制调外部 LLM 独立判定（via mcp__llm-chat__chat），自审是 escape hatch 而非默认
- ✅ **渐进式选题库** — 默认无 pool；`/cheat-trends` 用多 adapter 让"我没素材"问题在 onboarding 第二步消失
- ✅ **`/cheat-init` 5 问 onboarding** — 给从没做过自媒体的人也能 5 分钟跑通的入口
- ✅ **score-curve.py** — 用真实预测数据画收敛曲线（既是营销也是诊断工具）

剩下的批次（templates 实例化 / SQLite 迁移 / adapters 全套 / 长文 + 短文 starter-rubrics）见 SKILL.md 里的"⬜"标记。

---

## 谁该用

- **观点类创作者**：YouTube / B 站 / 抖音 / TikTok / Substack / 公众号 / 即刻 / X——任何单篇内容能被量化（播放 / 阅读 / 收听）的平台
- **冷启动博主**（已发布 < 5 篇）：从 v0 等权 rubric 起步，前 5 篇精度 ±50% 是数学事实
- **校准模式博主**（已发布 ≥ 5 篇且有可量化数据）：导入历史作品，让 skill 帮你**反推适合你账号的 rubric**

---

## 安装

### 1. 把项目 clone 下来

```bash
git clone https://github.com/<你或上游>/cheat-on-content.git
cd cheat-on-content
```

或者下载 zip 解压。

### 2. 跑安装脚本

```bash
bash install.sh
```

这会把 10 个子 skill 软链接到 `~/.claude/skills/`，让 Claude Code 全局可用。**安装动作只做一次**——以后任何内容项目都能用。

可选模式：

```bash
bash install.sh --copy   # 用复制代替软链接（frozen 版本，dev 改动不生效）
```

软链接（默认）适合 dev / 想跟新；复制适合发布到生产环境。

### 3. 验证

```bash
ls -la ~/.claude/skills/ | grep cheat
```

应该看到 10 个 `cheat-*` 条目。

### 4. 卸载（如果以后想清理）

```bash
bash uninstall.sh
```

只移除全局 skill 链接，**不会删你内容项目里的数据**（predictions/ / rubric_notes.md / .cheat-state.json 等保留）。

---

## 可选：装 perf-data adapter（自动抓取播放/评论）

默认 `/cheat-retro` 走 manual paste 路径——你粘数据，Claude 处理。如果想**自动抓取**，装对应平台的 adapter。

### 抖音（douyin-session）

抓视频播放 / 完播 / 转粉 / 评论。需要 Playwright + Chromium + 你的抖音创作者中心登录态。

```bash
# 在你的内容项目根（不是 skill 源码目录！）
cd ~/my-channel

# 建虚拟环境
python3 -m venv .venv
source .venv/bin/activate

# 装依赖
pip install -r ~/Desktop/cheat-test/cheat-on-content/adapters/perf-data/douyin-session/requirements.txt
playwright install chromium

# 首次扫码登录抖音
python ~/Desktop/cheat-test/cheat-on-content/adapters/perf-data/douyin-session/crawler.py login
# → 弹出 Chromium 窗口，扫码

# 启用 adapter
# 编辑 .cheat-state.json，把 enabled_perf_adapters 改为 ["douyin-session"]
# 把 data_collection 改为 "adapter"
```

完整 adapter 文档：[adapters/perf-data/douyin-session/README.md](adapters/perf-data/douyin-session/README.md)。

### 其他平台

YouTube / B 站 / 小红书 等 adapter 待写（路线图）。可参照 douyin-session 的实现自己加。

---

## 快速开始

### 第 1 次会话

```
你（在你的 content 项目目录里说）：
  初始化 cheat-on-content

Claude：
  [问 5 个 yes/no — 内容形态 / 历史发布数 / 数据回收方式 / 候选选题 / 装钩子]
  [创建 rubric_notes.md / predictions/ / .cheat-state.json / .claude/settings.json]
  [测试钩子是否拦截 prediction 段编辑]
  [cold-start + 没候选 → 自动询问"现在跑 /cheat-seed 找前 5 个选题？"]

如果你说 "yes, seed"：
  [问 3 个问题 — 兴趣关键词 / 频道调性 / 红线]
  [拉公开热点 (微博热搜 + 知乎热榜) + Claude brainstorm]
  [输出 15 候选让你挑 5]
  [默认顺带写 5 个 draft（你必须改写后再拍）]
```

### 之后

每条视频对应**三处文件**，用同一组 `<日期>_<id>_<short>` 命名：

- `scripts/<...>.md` — 拍前草稿（cheat-seed 写 / 用户写 / 用户改写都在这里）
- `predictions/<...>.md` — immutable 预测日志（cheat-predict 写，hook 保护）
- `videos/<...>/` — 拍后才建（cheat-shoot 时建）
  - `script.md` — 你提供的实际拍摄稿（cheat-shoot 时询问"和 scripts/ 一致吗"）
  - `report.md` — T+3d 数据（cheat-retro 写）

```
改写 draft →                  直接在 scripts/<...>.md 上覆盖
打分稿子 →                    "打分这篇 scripts/<...>.md"
准备发布前 →                  "启动预测 scripts/<...>.md"（写 immutable 预测）
拍完后 →                      "拍了 scripts/<...>.md"（建 videos/，问稿子是否一致，buffer +1）
发布后 →                      "已发布 https://..." (buffer -1)
T+3 天 →                      "复盘 videos/<...>/"
任何时候 →                    "状态"
日常补充候选池 →              "抓热点"
重新 brainstorm 一批选题 →    "找选题" 或 "seed"
```

> **没发过 vs 发过**：cheat-init 时问 "你这个频道发过视频吗？"
> - 没发过 → cheat-seed brainstorm（兴趣 × 热点）
> - 发过（不管 1 条还是 100 条）→ 装 adapter 抓回历史 + 建 video folder + reconstructed predictions，然后**也跑 cheat-seed**（带"你过去做过什么"的 context brainstorm）
> 两条路最终走同一工作流，区别只是 brainstorm 时 context 多一份。

### 节奏 + Buffer 警戒（v1 新增）

每次 Claude Code 会话开场，**SessionStart hook 自动报告 4-6 行状态**（无需主动问）：

```
[cheat-on-content / SessionStart 状态报告]

📦 Buffer: 3 篇 🟢 绿 (按 cadence 1d = 3 天预备)
⏰ 待复盘: 1 篇 (最早: 2026-05-01)
🎯 候选 top 3: 标题1 / 标题2 / 标题3
📅 上次抓热点: 2 天前

（不要主动开始任何动作——等用户决定。说"状态"看完整看板。）
```

Buffer 颜色由 `target_publish_cadence_days`（cheat-init 时问的发布频率）派生：
- 🔴 红：明天可能断更，**只推稳分**
- 🟠 橙：偏低，应该拍 1-2 条
- 🟢 绿：正常
- 🔵 蓝：积压，**暂停拍摄**先发存货

详见 [shared-references/cadence-protocol.md](shared-references/cadence-protocol.md)。

完整协议见 [SKILL.md](SKILL.md)。子 skill 细节见 `skills/cheat-*/SKILL.md`。

---

## 三条不可妥协的原则

这三条是方法论的脊柱。任何一条被违反，整个校准循环就退化成"凭直觉的自我安慰"。

1. **盲预测（Blind prediction）**：预测必须在看到任何实际数据**之前**写完。一旦写完，`## 预测` 段是不可修改的——只能往 `## 复盘` 段追加。**v1 升级：`hooks/prediction-immutability.sh` 在 PreToolUse 上物理阻塞**。完整规范：[shared-references/blind-prediction-protocol.md](shared-references/blind-prediction-protocol.md)。

2. **升级 = 全量重打（Bump = full re-score）**：当 rubric 升级（v2 → v2.1：权重变了 / 维度增减 / 公式改了），**所有有实绩数据的旧样本必须用新公式重打分**。新排序与实际表现排序若在 ≥4/5 样本上不一致 → 升级被拒。**v1 升级：跨模型独立审核（外部 LLM 重判一次）成为强制步骤**。完整规范：[shared-references/bump-validation-protocol.md](shared-references/bump-validation-protocol.md)。

3. **rubric 是工作台，不是博物馆**：被新数据推翻的观察 → **删掉**；被验证并吸收为正式维度的观察 → **也删掉**（维度本身就是新归宿）。绝不留"我曾经以为 X，但其实..."的考古层。git history 才是真正的归档。完整规范：[shared-references/observation-lifecycle.md](shared-references/observation-lifecycle.md)。

---

## 项目状态

### v1 已交付（批次 1 + 2 + cold-start 增强）

- [x] **方法论**（5 阶段流程 + v0/v2 公式 + v2.1 候选）
- [x] **参考实现**：一个中文观点视频博主，已发 25+ 视频，T+3d/7d/8d 数据回收完成 → 是 v2 公式的来源
- [x] **路由器主 SKILL.md**（明确触发词 → 子 skill 表）
- [x] **11 个子 skill** — 完整 SKILL.md（新增 `/cheat-shoot`：登记拍摄 + buffer 跟踪）
- [x] **7 份 shared-references 协议**（含 state-management.md / cadence-protocol.md）
- [x] **hooks/prediction-immutability** — 端到端验证 5/5 通过
- [x] **hooks/session-start** — SessionStart 自动报告（端到端 4 场景验证通过）
- [x] **hooks/meta-logging** — 被动使用记录（异步，不阻塞）
- [x] **Buffer 警戒系统**（cadence-protocol.md + cheat-shoot + cheat-publish 配对，按 target_publish_cadence_days 派生颜色阈值）
- [x] **script_patterns.md 写作 pattern 沉淀**（cheat-seed 写 draft 前必读，cheat-retro 复盘后建议追加新 pattern）
- [x] **starter-rubrics**: opinion-video.md（v2 已校准）+ opinion-video-zero.md（v0 cold-start，含比率桶 + cold-start 战略）
- [x] **tools/score-curve.py** — 已对真实 8 篇预测验证
- [x] **9 份 templates**（含视频分析的真实示例数据 + content.db.schema.sql 已 sqlite 验证 + prediction-cold-start.template.md）
- [x] **比率桶方案**：cold-start 用相对倍数桶，N=5/10 自动建议切绝对/percentile
- [x] **cold-start 简化预测**：前 5 篇只要 7 维打分 + 一句话 bet，不强求 bucket 数字
- [x] **adapters/trend-sources stubs**: weibo-hot.md + zhihu-hot.md（schema only，过渡期由 WebFetch 实现）

### v1 余项（批次 3，按需做）

- [ ] `tools/md-to-sqlite.py` 升级脚本（schema 已就位）
- [ ] `tools/validate-bump.py`（校准池全量重打的独立 CLI）
- [ ] `adapters/trend-sources/` 完整实现（专用 fetch 脚本替代 WebFetch + 加更多源：bilibili / 即刻 / hackernews / reddit）
- [ ] `adapters/perf-data/` + `adapters/candidate-pool/` 全套
- [ ] `starter-rubrics/long-form-essay.md` + `short-form-text.md`
- [ ] `examples/reference-implementation/` 视频分析脱敏快照
- [ ] License 决定（大概率 MIT）

---

## 文件树

```
cheat-on-content/
├── SKILL.md                                  ← 总协议 + 路由
├── README.md                                 ← 本文件
├── skills/
│   ├── cheat-init/SKILL.md                   ← 入口 onboarding
│   ├── cheat-seed/SKILL.md                   ← Cold-start 选题启动器（brainstorm + 可选 draft）
│   ├── cheat-score/SKILL.md                  ← 单稿打分（无写入）
│   ├── cheat-predict/SKILL.md                ← 盲预测 + immutable 日志
│   ├── cheat-publish/SKILL.md                ← 发布元数据登记
│   ├── cheat-retro/SKILL.md                  ← 数据回收 + 复盘
│   ├── cheat-bump/SKILL.md                   ← rubric 升级（含跨模型审）
│   ├── cheat-recommend/SKILL.md              ← 候选池排序推荐
│   ├── cheat-trends/SKILL.md                 ← 日常补充候选池（多 adapter 热点抓取）
│   └── cheat-status/SKILL.md                 ← 状态看板
├── shared-references/
│   ├── blind-prediction-protocol.md          ← 原则 #1
│   ├── bump-validation-protocol.md           ← 原则 #2
│   ├── observation-lifecycle.md              ← 原则 #3
│   ├── prediction-anatomy.md                 ← 一份合格预测的 7 个组件
│   └── candidate-schema.md                   ← 候选项统一 schema
├── starter-rubrics/
│   ├── opinion-video.md                      ← v2 已校准
│   └── opinion-video-zero.md                 ← v0 cold-start
├── hooks/
│   ├── prediction-immutability.json          ← 阻塞型钩子（PreToolUse）
│   ├── prediction-immutability.sh            ← 拦截脚本
│   ├── meta-logging.json                     ← 被动钩子
│   └── log-event.sh                          ← 日志脚本
└── tools/
    └── score-curve.py                        ← 预测精度收敛曲线
```

---

## 借鉴的工程思想

本项目的工程结构借鉴 **[ARIS](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep)**（一套用 Markdown 写成的可移植研究流水线 skill 集合，由社区维护，作为 ARIS 项目的下游应用）：

- **多子 skill + 路由主 SKILL.md** — 抄 [ARIS research-pipeline](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/research-pipeline)
- **shared-references 共享协议组织** — 抄 [ARIS shared-references](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/shared-references)
- **state file + 子 skill 共享上下文** — 抄 [ARIS auto-review-loop](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/auto-review-loop)
- **多 adapter 热点抓取** — 抄 [ARIS research-lit](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/research-lit) + [arxiv](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/arxiv) + [semantic-scholar](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/semantic-scholar)
- **跨模型独立审核** — ARIS 的核心 insight（执行者不能审自己），应用到我们最高风险的 bump 决策点
- **被动钩子记录** — 抄 [ARIS meta_logging.json](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/blob/main/templates/claude-hooks/meta_logging.json)

**独立创新**：[`hooks/prediction-immutability.sh`](hooks/prediction-immutability.sh) 是阻塞型钩子——ARIS 全套钩子都是异步被动记录，没有 blocking。这是把"immutable 预测"从君子协定升级为 harness 强制的关键。

---

## License

待定（大概率 MIT）。

---

*是的，他们会说这是作弊。拼写检查也曾是。Google 也曾是。未来不奖励努力——它奖励先看见的人。*
