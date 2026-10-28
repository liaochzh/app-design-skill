---
name: app-design
description: 高质量 App 端视觉设计。Flutter、SwiftUI（iOS）、Compose（Android）原生 UI 的视觉重塑/美化/去模板化。强调独特艺术方向、签名元素与平台原生质感，拒绝模板化 AI 风。仅做视觉层；不用于从零产品的 PRD→原型全流程（用 product-prototype-generation，全局可用）、不用于无障碍专项审计（用 accessibility）、不用于 Web 端视觉设计（用 frontend-design）。
license: MIT-0
metadata:
  version: "1.3.0"
  platforms: "flutter,ios,android"
  last-updated: "2026-10-27"
---

# App Design V1.3.0（App 端）

## 触发条件
- 用户要求**重塑/美化/去模板化** App 端 UI：Flutter Widget、SwiftUI View、Compose UI、iOS 设计、Android 设计、移动端原生界面
- 关键词：移动端设计、App 美化、Flutter 主题、SwiftUI 样式、Compose 主题、原生 UI、屏幕视觉
- **仅做 App 视觉层**；Web 端 → 用 `frontend-design`
- **不接 PRD 流水线、后端、部署**；从零产品想法→PRD→原型 → 用 `product-prototype-generation`

## 哲学层引用（开工前必读者）
- **执行本 Skill 前，先读取 `~/.config/opencode/design-spec/design-spec/references/ui-design-philosophy.md`**，获取设计立场、设计原则、两遍工作法、通用自检清单、输出模板与纪律。
- 本文件只负责 **App 平台实现层**（Flutter / iOS / Android），哲学层内容不在此重复。

### 加载分层（省 token，禁止全量读取哲学层）
- **L0 常驻**：快速决策树 + 四层模型（每次必读；任何任务都须对照俗套校准表）
- **L1 任务相关**：新建界面读 栅格与对齐 / 组件基线落地 + Token/色彩/字体/间距；改造读俗套表+交互（含边界状态）；多端/大屏读自适应布局；多页/衔接读跨页骨架 + 页面衔接；形态/输入读形态与共存；语言/RTL 读国际化与 RTL；涉及 AI 生成 / 改写则读生成式 AI 基线；兼容/机型读兼容与系统下限；图标读图标规范；贴图读贴图规范+合规清单；交付前读交付闸门（记录项回填）
- **L2 参考**：无障碍完整条款 / 额度 / 版本演进 / 反面案例库（仅冲突或审计时读）

## 步骤0：推断优先，不阻断
- 能推断直接采用，不要追问：主题/受众/任务、技术栈（按下方检测规则）、交付形态（单屏/多屏/组件）。
- 仅当缺失会显著跑偏时追问 1 个最关键的。
- 推断假设在最终交付开头 `假设` 一行显式标注。

## 技术栈检测（按优先级）
1. **Flutter**：检测到 `pubspec.yaml` → Flutter 分支
2. **iOS 原生**：检测到 `*.xcodeproj` / `Package.swift` / `*.swift`（无 Flutter）→ SwiftUI 分支
3. **Android 原生**：检测到 `build.gradle` / `build.gradle.kts`（无 Flutter）→ Compose 分支（Compose-first；XML Views 仅维护模式）
4. **混合项目**（Flutter + 原生插件）：以 Flutter 为主分支，原生代码跟随 Flutter 主题色/字体
5. **无项目上下文**：追问用户目标平台

## 平台实现层

### 栅格与对齐
- 栅格：8pt 基准 + 4pt 半档；同屏 padding 只用栅格值。
- 同容器单对齐轴；左右边界对齐 `layout/gutter`；跨字号并排按文本基线；数值列右对齐（`tabular-nums` 等价：数字等宽字形）。
- 列表/卡片用统一 `contentPadding`，不逐屏写 magic number；Compose 用 `Arrangement.spacedBy`，SwiftUI 用 `spacing:`，Flutter 用 `SizedBox`/`gap` 统一值。

### 组件基线落地
- 一类控件只有一处定义（`ThemeExtension` / 自定义 `ViewModifier` / Flutter `ThemeData` 组件主题）；页面只组合与选变体。
- `control-size`：44 / 48 / 56（pt / dp）；命中区域 ≥ §触达尺寸 取最严值，视觉小于命中区时补 padding。
- 圆角角色 surface / control / pill，取值来自主题，不在 Widget/View 内写字面值。
- 状态集按适用：default / hover（指针设备）/ focus / pressed / disabled / loading / error；数据视图另需 empty / loading / error。
- 语义固定写法：表单 → 字段 + 可见标签；选项集 → 分段控件；危险操作 → 确认弹窗；空数据 → 空态 + 下一步。

