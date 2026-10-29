# 动效与微交互数值

没有现成动效体系时的起点。大部分数值来自 Gabriel（[@gabriell_lab](https://x.com/gabriell_lab)）的设计工程笔记，标"整理"的是归纳。产品界面优先克制：能用颜色和位移说明的，不加缩放、旋转和模糊。

## 默认值

- 时长：微交互 200–300ms；退出比进入略快。（整理）
- 缓动：默认 ease-out 用 `cubic-bezier(.22,1,.36,1)`。（整理，[1](https://x.com/gabriell_lab/status/2094801103904801164)）
- 遵守 `prefers-reduced-motion`：减弱为淡入淡出或直接切换。

## 场景与数值

| 场景 | 做法 / 数值 | 来源 |
| --- | --- | --- |
| 可打断的状态（hover、press、切换） | spring，stiffness 520，damping 38，保留当前速度，不从 0 重启 | [原帖](https://x.com/gabriell_lab/status/2077366193766232092) |
| 调弹簧手感 | 用 visualDuration（如 0.5），不用 duration | [原帖](https://x.com/gabriell_lab/status/2090129262065971584) |
| 展开 / 收起 | `grid-template-rows: 0fr → 1fr`，不要对 `height: auto` 做动画 | [原帖](https://x.com/gabriell_lab/status/2071947977766117468) |
| 多步流程（shared axis） | 进入 translateX 24px→0，260ms；退出 0→-24px，220ms | [原帖](https://x.com/gabriell_lab/status/2076686595294363837) |
| 网格 / 列表入场 | 每项错开约 50ms；y 12→0 | [原帖](https://x.com/gabriell_lab/status/2075575648282722679) |
| 层级入场 | 先容器后内容：卡片每张延迟约 64ms，文字在卡片之后 | [原帖](https://x.com/gabriell_lab/status/2056711072611029300) |
| 菜单项选中 | 高亮灭 90ms、亮 90ms，再关闭菜单（macOS 做法） | [原帖](https://x.com/gabriell_lab/status/2104946256950813044) |
| 不可用的操作 | hover 时轻微 shake，约 280ms | [原帖](https://x.com/gabriell_lab/status/2094801103904801164) |
| 橡皮筋拖拽 | `(1 - 1/((x*c)/d + 1)) * d`，c = 0.55，越拉阻力越大且有上限 | [原帖](https://x.com/gabriell_lab/status/2109996591461355748) |
| 拖拽边界 | 弹性很小（如 dragElastic 0.08），关掉惯性，配吸附点和松手弹簧 | [原帖](https://x.com/gabriell_lab/status/2065821179810644410) |
| 可拖拽分栏 | 吸附 25 / 50 / 75 | [原帖](https://x.com/gabriell_lab/status/2100219330373849371) |
| 滚动到边缘 | 加阻力、回弹或减速，不要硬停 | [原帖](https://x.com/gabriell_lab/status/2074847639305060372) |
| 弹窗出现 | 背景变暗，同时背后页面轻微缩小 | [原帖](https://x.com/gabriell_lab/status/2079908738538860705) |
| 快速滚动时的模糊导航栏 | blur 24px → 8px，停止 120ms 后恢复 | [原帖](https://x.com/gabriell_lab/status/2054901923476607068) |
| 卡片重排 | 稳定的 layoutId，位置、尺寸、圆角一起动画 | [原帖](https://x.com/gabriell_lab/status/2089016710745534571) |
| 移动路径 | 直线显得机械，加轻微弧线 | [原帖](https://x.com/gabriell_lab/status/2104194013314097321) |
| 同一属性两个动画 | `animation-composition: add` 叠加，避免跳回 | [原帖](https://x.com/gabriell_lab/status/2093712246115287470) |
| 抬起物体 | 影子留在原地并缩小淡出（opacity .5→0，scaleX 1→.35） | [原帖](https://x.com/gabriell_lab/status/2063591609392840984) |
| 靠近感应（Dock、工具栏） | 光标 120px 内按距离放大，最大 1.5 倍 | [原帖](https://x.com/gabriell_lab/status/2060336070059864461) |
| 输入框预判 | 光标在 180px 内时焦点环按 `(1 - 距离/180)²` 渐显 | [原帖](https://x.com/gabriell_lab/status/2065039972483182708) |
| 方向感悬停 | 按光标进入方向，内容从对应方向偏移约 20px 进入 | [原帖](https://x.com/gabriell_lab/status/2085730524736143521) |

玩味型效果（速度拉伸、Magnetic、Tilt、Spotlight、Morph）只用于营销页或少量重点元素，产品界面慎用。
