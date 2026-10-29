# 设计工程细节清单

实现层面让界面"稳、顺、不跳"的做法。来源同 motion.md。

| 问题 | 做法 | 来源 |
| --- | --- | --- |
| 弹窗锁滚动时页面右移约 15px | `html { scrollbar-gutter: stable; }` | [原帖](https://x.com/gabriell_lab/status/2087520044196507963) |
| 字体加载后布局跳动 | 回退字体用 size-adjust、ascent-override、descent-override 调到同尺寸 | [原帖](https://x.com/gabriell_lab/status/2081021694441914372) |
| 悬停加粗挤动相邻元素 | 改颜色，或用可变字体 GRAD 轴加深不变宽 | [原帖](https://x.com/gabriell_lab/status/2113978631127253395) |
| 多行输入出现嵌套滚动 | textarea 用 `field-sizing: content` 随内容增高 | [原帖](https://x.com/gabriell_lab/status/2061425854844924325) |
| 弹窗内滚动带动背景 | 弹窗内容加 `overscroll-behavior: contain` | [原帖](https://x.com/gabriell_lab/status/2082104815044235428) |
| 弹窗背景模糊卡顿 | backdrop-filter 放到独立伪层 | [原帖](https://x.com/gabriell_lab/status/2078471817715429591) |
| ⌘F 搜不到折叠内容 | 折叠区用 `hidden="until-found"` | [原帖](https://x.com/gabriell_lab/status/2103491403493847195) |
| Tooltip 翻转后动画方向不对 | 用 anchored container queries 检测实际方向 | [原帖](https://x.com/gabriell_lab/status/2084276632353690061) |
| 移动正在播放的 iframe 会重新加载 | 用 `moveBefore()`，不支持时再 insertBefore | [原帖](https://x.com/gabriell_lab/status/2108890580529099211) |
| 容器动画时文字被拉伸 | Motion 子元素用 `layout="position"` | [原帖](https://x.com/gabriell_lab/status/2091140734560669972) |
| 加载只有转圈 | 按场景选 Skeleton、Progress Bar、Optimistic UI、Blur-up、Stateful Button | [原帖](https://x.com/gabriell_lab/status/2110711852787539974) |
| AI 回复全是文字 | 适合时给可交互图表或控件，让用户能自己探索 | [原帖](https://x.com/gabriell_lab/status/2106008207797739985) |
| 点击没有反馈 | 菜单闪烁、按钮状态变化或轻微 shake 确认 | [原帖](https://x.com/gabriell_lab/status/2104946256950813044) |
| 控件常驻显得嘈杂 | 按意图出现：hover、focus、靠近或开始操作时再显示 | [原帖](https://x.com/gabriell_lab/status/2070497862543097929) |