### 跨页骨架
- Scaffold 槽位固定（topBar / content / bottomBar|rail），高度与内边距入主题；不使用逐页自定义高度。
- 导航形态随宽度切换（bottomBar → rail → drawer），当前态同位置同视觉。
- 跨页不变量：品牌标识、主导航、主 CTA、帮助入口，同位同尺寸同顺序。

### 页面衔接
- 层级模型 push / modal / replace / reveal；转场用系统默认（`NavigationStack` / Compose Navigation），不自造参数。
- 共享元素每次 1 个（`matchedGeometryEffect` / Shared Transition）；无转场时信息完整。
- 返回恢复滚动、筛选、已填输入；Predictive Back 走系统回调。
- Reduce Motion / 移除动画时降为瞬时。

### 兼容与系统下限
- 下限声明：iOS 17+ / Android 10+；低于下限只保可用不保视觉。
- API 分支用 `#available` / `Build.VERSION`，不用型号或屏宽判断。
- 验证矩阵：360dp / iPhone SE / 标准 / 大屏 / 平板 × 系统字号 130% / 200% × 深色 / 浅色；未测写 N/A + 理由。
- 真机取证交 `debug-loop`，交付只贴预览/模拟器抽样截图清单。

### 跨端系统落地（消费哲学层系统层，先于分支选择读）

#### 三层 Token 映射到主题系统
- Primitive → 主题系统内的原始数值（不进组件）
- Semantic → `ColorScheme` / `Color` 命名色 / `MaterialTheme.colorScheme`（`surface`/`onSurface`/`error` 等角色名）
- Component → Widget 只消费角色名，**禁止硬编码色值/字号/间距**
- 全局层 Token 一次定；产品层覆盖须说明

#### 字号单位（最易错）
- iOS 正文 **17pt**（不是 16）；支持 Dynamic Type，不锁死字号
- Android 单位必须用 **`sp`**（不是 `dp`）；支持 Font Scale
- 移动端**不使用** `clamp()` 视口缩放

#### 高度（Elevation）
- Android M3：`tonalElevation`（改**表面色**）与 `shadowElevation`（改**投影**）**分离**，M3 方向是 tonal

#### 暗色 = 语义重映射，不是反转
- 走哲学层「色彩规范」的语义映射表；**禁止**把亮色 token 直接反转
- 本 skill 三分支（Flutter/SwiftUI/Compose）的自检项已同步为「语义重映射」

#### 平台设计语言硬规则（消费哲学层，不可违反）
- **iOS Liquid Glass（iOS 26+）**：玻璃只用于导航层，**禁止内容层、禁止玻璃嵌套玻璃**（例外：内容层的滑块 / 开关等瞬态交互控件仅在操作瞬间呈现玻璃外观，非常驻——细则见哲学层 §平台设计语言硬规则 · Apple）；文字永远在实色层，不压玻璃；Reduced Transparency / Increased Contrast / Reduced Motion 自动生效
- **iOS 全屏背景（iOS 26+）**：铺满屏的背景图延伸到 sidebar / toolbar / tab bar 之下；被压住关键内容时用系统 `backgroundExtensionEffect`（SwiftUI `.backgroundExtensionEffect()` / UIKit `UIBackgroundExtensionView`），**不自绘镜像 / 模糊 / 渐变遮罩**（判据与场景区分见哲学层 §平台设计语言硬规则 · Apple）
- **Android Material 3 Expressive**：形状**克制使用**（官方警告滥用致混乱）；弹簧物理动效取代标准 easing
- **iOS SF Symbols**：渲染模式显式且同界面单一（monochrome / hierarchical / palette / multicolor），状态变化仍走填充度
- **触觉（iOS/Android）**：只做结果确认 / 警告 / 选择三类，不绑装饰动画（签名靠位置与节奏，不靠震动时长）

#### 跨端 AI 基线（消费哲学层 §交互规范）
- 明确标识 AI 参与、结果可编辑 / 撤销 / 重试、不可逆操作先确认、可能出错的内容不作确定事实呈现、意图模糊给多版本、等待用具体进度文案、隐私端侧优先（完整 7 条见哲学层 §交互规范 · 生成式 AI 交互基线，本文件不重述）；无 AI 功能则跳过

