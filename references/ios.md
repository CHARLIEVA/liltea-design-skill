# iOS 端落地（SwiftUI / UIKit）

liltea 原则在 iOS 上的对应写法。优先系统组件和 Human Interface Guidelines；下列是没有现成规范时的默认做法。新 API 注明了最低系统版本，低于该版本要做兼容分支。

## 圆角

- 一律用连续曲率圆角：
  - SwiftUI：`RoundedRectangle(cornerRadius: 16, style: .continuous)`，或 `.clipShape(.rect(cornerRadius: 16, style: .continuous))`。
  - UIKit：`view.layer.cornerRadius = 16; view.layer.cornerCurve = .continuous`。
- 嵌套同心：内圆角 = 外圆角 − 内边距，同 `references/radius.md`。可以把外层圆角和内边距做成常量，内层用计算值，不手写。
- iOS 26+ 可用 `ConcentricRectangle` / 同心圆角形状让内层自动贴合容器圆角；低版本退回手算。
- 胶囊用 `Capsule()`，圆形用 `Circle()`，不用大圆角近似。

## 间距与布局

- 间距用 4 的倍数（8 / 12 / 16 / 24 / 32），抽成常量统一引用。
- 尊重安全区，不用硬编码偏移躲刘海和 Home 指示条。
- 可点击区域至少 44×44pt，视觉小的控件用 `.contentShape` 或 padding 扩大命中区。
- 支持动态字体（Dynamic Type）：用 `.font(.body)` 等文本样式，不写死字号；大字号下布局不截断、不重叠。

## 颜色与材质

- 用语义色（`Color.primary`、`.secondary`、`Color(.systemBackground)` 等）或项目颜色资源，自动适配深浅色。
- 遮罩和浮层优先系统材质（`.ultraThinMaterial` 等）或纯色半透明，不用渐变、不加发光。
- 平台原生材质（含 iOS 26 的 Liquid Glass）属于平台习惯，系统组件自带时保留，不自己仿造。

## 图标与字体

- 图标用 SF Symbols，同组统一 `symbolRenderingMode` 和字重；用 `.imageScale` / 文本样式让图标随字号缩放。
- 不用 emoji 代替功能图标。
- 纵向对比的数字用 `.monospacedDigit()`。
- 选中态不改字重，用颜色区分，避免宽度跳动。

## 动效与反馈

- 默认弹簧：`.spring(duration: 0.3, bounce: 0)`，或系统预设 `.smooth` / `.snappy`；需要轻微回弹再加 `bounce: 0.1–0.2`。SwiftUI 弹簧天然可打断并保留速度。
- 共享元素转场用 `matchedGeometryEffect`；iOS 18+ 的导航转场可用 `.navigationTransition(.zoom(...))`。
- 列表入场依次错开时延迟控制在每项 30–50ms 以内，总时长不拖沓。
- 遵守减弱动态效果：`@Environment(\.accessibilityReduceMotion)` 为 true 时用淡入淡出替代位移和缩放。
- 触觉反馈克制：确认、成功、错误等关键时刻用 `.sensoryFeedback`（iOS 17+）或 `UIImpactFeedbackGenerator`，不在滚动和普通点击上滥用。
- 加载：首屏用骨架（`.redacted(reason: .placeholder)`），按钮内用自身的加载状态，避免整屏 `ProgressView` 转圈。

## 验收

- 在目标机型的最小和最大屏、深浅色、最大动态字体、减弱动态效果打开时各截一张图（或录屏）。
- 用 Xcode 的 Accessibility Inspector 检查点击区域和 VoiceOver 标签。
- 没有真机或模拟器时说明限制，不声称已验证。
