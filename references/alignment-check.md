# 交付前对齐检查（必须做）

光看代码或"感觉对齐了"不算检查。每次改动界面后，在实际渲染的页面上量一次，量完再交付。

## Web：量主要区块的边缘

1. 打开目标页面，在 1280 和 1440 两个视口宽度下各做一次（移动端页面用 375 和 430）。
2. 在浏览器控制台或 Playwright 里运行下面的脚本。`selectors` 换成本页的主要区块：面包屑、页面标题、筛选栏、说明文字、各卡片、图表、提示条、表格。

```js
const selectors = ['header h1', '.filter-bar', '.page-desc', '.metric-card', '.chart-card', '.notice'];
const vw = document.documentElement.clientWidth;
const rows = [];
for (const sel of selectors) {
  document.querySelectorAll(sel).forEach((el, i) => {
    const r = el.getBoundingClientRect();
    if (r.width === 0) return;
    rows.push({ el: `${sel}[${i}]`, left: Math.round(r.left), right: Math.round(r.right), top: Math.round(r.top), clippedRight: r.right > vw + 0.5 });
  });
}
console.table(rows);
console.log('页面横向溢出:', document.documentElement.scrollWidth > vw);
```

3. 按"列"分组比对：
   - 同一列的区块（标题、说明、卡片外框、筛选栏外框）左边缘相差超过 1px 即不合格。
   - 卡片内的文字左边缘之间也要一致（同为卡片内边距）。
   - 并排卡片的上边缘一致，间距相等。
   - `clippedRight` 为 true 或页面横向溢出为 true 即不合格，除非是有意的横向滚动区。
4. 不合格就改，改完重新量，直到全部通过。
5. 数值通过后再看截图：图标、文字混排、不对称图形按 SKILL.md 的视觉对齐规则做光学检查。数值相等不等于视觉对齐，但数值不等一定要先修。

常见原因：标题区和内容区用了不同的 padding；卡片外框和外面的文字一个算边框一个算内容；某个容器多包了一层 padding；固定宽度或 `min-width` 撑出视口。

## iOS

- 在模拟器最小和最大屏幕上截图，用 Xcode 的 Debug View Hierarchy 读各区块的 frame，按同样规则比对左边缘（同列相差超过 1pt 不合格）和是否超出安全区。
- 能写 UI 测试时，用 `XCUIElement.frame` 断言同列元素的 `minX` 一致。

## 交付时附上

- 坐标表（元素、左、右、上、是否被裁切），标出改前不合格的行和改后的值。
- 改前、改后截图，注明视口宽度。
- 没能运行页面或量不了时，明确写"未验证对齐"及原因，不说已对齐。