#### 自适应布局（消费哲学层 §自适应布局）
- 分支按**窗口尺寸类**而非机型或物理屏宽：Android `WindowSizeClass`（compact / medium / expanded，桌面另有 large / extra-large）、iOS size class（compact / regular；`horizontalSizeClass` / `traitCollection.horizontalSizeClass`）；读窗口宽用 `MediaQuery.sizeOf` / `LayoutBuilder`（Flutter）、`WindowSizeClass`（Compose）；布局分支禁用 `Build.MODEL` / `UIScreen.main.bounds` / UA 判断；窄窗与分屏下主操作不被遮挡、无横滚
- **先选模式再画**：list-detail（主从）/ supporting pane（主 + 辅）/ feed（单列流）择一；导航形态随宽度切换（bottom bar → rail → drawer）——Compose `NavigationSuiteScaffold`（M3 Nav3）/ Flutter `NavigationRail` + `Drawer` / SwiftUI `NavigationSplitView`，**不手写尺寸分支与返回动画**；宽度到 medium 但高度 compact（横屏手机）时不做双栏
- 大屏**加列不加字号**；Dynamic Type / Font Scale 放大时不得靠加大间距补偿（一屏两行即超限）；除显式横向画廊外，`overflow-x` 须在交付说明写出用途
- 滚动驱动动效（`animation-timeline` 等）只作渐进增强：须在 `@supports` 内并保留无动画可读态

#### 形态与共存（消费哲学层 §形态与共存）
- 键盘与安全区：Android 用 `WindowInsets.safeDrawing` / `imePadding` + `android:windowSoftInputMode="adjustResize"`（edge-to-edge 自 targetSdk 35 强制）；iOS 用 `keyboardLayoutGuide`（`followsUndockedKeyboard` 自 iOS 15；`usesBottomSafeArea` 自 iOS 17）；Flutter 用 `viewInsets` 收键盘
- 折叠按姿态：Compose Material 3 Adaptive `collectFoldingFeaturesAsState()` + `FoldingFeature`（`state` / `orientation` / `isSeparating` / `bounds`）；**无铰链角度**，不写角度分支
- 形态重建保状态：Compose `rememberSaveable`、Flutter `PageStorageKey` / `RestorationMixin`、SwiftUI `@SceneStorage`；旋转/分屏/折叠后不重放首屏动画
- 输入：hover / 笔不承载唯一反馈；无指针设备的关键操作与焦点按 `accessibility/platforms/*` 判定
- **共存不抢**：分屏 / 多窗口失焦时不弹模态、不自动播放音视频、不抢输入焦点（后台恢复口径见哲学层 §交互规范 · 边界状态）
- **IME 组合态**：组合期间回车只确认候选，不触发提交 / 搜索 / 校验（行为判据，细则见哲学层 §形态与共存）

#### 国际化与 RTL（消费哲学层 §国际化与 RTL）
- 镜像：iOS 前缘 / 后缘（`leading` / `trailing`）+ `semanticContentAttribute`；Android `start` / `end` + `android:supportsRtl`；Flutter `EdgeInsetsDirectional` / `Directionality`；媒体进度、时间轴等固有方向元素不镜像
- 文案：不写死宽高与按字符截断；标签 `min-width` + 换行；截断保留语义完整（可及名称细则交 `accessibility`）
- 排版：`lang` 驱动断行 / 标点 / 字体回退；CJK 与拉丁混排的字距与行高随语言调整
- 格式：数字 / 日期 / 货币 / 复数走系统本地化 API，双向文本用隔离标记

#### 边界状态（消费哲学层「交互规范」）
- 网络三态区分（无网络/弱网超时/服务端错误）；**空数据 vs 无权限必须区分**
- 低电量检测：iOS `NSProcessInfo.isLowPowerModeEnabled` / Android `PowerManager.isPowerSaveMode()`；触发后禁用非关键动画、动效降到 `motion/fast`，**只减装饰不减功能**
- 后台恢复：iOS `applicationWillEnterForeground` / Android `onResume`（**不阻塞主线程**）；数据新鲜度 5 分钟；**不重放进入动画**
- 长表单草稿本地自动保存；提交/支付**幂等**防重复

#### 触达尺寸（取最严）
iOS 44×44pt / Android 7–10mm / WCAG 24×24px

