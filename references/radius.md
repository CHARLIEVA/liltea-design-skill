# 圆角：同心与连续曲率

来源：[@Kubai087 · 004 递进式圆角 + 连续曲率圆角](https://x.com/Kubai087/status/2115469767650021796)，按 liltea 的原则做了收敛。

## 同心圆角（等距内缩）

- 公式：内圆角 = 外圆角 − 内边距（r = R − p），内外两条弧共用一个圆心。
- 从外往里算：弹窗或卡片 → 面板、输入框、按钮 → 标签和 chip。
- 例：弹窗 32、内边距 16 → 面板和按钮 16；面板 16 里距边 8 的 chip → 8。
- 同级元素共用一个圆角。同一层里离外框距离不一致时（如贴边输入框和居中按钮），按最常见的内边距算一次，不逐个算出多个值。
- 算出来小于 4 时取 4；内边距大于等于外圆角时，内层按自身尺寸用圆角体系的值，不再套公式。
- 胶囊（高度一半）和圆形保持全圆，不参与公式。
- 内边距、间距、圆角都落在 4px 网格上。
- 项目有圆角 token 时改 token，不在组件里硬写数值。

## Web 连续圆角（超椭圆）

普通圆角在接直边处曲率突变，有一道"折痕"；连续圆角过渡更顺。CSS `corner-shape` 目前只有 Chrome / Edge 139+ 支持，Safari、Firefox 忽略并显示普通 `border-radius`，所以只做渐进增强：

```css
.card, .dialog, .panel, .input, .button {
  border-radius: 16px; /* 始终保留 */
}

@supports (corner-shape: superellipse(1.25)) {
  .card, .dialog, .panel, .input, .button {
    corner-shape: superellipse(1.25);
  }
}
```

- 只加在卡片、弹窗、面板、输入框、按钮等组件的类名或 token 上，不用 `:where(*)` 全局套用：头像、图片裁切、第三方组件、胶囊和圆形都不该变。
- 视觉不能依赖它：不支持的浏览器上效果也必须成立。

## 给 AI 的 prompt（可直接用）

```
Make the corner radii in the screens/components I name consistent, then add continuous corners. Ask me if I have not named them. If the project has radius tokens, change the tokens instead of hard-coding values.

Inner radius = outer radius - padding (r = R - p), so nested corners share one center. Work from the outside in: dialog/card -> panels, inputs, buttons -> chips and tags. Boxes on the same level share one radius, computed from the most common padding at that level. Never go below 4. Pills and circles stay fully round. Keep padding, gap and radius on a 4px grid.

Web: keep border-radius; add `corner-shape: superellipse(1.25)` inside `@supports` only on card/dialog/panel/input/button selectors, never globally.
iOS: use RoundedRectangle(cornerRadius:style: .continuous) / layer.cornerCurve = .continuous.

When done, list every radius you changed as "element: old -> new".
```

## 验收

- 列出所有改动："元素：旧值 → 新值"。
- 放大到 200–400% 看嵌套角：内外弧线间距均匀，不出现内角比外角"胖"的情况。
