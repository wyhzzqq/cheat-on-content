# Cheat on Content

> 一个把"凭感觉发内容"换成"每条都比上条更准"的 Claude Code 工作流。
> 也叫**网红外挂**。我用这套一个月涨粉 100w —— 不是灵感，是系统。

[![Version](https://img.shields.io/badge/version-v0.1.0-orange)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

---

## 它真正在干什么

90% 的创作者都活在同一个循环里：

> 凭感觉发 → 数据出来发现拉了 → 不知道为什么拉 → 下一条还是凭感觉

爆了不知道为啥爆，扑了学不到东西。一年发 200 条，"网感"长不到 10%。

**网红外挂**把每一篇都强行变成一次校准实验：

打分 → 盲预测 → 发布 → T+3 天复盘 → 进化你的评分公式

跑一个月 = 你有了一份**只属于你的爆款公式**。
跑三个月 = 你比刚开始的自己强 10 倍。

---

## 它和别的"创作工具"哪里不一样

| 别人 | 这个 |
|---|---|
| 给你"灵感" | 让你**自己的灵感被量化** |
| AI 帮你写 | AI 帮你**判**——稿子还是你的 |
| 一发发 10 个版本 A/B 测 | 一发就**赌**——把判断写下来，数据出来对账 |
| 静态数据看板 | **会进化的评分公式**——你三个月后的 rubric 已经不是初始版 |

一句话：别的工具帮你"产出更多"，这个工具帮你"判得更准"。

---

## 为什么有用

每篇内容发布前你给它打分、写预测、下 bet —— 全部留下完整决策日志（自动锁存，免得几天后被自己的后见之明覆盖）。

T+3 天数据出来，工具帮你算偏差、找规律。**三次同向偏差 → 自动建议升级你的评分公式**。

升级不是拍脑袋——所有历史样本必须用新公式重判，新排序与实际表现 ≥4/5 一致才放行；还要跨模型独立审核（防自欺）。

被推翻的观察删，被吸收的也删。git history 才是档案。**rubric 永远只放当下最有用的。**

---

## 安装

```bash
git clone https://github.com/XBuilderLAB/cheat-on-content.git
cd cheat-on-content
bash install.sh
```

13 个子 skill 软链接到 `~/.claude/skills/`。装一次，所有内容项目都能用。

> 冻结版本：`bash install.sh --copy` / 卸载：`bash uninstall.sh`（不动你的内容数据）

---

## 第一次跑

在你的内容项目目录里开 Claude Code，说：

```
初始化 cheat-on-content
```

5 个 yes/no 搞定 onboarding。**强烈建议导对标账号**——5-10 条样本 → 工具立刻有 anchor，不然前 5 篇预测精度 ±50%。

---

## 日常用法

```
打分这篇 scripts/<...>.md         → 评分
启动预测 scripts/<...>.md         → 盲预测 + 决策日志
拍了 scripts/<...>.md            → 建 video folder + buffer +1
已发布 https://...                → buffer -1
复盘 videos/<...>/                → T+3d 数据回收 + 复盘
状态 / 抓热点 / 找选题 / 升级 rubric / 找对标
```

每次开会话 hook 自动报告 buffer + 待复盘 + top 候选——你不用主动问。

完整工作流 + 子 skill 细节见 [SKILL.md](SKILL.md)。

---

## License

MIT。商用、改造、闭源接入都行。

---

*他们会说这是作弊。计算器也曾是。Google 也曾是。未来不奖励努力——它奖励先看见的人。*