#### App 俗套表（消费哲学层「俗套校准表 · App」）
Expressive 形状滥用 · Liquid Glass 用在内容层（**禁用**）· 渐变胶囊按钮 · Emoji 当图标（**禁用**）。Brief 明示要才用属前者。

---

### Flutter 分支

#### 主题系统
- `MaterialApp` → `ThemeData`；需 iOS 原生控件时用其 `cupertinoTheme` 参数（配 `MaterialBasedCupertinoThemeData` 可调和两套主题）。纯 iOS 风格应用可用 `CupertinoApp` + `CupertinoThemeData`
- 颜色：`ColorScheme` 命名 Token（primary/secondary/surface/background/error + 自定义命名色）
- 字体：`TextTheme` 配置 Display/Body/Utility 角色，`GoogleFonts` 或自定义 asset
- 圆角/阴影/间距：统一在主题系统（`ThemeData` 或 `CupertinoThemeData`）中定义，不硬编码到 Widget

#### 签名元素库（至少选一个作为全屏记忆点）
- **Hero 动画**：`Hero` 跨屏过渡，配合 `animations` 包（`SharedAxisTransition`/`FadeThroughTransition`）承载主题叙事
- **SharedAxis 转场**：页面切换的轴向运动，暗示信息层级
- **CustomPainter**：自定义绘制（签名图形、分隔线、背景纹理）
- **Haptic 反馈**：关键操作配合 `HapticFeedback`，触觉即签名
- **Shimmer/渐变环境光**：克制的背景氛围动画

#### Flutter 俗套清单（Brief 明示要才用，否则换自由轴）

| 俗套 | 特征 | 何时可用 |
|------|------|----------|
| Material3 紫 | 紫色 primary + 浮动按钮 + 卡片列表 | 内部工具/B 端且 Brief 点名要 |
| 默认 Scaffold | 蓝色 AppBar + ListView + 白色背景 | 原型验证阶段且 Brief 点名要 |
| 圆角卡片堆砌 | 全页 16px 圆角卡片 + 灰色背景 | 电商/社交且 Brief 点名要 |

#### Flutter 质量自检（在哲学层通用清单之上）
- [ ] 主题系统统一（`ThemeData`/`CupertinoThemeData`）：无硬编码颜色/字号/间距
- [ ] 暗色模式：走**语义重映射**（哲学层色彩映射表），**不是 `brightness` 反转**；不自造纯黑纯白
- [ ] 无障碍：`Semantics` 包裹关键交互、`ExcludeSemantics` 去除装饰
- [ ] 性能：避免不必要 rebuild（`const` Widget、`RepaintBoundary`）
- [ ] 表单：错误态以文本 + `FocusNode` 驱动焦点，占位符不作标签（回指 3.3.1/3.3.2）
- [ ] 平台适配：单屏内不混两种设计语言（App 根主题经 `cupertinoTheme` 调和不算混用）
- [ ] 安全区：`SafeArea` 处理刘海/底部横条
- [ ] 对齐与栅格：padding 全走栅格；单对齐轴；数值列右对齐；无逐屏 magic number
- [ ] 组件唯一：控件只有一处定义；`control-size` 与命中区合规；状态集齐（含 empty/loading/error）
- [ ] 跨页与衔接：Scaffold 槽位一致；当前态同位；转场走系统；返回恢复状态

---

### SwiftUI 分支

#### 主题系统
- 自定义 `ViewModifier` + `Environment` 注入主题 Token
- 颜色：`Color` 扩展命名色（`Color.brandPrimary`）+ Asset Catalog 支持 Dark Mode
- 字体：`.font(.custom(...))` 注册自定义字体，Dynamic Type 兼容
- 间距/圆角：统一在 `Theme` 结构中定义

#### 签名元素库
- **matchedGeometryEffect**：跨视图几何过渡，承载主题叙事
- **自定义 Shape**：签名图形/分隔线/背景纹理
- **SensoryFeedback**（iOS 17+）：触觉反馈即签名
- **Phase Animator / Keyframe**：克制的首屏/转场动画
- **Canvas / TimelineView**：环境氛围动画

#### SwiftUI 俗套清单

| 俗套 | 特征 | 何时可用 |
|------|------|----------|
| 系统蓝白 | 系统蓝 + 白色背景 + 默认 List | 原型验证且 Brief 点名要 |
| 默认 NavigationStack | 系统导航栏 + List + NavigationLink | 工具类 App 且 Brief 点名要 |
| 全圆角卡片 | 16px RoundedRectangle + 灰色背景 | 社交/内容类且 Brief 点名要 |

