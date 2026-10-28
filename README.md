# app-design

一个用于 **App 端视觉设计** 的 agent skill：Flutter / SwiftUI / Compose 原生 UI 的重塑、美化、去模板化。强调独特艺术方向、签名元素与平台原生质感，拒绝模板化 AI 风。

> 面向 opencode / Claude 一类支持 `SKILL.md` 的 agent 运行时。纯提示词实现，无依赖、无脚本、不联网也能用。

## 它解决什么问题

| 常见失败 | 本 skill 的做法 |
|---|---|
| 出来一股「AI 风」：紫蓝渐变、圆角卡片堆叠、无差别插画 | 先定**艺术方向与签名元素**，再动手；判据来自哲学层 §俗套校准表，落地时逐项过 |
| 三端代码各写一套视觉规则 | 按技术栈分流（Flutter / SwiftUI / Compose），每端给**平台原生质感**的具体做法（材质、层级、动效、字体） |
| 设计稿层面的建议，落到代码就散 | 「两遍工作法」：第一遍定方向，第二遍按平台实现层逐项落地，附自检清单 |
| iOS / Android 各自的系统能力被忽略 | 平台实现层分节覆盖 Liquid Glass、Material 3 表达、尺寸类与折叠形态、系统字体与动态字号 |
| 做完没人验收 | 「原型验收条款」消费哲学层 §交付闸门，交付前逐条自查 |

## 何时触发

用户要求**重塑 / 美化 / 去模板化** App 端 UI：Flutter Widget、SwiftUI View、Compose UI、iOS 界面、Android 界面、移动端原生界面，或提到「这个页面太模板了 / 没有设计感 / 想要更高级的质感」且目标平台为 App。

不负责：Web 端视觉（用 frontend-design）、无障碍专项审计（用 accessibility）、从零产品的 PRD→原型全流程（用 product-prototype-generation）。

## 安装

```bash
# 方式一：整个目录放到 skills 根目录
git clone https://github.com/liaochzh/app-design-skill.git
cp -R app-design-skill ~/.config/opencode/skills/app-design

# 方式二：软链（便于跟随仓库更新）
ln -s "$(pwd)/app-design-skill" ~/.config/opencode/skills/app-design
```

不同运行时的 skills 目录不同（例如 Claude Code 的 `~/.claude/skills/`），把本目录放进去即可，`SKILL.md` 的 frontmatter（`name` / `description`）就是触发依据。

## 结构

```
SKILL.md     # 主体：触发条件 → 哲学层引用 → 步骤0 → 技术栈检测 → 平台实现层 → 字体加载 → 边界 → 示例 → 验收 → 维护说明
```

## 设计要点

- **哲学层是唯一权威源**：视觉立场、设计原则、两遍工作法、通用自检清单、输出模板都在同一份共享文档里（`design-spec` 仓的 `references/ui-design-philosophy.md`），本文件只写 **App 端专属**落地，不复制通篇。文件头给出该文档的路径与「配套版本」锚。
- **判据不重复**：平台实现层引用哲学层已有条目，只补 App 端差异（材质与层级、折叠姿态、系统字体与动态字号、动效曲线），避免同一规则两处维护而漂移。
- **锚语义**：维护说明里的「配套 vX.Y.Z」= 本文件内容最后一次同步的哲学层版本；哲学层未触及本文件章节时允许滞后。

## License

MIT（见 `LICENSE`）。
