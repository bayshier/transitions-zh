<div align="center">

# transitions.dev 中文指南 & 接入示范

**线框 → 轮廓 → 内容长成——"转圈圈 loading 可以退休了"。**

[![Based on](https://img.shields.io/badge/原项目-transitions.dev-3DDC84?style=flat-square)](https://github.com/Jakubantalik/transitions.dev)
[![Live demo](https://img.shields.io/badge/接入示范-我的个人主页-blue?style=flat-square)](https://bayshier.github.io/GYZApp/easin.html)
[![License](https://img.shields.io/badge/原库许可-Free%20to%20use-green?style=flat-square)](https://github.com/Jakubantalik/transitions.dev/blob/main/LICENSE)

</div>

---

> 📌 **这是什么**：[Jakub Antalík 的 transitions.dev](https://github.com/Jakubantalik/transitions.dev) 的**中文使用指南 + 真实站点接入示范**。配方与运行时版权归原项目所有（允许无限商用/修改/集成到产品，禁止重新分发为竞品过渡库——所以本仓库只放指南与接入代码，不搬运配方本体）。

## 它解决什么问题

传统 loading 是"内容没好，转个圈敷衍"。transitions.dev 的 ui-generation 反过来：

1. **点阵场**从中心涟漪展开，配 matrix 雨与探测线
2. 从元素**自身结构**测量，画出虚线线框（外框、文字行、图标路径），带辅助线、角部手柄和尺寸标签
3. 扫描线扫过，真实内容在线后淡入长成

**没有任何造假**：线框是量出来的，所以虚线落点与真实内容分毫不差。零依赖运行时，vanilla/React/Vue/Svelte 通吃，`prefers-reduced-motion` 自动降级。

## 我的接入实录（个人主页）

三步，全部改动见 [GYZApp/easin.html](https://github.com/bayshier/GYZApp/blob/main/easin.html)：

**① 运行时进项目**（从原仓库 `skills/transitions-ui-generation/` 取 `ui-generation.css/js`，附 LICENSE）：

```
assets/transitions/
├── ui-generation.css
├── ui-generation.js
└── ui-generation-LICENSE.txt   # 原库许可，随身携带
```

**② head 引样式：**

```html
<link rel="stylesheet" href="assets/transitions/ui-generation.css">
```

**③ 页尾播放**——对主区块逐个生成，`await .finished` 串行衔接：

```html
<script type="module">
const play = async () => {
    const { generate } = await import('./assets/transitions/ui-generation.js');
    await document.fonts.ready;                     // 字体就绪再测量，线框才准
    for (const [sel, done] of [['#resume', 'Résumé ready'], ['#works', 'Works ready']]) {
        const el = document.querySelector(sel);
        if (el) await generate(el, { mode: 'skeleton', done }).finished;
    }
};
document.readyState === 'complete' ? play() : addEventListener('load', play);
</script>
```

三个实战要点：
- **等 `document.fonts.ready`**——字体晚到会改布局，线框就量歪了
- **`await generate(...).finished`** 串行播放比并发更从容
- **try/catch 静默回退**——过渡是锦上添花，任何失败都不该拦住内容

效果预览（真站，打开即播）：**[bayshier.github.io/GYZApp/easin.html](https://bayshier.github.io/GYZApp/easin.html)** 🎬

> 建议直接打开线上站看——动效是"页面加载"的一部分，任何静态截图都只是它的遗照。
> 想重播：刷新页面即可；想对照原理，读下面三步实录。

原项目的官方演示：[transitions.dev](https://transitions.dev/)（18 个可交互卡片）与
[skills/transitions-ui-generation/demo.html](https://github.com/Jakubantalik/transitions.dev/blob/main/skills/transitions-ui-generation/demo.html)（可重播、双模式切换）。

## 35 个过渡全览（速查表）

原库共 35 个配方，除 skeleton 外这些也很适合替代 loading：

| 配方 | 适用场景 |
|---|---|
| `skeleton` / `skeleton-texts` | 页面/组件首次登场（本仓库示范） |
| `matrix-loader` | AI 产品生成等待 |
| `thinking-states` / `reasoning-stream` / `streaming-text` | LLM 流式输出场景 |
| `number-pop-in` / `spinning-counter` | 数字滚动（行情、统计） |
| `success-check` / `error-state-shake` | 表单反馈 |
| `text-morph` / `texts-reveal` | 标题登场 |

安装方式（二选一）：

```bash
npx skills add Jakubantalik/transitions.dev   # 作为 agent skill 装进 Cursor/Claude Code
# 或手动取 skills/transitions-ui-generation/ 下的运行时
```

## 许可证（务必读完再用）

原库为定制许可：**允许**无限个人与商用、修改、集成进你的产品上线；**唯一禁止**——把过渡库本身重新分发为竞品库/工具包。CLI 与工具链部分为 MIT。详见[原 LICENSE](https://github.com/Jakubantalik/transitions.dev/blob/main/LICENSE)。

本仓库因此**只包含中文指南与接入示范代码**，不含配方搬运。

## 致谢

- [Jakub Antalík](https://github.com/Jakubantalik) — transitions.dev 作者，效果的真正缔造者
- [Moeru AI](https://github.com/moeru-ai) — 资助了原项目的签名公证（Mac-Duo 生态）

## License

指南与接入示范代码：[MIT](LICENSE) · 原过渡库版权归 Jakub Antalík，按其定制许可使用