#### 折叠（iPhone Duo）

- 分栏用 `NavigationSplitView`（或 `UISplitViewController`）：内屏常规宽走列表-详情，外屏 compact 回单栏；尺寸类别与机型禁令见「跨端系统落地」小节，不在此重述
- 折痕避让：优先系统组件（sheet / alert / contextMenu / split view 自动让位）；自定义锚定控件用 `ReservedRegion`（iOS 27.1+，以 SDK 为准），不写死铰链宽度、不自绘遮罩
- 跨折痕网格用偶数列 + 外层边距；跨折痕连续内容保持连续滚动（后者为本仓引申，依据见哲学层 §iPhone Duo 的归因行）
- 内屏被 Split View 分成两半时按当前可用宽度降级布局，不假设恒为双栏

#### SwiftUI 质量自检（在哲学层通用清单之上）
- [ ] 主题统一：无硬编码颜色/字号，全部走 Environment/ViewModifier
- [ ] Dark Mode：Asset Catalog 或 `Color(light:dark:)` 双模式
- [ ] Dynamic Type：`.font(...)` 支持系统字号缩放，不硬编码固定 size
- [ ] VoiceOver：`.accessibilityLabel` / `.accessibilityHint` 覆盖关键交互
- [ ] 表单：错误态以文本 + `@FocusState` 驱动焦点，占位符不作标签（回指 3.3.1/3.3.2）
- [ ] iOS HIG：遵循 Human Interface Guidelines，不混 Android 风格
- [ ] 安全区：`.safeAreaInset` / `.ignoresSafeArea` 正确处理
- [ ] SF Symbols：渲染模式显式且同界面单一，状态变化仍走填充度
- [ ] 折叠：无机型判断、无写死折痕宽度；开合与内屏半宽态下分栏不溢出、选中 / 滚动位置不丢
- [ ] 对齐与栅格：padding 全走栅格；单对齐轴；数值列右对齐；无逐屏 magic number
- [ ] 组件唯一：控件只有一处定义；`control-size` 与命中区合规；状态集齐（含 empty/loading/error）
- [ ] 跨页与衔接：Scaffold 槽位一致；当前态同位；转场走系统；返回恢复状态

---

### Compose 分支

#### 主题系统
- `MaterialTheme` / 自定义 `CompositionLocal` 注入主题 Token
- 颜色：自定义 `ColorScheme`（primary/secondary/surface/background + 自定义命名色）
- 字体：`Typography` 配置 Display/Body/Utility 角色
- 形状/间距/阴影：统一在 `Shape` / `Spacing` 对象中定义

#### M3 平台事实（2026）
- **Compose-first**：新 Android 项目默认 Compose；Material Views 1.14 为**最后一个 stable 发布**（官方 "all in on Compose"），此后仅维护。
- **spacing token**：M3 Expressive layout system 提供 8dp 基准 spacing token，可编程适配设备/密度；自定义 `Spacing` 应与 8dp 基准对齐。
- **Type scale**：支持按语言文字高度类别自适应行高；不要用固定行高覆盖，除非 Brief 明确要求。
- **Styles API**（Compose `@Experimental`；foundation 1.12+ / BOM 2026.04.01 起可用，Material 组件原生 style 参数官方称 future releases、截至 2026-10-26 未点名版本）：`StyleState` 驱动按压 / 选中 / 悬停态，替代样式参数（padding / 颜色）并避免重组；**不替代 modifiers**（布局 / 手势 / 语义仍归 modifier），实验期不挤掉 stable 默认位（边界见哲学层 §平台设计语言硬规则·M3）。

#### 签名元素库
- **AnimatedVisibility + Enter/ExitTransition**：克制的显隐动画
- **Custom Composable + Canvas**：签名图形/分隔线/背景纹理
- **HapticFeedback**：触觉反馈即签名
- **Shared Transition**（Compose 1.7+）：跨屏共享元素过渡
- **InfiniteTransition**：环境氛围动画

#### Compose 俗套清单

