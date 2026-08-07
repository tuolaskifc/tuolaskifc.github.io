# 彩蛋页 · 小游戏合集（/play/） — 设计文档

> 本文件是开发参考，不部署上线（已加入 `_config.yml` 的 exclude）。
> 输入 `play` 进入小游戏合集页，以后在这里加更多小游戏。

---

## 一、触发与流程

| 项 | 说明 |
|----|------|
| 触发词 | `play`（在网站任意页面键盘连续输入即跳转） |
| 彩蛋页 URL | `/play/` |
| 彩蛋页文件 | `easter-egg-play.html`（站点根目录，`layout: null`，`permalink: /play/`） |
| 触发逻辑位置 | `_layouts/default.html` 里的 `PLAY` 数组 + keydown 监听 |

**流程**：
```
访客在任意页面输入 play
  → default.html 的 keydown 监听匹配 PLAY 数组
  → window.location.href = '/play/'
  → easter-egg-play.html 加载（带全屏入场特效 LABORATORY）
  → 游戏列表屏 → 选「化学老虎机」→ 老虎机屏
```

> ⚠️ 改触发词：改 `_layouts/default.html` 里的 `PLAY` 数组。
> 例：改成 `game` → `const PLAY = ['g','a','m','e'];`

> ⚠️ 如果以后想加一个 **手机端入口**，可以仿照默认布局里「连点页脚 5 次」的写法，
> 把 `window.location.href` 指向 `/play/`。当前手机端只有连点进化学鉴定这一条。

---

## 二、页面结构

`easter-egg-play.html` 用两个 `.game-screen` 面板切换（`active` 类控制显示）：
- **`#screenMenu`（游戏列表屏）**：标题「小游戏合集」+ `.game-list` 里的游戏卡片
- **`#screenSlot`（老虎机屏）**：3 个转盘 + 结果文字 + SPIN 按钮 + 计分

```js
// 切换面板
document.getElementById('gotoSlot').addEventListener('click', () => {
  screens.menu.classList.remove('active');
  screens.slot.classList.add('active');
});
```

> ➕ **以后加新游戏**：在 `.game-list` 里加一个 `<button class="game-card">`，
> 复制一个 `.game-screen` 面板放新游戏，再加一段切换 JS 即可。

---

## 三、化学老虎机（GAME 01）

### 化学物质池（POOL）

18 个转盘符号，按稀有度分级（common 4x / rare 2x / legendary 1x 权重抽取）：

| rarity | 符号 | 名称 |
|--------|------|------|
| common | H C O N Na Fe Si S P H₂O | 氢 碳 氧 氮 钠 铁 硅 硫 磷 水 |
| rare | Au Ag Pt Hg Cu U | 金 银 铂 汞 铜 铀 |
| legendary | 💎 C₆₀ | 钻石 富勒烯 |

- 抽取函数 `randomItem()`：按权重展平成一个数组再随机取。
- 符号颜色：common 沙色 / rare 主红 / legendary 金色（`.slot-symbol.rare` / `.legendary`）。

### 判定规则（judge 函数）

| 情况 | 结果文案 | level | 加分 |
|------|----------|-------|------|
| 三连传说（💎/C₆₀） | 🌈 究极传说！三传说连击！ | jackpot | +100 |
| 三连相同·传说 | 🔥 传说三连！{symbol}！！ | jackpot | +80 |
| 三连相同·稀有 | ✨ 稀有三连！{symbol}！ | hit | +40 |
| 三连相同·普通 | ⚡ 三连！{name}！ | hit | +20 |
| 两连·传说 + 另一 | 🌟 传说成对！ | hit | +15 |
| 两连·其他 | 👍 {name} 成对！ | normal | +5 |
| 三枚全稀有/传说（非三连） | 🔮 全稀有阵容！ | normal | +8 |
| 其他 | ❌ 再试一次 | none | +0 |

- 判定顺序：**先判三连传说 → 三连相同 → 两连 → 全稀有 → 无**。
- 计分：`streak`（当前连击分，输一次清零）+ `best`（最高分，仅本次会话）。

### 旋转动画

- 每列一个 `setInterval` 每 60ms 换一个随机符号，视觉上高速转动。
- 逐列停止：第 1 列 400ms、第 2 列 800ms、第 3 列 1200ms 停稳。
- 转动时转盘内容加 `.spinning` 类 → `filter: blur(2.5px)` 产生残影效果。
- 全部停稳后调用 `judge()` 显示结果、更新计分。

### 计分存储

目前 `streak` / `best` 只存在内存里，刷新即清零。
> 以后想持久化：用 `localStorage` 保存 `best`（见下方"如何修改"第 4 条）。

---

## 四、如何修改 / 添加

### 1. 改/加转盘符号
改 `easter-egg-play.html` 里的 `POOL` 数组，每项：
```js
{ symbol: 'Fe', name: '铁', rarity: 'common' }
```
- `rarity` 三选一：`common` / `rare` / `legendary`
- 想让它更常出现 → 用 common（权重 4）；更少见 → rare（2）或 legendary（1）

### 2. 加/改判定组合
改 `judge(a, b, c)` 函数。三个参数是 {symbol, name, rarity}。
按需在合适的位置插入新的 `if` 分支（注意保持判定优先级）。

### 3. 调整转动节奏
- 换符号速度：`setInterval(..., 60)` 里的 60ms
- 停止时间：`startReel(0, 400)` / `startReel(1, 800)` / `startReel(2, 1200)` 的第二个参数

### 4. 让最高分跨会话保留
把 `best` 换成：
```js
let best = Number(localStorage.getItem('slotBest')) || 0;
bestEl.textContent = best;
// 更新时：
localStorage.setItem('slotBest', best);
```

### 5. 加一个新小游戏
1. 在 `.game-list` 加游戏卡片按钮
2. 复制一个 `<div class="game-screen" id="screenXxx">` 放游戏内容
3. 在 JS 里加：
```js
screens.xxx = document.getElementById('screenXxx');  // 在 screens 对象里
document.getElementById('gotoXxx').addEventListener('click', () => {
  screens.menu.classList.remove('active');
  screens.xxx.classList.add('active');
});
// 返回时参考 slotBack 的写法
```

---

## 五、文件清单

| 文件 | 作用 |
|------|------|
| `easter-egg-play.html` | 小游戏合集页（全部游戏逻辑 + 样式 + 数据） |
| `_layouts/default.html` | 触发词检测 + 跳转（`PLAY` 数组） |
| `_config.yml` | `exclude` 里加了本文件名，防止部署 |
| `PLAY_PAGE_DESIGN.md` | 本文档 |