| 俗套 | 特征 | 何时可用 |
|------|------|----------|
| Material3 紫 | 紫色 primary + FAB + 卡片列表 | 内部工具/B 端且 Brief 点名要 |
| 默认 Scaffold | TopAppBar + LazyColumn + 白色背景 | 原型验证且 Brief 点名要 |
| 全圆角卡片 | 16dp RoundedCornerShape + 灰色背景 | 电商/社交且 Brief 点名要 |

#### Compose 质量自检（在哲学层通用清单之上）
- [ ] 主题统一：无硬编码颜色/字号/间距，全部走 MaterialTheme/CompositionLocal
- [ ] Dark Theme：走**语义重映射**（哲学层色彩映射表），**不是 `darkColorScheme` 反转**
- [ ] 无障碍：`contentDescription`、`semantics` 覆盖关键交互
- [ ] 表单：错误态以文本 + `focusRequester` 驱动焦点，占位符不作标签（回指 3.3.1/3.3.2）
- [ ] 性能：`remember` / `derivedStateOf` 避免不必要 recomposition
- [ ] Material3 规范：遵循 M3 指南，不混 iOS 风格
- [ ] 系统栏与键盘：`enableEdgeToEdge` + insets 避让，细则见「形态与共存」小节
- [ ] Predictive Back：返回走系统回调，不消费返回事件做业务逻辑
- [ ] Dynamic Color：仅作可选系统层，关掉后品牌识别仍成立
- [ ] Styles API：选用时样式参数走 Style，布局 / 手势 / 语义仍走 modifier；不作 stable API 的默认替代
- [ ] 对齐与栅格：padding 全走栅格；单对齐轴；数值列右对齐；无逐屏 magic number
- [ ] 组件唯一：控件只有一处定义；`control-size` 与命中区合规；状态集齐（含 empty/loading/error）
- [ ] 跨页与衔接：Scaffold 槽位一致；当前态同位；转场走系统；返回恢复状态

---

## App 端字体加载策略
- **Flutter**：`pubspec.yaml` 声明 `assets/fonts`，`GoogleFonts` 包可运行时加载
- **SwiftUI**：`Info.plist` 声明 `UIAppFonts`，Asset Catalog 注册
- **Compose**：`res/font` 目录声明，`FontFamily` 引用

## 与其他 Skill 边界
- 要走 `PRD→模板渲染` 流水线（多页原型）→ 用 `product-prototype-generation`。
- 要定制视觉身份、单屏/组件级打磨、去 AI 味 → 用本 Skill。两者不混调。
- Web 端视觉设计（HTML/CSS/React）→ 用 `frontend-design`，本 Skill 不覆盖。
- 无障碍专项审计/修复 → 交 `accessibility`，本 Skill 只保底线。
- 视觉产出评审→本 Skill 自检章节；技术方案/架构评审→`design-review`。

## 使用示例
- `美化这个 Flutter 设置页，要有质感` → 检测 pubspec.yaml → Flutter 分支 → 定主题+Token+签名（如 CustomPainter 分隔线）→ 主题系统统一
- `SwiftUI 首页视觉重塑，别像默认模板` → 检测 xcodeproj → SwiftUI 分支 → matchedGeometryEffect 签名+自定义 Shape
- `Compose 列表页去模板化` → 检测 build.gradle → Compose 分支 → AnimatedVisibility 签名+自定义 ColorScheme

## 原型验收条款（消费哲学层 §交付闸门）

App 记录项 5 条（**记录项**，非硬禁令，交付时自检回填）与 Web 淘汰闸门 11 项（无 Brief 依据时命中即淘汰）见哲学层 §交付闸门；本文件不重复。`PRD 即 Brief`。

AI：涉及生成式 AI 的界面须过哲学层 §交互规范 · 生成式 AI 交互基线（标识 / 可编辑·撤销·重试 / 不可逆先确认 / 不确定标注 / 等待 / 多版本 / 隐私）；无 AI 功能则跳过。

## 维护说明
- 版本：1.3.0（2026-10-27），哲学层共享 `design-spec/references/ui-design-philosophy.md`（配套 v2.5.0）
- 锚语义：上面「配套」= 本文件内容**最后一次同步**的哲学层版本；哲学层仅兼容且未触及本文件章节时允许滞后，触及须 bump 本文件版本并同步锚（见哲学层 §维护说明；校验 `design-spec/scripts/check-design-spec.py --only anchors`）
- v1.3.0：新增 5 节（栅格与对齐 / 组件基线落地 / 跨页骨架 / 页面衔接 / 兼容与系统下限）+ 三分支自检各 3 行；L1 补 5 章路由；配套哲学层 v2.5.0
- v1.2.15：三分支质量自检各 +1 行（表单错误态以文本 + 平台焦点 API、占位符不作标签）；配套哲学层 v2.4.14
- v1.2.14：新增「国际化与 RTL」小节（镜像 API / 文案不写死 / 排版随语言 / 本地化格式）+ L1 路由；配套哲学层 v2.4.12
- v1.2.13：折叠（iPhone Duo）块「跨折痕连续内容保持连续滚动」条标明**本仓引申**（依据见哲学层 §iPhone Duo 归因行）；配套哲学层 v2.4.11（`ReservedRegion` 版本口径统一为「以 SDK 为准」）
- v1.2.12：镜像审计补 3 处（皆消费哲学层既有判据，无新增口径）——`自适应布局` 补第 2 条（模式选择 + 导航随宽度切换）；`形态与共存` 补 2 条（失焦共存、组合输入回车不提交）；锚不变 v2.4.10
- v1.2.11：`跨端系统落地` 补「iOS 全屏背景（iOS 26+）」条（系统 `backgroundExtensionEffect`，不自绘镜像 / 模糊 / 渐变遮罩）；配套哲学层 v2.4.10（含锚滞后 2 位收口）
- v1.2.10：配套哲学层 v2.4.7（版本/指针同步，无新增条款；本批改动在哲学层 §俗套校准表 · App 的例外指针与结构整理，本文件摘要不变）
- v1.2.9：SwiftUI 分支新增「折叠（iPhone Duo）」小节（`NavigationSplitView` 分栏 / 折痕避让与 `ReservedRegion` / 跨折痕偶数列与连续内容 / 内屏半宽按可用宽度降级）；SwiftUI 自检加折叠项；配套哲学层 v2.4.6
- v1.2.8：iOS Liquid Glass 条补瞬态例外（内容层滑块 / 开关在操作瞬间可呈玻璃外观，非常驻；细则回指哲学层 §平台设计语言硬规则 · Apple）；配套哲学层 v2.4.5
- v1.2.7：新增「形态与共存」小节（insets / IME、折叠姿态、形态重建保状态、输入能力）+ L1 路由；Compose 自检「系统栏」改指针避免同文件第二份 `WindowInsets`；配套哲学层 v2.4.4
- v1.2.6：新增「自适应布局」小节（窗口尺寸类、多窗口/分屏、字号放大不靠间距补偿、滚动驱动降级）+ L1 补多端/大屏路由；iOS size class 按 compact / regular 拆写（补 `horizontalSizeClass`）；AI 基线简写 6 标签补齐为 7 标签；配套哲学层 v2.4.3
- v1.2.5：Styles 条补「截至 2026-10-26 未点名版本」时效标注（配合哲学层时效守卫）；配套哲学层 v2.4.2
- v1.2.4：新增「跨端 AI 基线」小节（消费哲学层 §交互规范）+ 原型验收补 AI 项；配套哲学层 v2.4.1
- v1.2.3：Compose 分支「M3 平台事实」补 Styles API（实验期边界：替代样式参数、不替代 modifiers、不压 stable 默认位）并把 Compose-first 条订正为 Views 1.14「最后一个 stable 发布」+ Material Compose 1.5.0 预计 2026 H2 转 stable；Compose 自检加 Styles API 项；配套哲学层 v2.4.0
- v1.2.2：App 原型验收条款 5 条与 Web 闸门 1 行并入哲学层 §交付闸门（改指针）；加载分层补交付闸门；配套哲学层 v2.3.6
- v1.2.1：配套哲学层 v2.3.5（版本/指针同步，无新增条款）
- v1.2.0：平台硬规则补 SF Symbols 渲染模式与触觉边界；Compose 分支技术栈检测标 Compose-first；Compose 自检加 Predictive Back / Dynamic Color；SwiftUI 自检加渲染模式；三处版本号统一
- v1.1.1：Compose 分支新增「M3 平台事实（2026）」三行注记（Compose-first / Views 维护模式、8dp spacing token、type scale 语言高度自适应）
- v1.1.0：新增「跨端系统落地」章节（三层 Token 映射、17pt/sp、tonalElevation、Liquid Glass/M3 Expressive 硬规则、边界状态含低电量 API）；暗色「反转规则」改为「语义重映射」
- 三平台俗套清单/签名元素库可独立演进，不影响哲学层
