# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 这个目录是什么

CANN「开发者效能全景概览」看板的**总览界面 v3.0 重构工作区**。目标：帮助产品/体验团队快速定位体验最差的角色/场景，基于 Agentic 评分、客户痛点、VOD 声量三类证据作改进决策。全部为单文件静态 HTML + 内联模拟数据，无构建系统、无后端、无依赖安装。

> Git 仓库：`git@github.com:SchihHsin/cann-dashboard.git`（main 分支）。与 `../AscendCANN/` 是两个独立仓库。

## 版本谱系（理解任务的关键）

| 文件 | 角色 |
|------|------|
| `cann-dashboard-homepage-before.md` | **v1.0 架构规格**（旧版总览：总览4卡 → TOP客户痛点 → 开箱即用 → 开发效率），与 `IMG_3633` 内容一一对应 |
| `IMG_3633.HEIC` | **视觉参考图**（= v1.0 的视觉实现）：浅色通透白卡、右上淡蓝网络装饰、横向渐变条、环形仪表、气泡图、分组柱、右下悬浮按钮 |
| `design-options-themed.html` | **v2.0 主题看板**（4398 行，含 总览/体验测试/用户旅程 三个 tab，完整 CSS token + ECharts）。v3 的视觉 token、组件、图表写法都从这里沿袭 |
| `总览界面改版新架构说明.md` | **v3.0 新架构规格**：五大楼层 |
| `overview-v3.html` | **v3.0 总览成品**（单页、仅总览、内联模拟数据） |
| `journey-v3.html` | **v3.0 用户旅程成品**（沿用 overview-v3 皮肤重绘 v2.0 用户旅程 tab，三大楼层、内联模拟数据） |

v3.0 把 v1.0 的内容**重新分组**为五楼层，并采用 `IMG_3633` 的视觉皮肤。`.md` 规格对部分楼层描述较简略，缺口用模拟数据 + 复用 v2.0/v1.0 结构补全。

## v3.0 五大楼层（`overview-v3.html`）

锚点 id 依次为 `#f-top / #f-vod / #f-eco / #f-dev / #f-core`：

1. **总览**（4 张汇总卡，每卡"查看更多"锚点跳到对应详情楼层）：VOD声音(NSS环形仪表+正负向+闭环率) / 生态增益(API支持度·开箱达标/通过率表) / 开发界面(产品易用性横向评分条) / 技术内核(专项维度 vs CUDA)
2. **VOD 声音**：痛点闭环率横条、痛点分布气泡图、正向案例分组柱、高频共性声音 Top5 横条 **↔ 右侧 Top原声列表/详情双态联动**（点左柱或列表项进入详情，"返回列表"按钮回到列表）。v1.0 的"TOP客户痛点"折叠进此层
3. **生态增益**：PyTorch API(Core test KPI + 增强能力表)、模型开箱成功率/性能、高影响力模型 0day 发布（v1.0 的"开箱即用"层）
4. **开发界面**：**体验质量五级金字塔**（L5 好用 8~10 → L1 不可用 0~2，自顶向下评分由好到差，SVG polygon 拼接 + 当前水平定位标记）、Sample覆盖度、Agentic KPI、接口能力满足度
5. **技术内核**：能力对标 vs CUDA 雷达、维度明细、自动化覆盖说明

## v3.0 用户旅程三大楼层（`journey-v3.html`）

锚点 id 依次为 `#f-top / #f-matrix / #f-scene`，沿用 overview-v3 的 topbar / filter-bar / 网络装饰 / token / 组件。**两页 topbar 互通**：journey 的「总览」tab 跳 `overview-v3.html`，overview 的「用户旅程」tab 跳 `journey-v3.html`（均为 `location.href` 跳转，「体验测试」仍是 alert 占位）。filter-bar 此页用 **角色**（算子开发者 / AI 框架开发者 / 应用开发者）= CANN 终端开发者角色，**不是**总览页的「视角」内部使用者 chips，两套别混（见设计约定）。


**配色 / 样式约定**：场景评分相关数字、徽章、标签按 grade 走语义色（红 bad / 金 mid / 绿 ok，问题优先）。场景卡 `.sc` 左上有同 grade 色的扩散光晕（`.sc::before` 径向渐变，颜色由 `.sc.bad/.mid/.ok` 的 `--sc-glow` RGB 变量驱动；卡片内容靠 `.sc>*{z-index:1}` 浮在光晕之上）。**不要用左描边 / 上描边**（左侧/顶部彩色 accent bar）的卡片样式，统一用 1px 整框 + 柔阴影。**楼层标题不带彩色 icon 徽章**（`.floor-badge` 已从各楼层移除——用户嫌颜色多），floor-hd 直接以标题起头。**总览楼层（Floor1）4 张 KPI 卡用毛玻璃效果**：`.card.jkpi`（半透明白 + `backdrop-filter:blur(16px) saturate` + 斜向高光 ::before），直接铺在页面浅色底上（用户不要彩色渐变背板）。

1. **总览**：4 张 hero KPI 卡（`.jkpi`）——整体健康评分 / 严重场景 / 活跃痛点 / 本期优先处理（chip 点击锚跳并 `selScene` 展开对应场景）
2. **触点体验矩阵**（样式/交互照搬 `matrix.html` 重绘）：CSS grid（`#tpHeatGrid`，4 场景 × 文档/API/工具/环境 + 综合列），每格是独立**圆角小卡**（48px，淡底 5% + 同色 10% 描边 + 表情脸 `faceIcon` + 数字 + 趋势箭头），数字用 JetBrains Mono。色阶 `heatColor/borderColor/textColor` + hover 版（`heatColorHover/borderColorHover`，经 `--bg-hover/--bd-hover` CSS 变量）为 **5 段：低→红/橙/绿/青/蓝←高**（matrix.html 方案，非"低分=红"语义；情绪靠表情脸传达）。hover 描边变 2px+实色，选中 `.hm-sel` 紫环+`scale(1.06)`。点格 `selectTpCell()` → **网格下方** `#tpDetail` 内联详情（分数大色块 + 场景·维度名 + **近 6 期迷你折线** `miniSparkSvg` + 五维分值条 + Agent 观测），非右侧栏。综合列不可点。折线无真实历史，由 `tpHist(score,trend)` 按当前分+趋势('u'/'d'/'f')合成 6 点。
3. **场景深潜**：场景列表（`.sc` 卡，`renderList()`，问题优先/目录顺序 `setSort()` 切换，`catalogOrder=[0,2,1,3]`）+ 场景详情 **主从联动**（`selScene(id)`，初始 `selScene(0)` 默认展开最差场景）。详情含：
   - **Agentic 指标卡**（`#detKpis.mk-grid`，4 张 `.mk-card`，样式参考 `reference/阶段的指标卡片.jpg`）：开发成功率 / 平均复杂度（tokens消耗量）/ 平均任务执行时长 / 用例执行通过率 ——**替代了 v2.0 的「5 维诊断雷达」**。每卡：左上图标(`KPI_META[i].ic`/accent/soft) + 右上「近7次数据」 / 指标名 / 大数值+单位+占比(`kpiFrac`，成功率 /47、通过率 /300，由百分比合成)+趋势 / 右侧迷你图（`KPI_META[i].chart`：成功率·通过率=`kpiBars` 末柱高亮，复杂度·时长=`kpiLineChart`；7 点序列 `kpiSeries(base,trendCls)` 合成，无真实历史）。数据仍在 `scenarios[].kpis`，元信息按 index 取 `KPI_META`。
   - **Agent 逐步评分**（旅程环节时间线 `.step-tl` + 指标网格 + 工具调用链）直接平铺展示。原 v2.0 的「三类证据 Tab（Agent/痛点/VOD）」已**去掉 tab 切换**——痛点 / VOD 改由证据速览 popover 查看（见下），详情区只剩 Agent 逐步评分。步骤评分用 `.step-tl-dot` 内的**环形进度图**呈现（`stepRing(score,cls)` 内联 SVG，`pathLength=100` + `stroke-dasharray` 映射百分比，中心 `.sd-num` 叠数字），原来的横向 bar 和单独 `as-score` 文字已移除；步骤头只剩 名称 + 右侧「痛点·VOD ›」入口。
   - 注：v2.0 的「排名对照 mini bars」和「近 7 次评测趋势（ECharts line）」已按用户要求**移除**；`scenarios[].runs` 仅余场景卡 sparkline 使用。
   - **证据速览 popover**（`#evPop`，JS 动态建一个 `.evpop`）：场景卡和 Agent 旅程环节步骤卡都在**名称右侧**各放两个独立入口（痛点 / VOD），由 `evEntry(id,kind,label,count)` 统一生成（`kind='pain'|'vod'`，带 `EV_ICON` 图标）。入口样式 `.ev-entry` 用**虚线描边 + 图标 + 浅灰底**，刻意区别于 grade-tag（实心圆角药丸）；hover 时按 pain=珊瑚 / vod=青 变实心。**悬浮预览 / 点击锁定**（`showEvPop(id,anchor,focus,pin)`），弹出所属场景的痛点 + VOD 详情（痛点/VOD 是场景级数据，步骤卡弹的是其所属场景）。锁定态点空白/滚动关闭；entry 的 onclick 需 `stopPropagation` 以免触发选场景。
   - 数据模型 `scenarios[]`（id 0-3）逐字沿用 v2.0 + 新增 `kpis` 字段；**已去掉 v2.0 的「设计点」侧栏**（①–⑥ 设计理由讲解），与 overview-v3 一致保持干净成品。

## 代码结构（单文件约定）

`overview-v3.html` 内部分三段：`<style>`（:root token + 组件类）→ `<body>`（topbar / filter-bar / `.page` 内五个 `<section class="floor">` / FAB）→ `<script>`。

- **图表统一走 `makeChart(id, opt, renderer)`**（默认 svg 渲染，自带 resize 监听）。当前实例：`ringNSS`(gauge) / `bubblePain`(scatter) / `posCase`(grouped bar) / `vodVol`(横向 bar，实例存 `_vodChart`) / `sampleCov`(bar) / `coreRadar`(radar)。`AX` 常量统一坐标轴样式。
- ECharts 由 CDN 引入（`cdn.jsdelivr.net/npm/echarts@5`）——**离线/无网环境图表不渲染**，截图验证时需放网。
- 右上**网络装饰图**由脚本随机生成节点+连线（纯 SVG），非位图。
- 横向评分条为纯 CSS；**体验金字塔为内联 SVG polygon**；仪表/气泡/柱/雷达用 ECharts。
- "查看更多 / a[href^='#']" 通过末尾脚本做平滑锚点滚动。
- 所有数据为内联模拟值，无外部数据源。

### 双态卡片联动（如 Floor 2 的 VOD）

`VOD_TOP` 数组驱动 list/detail 两套渲染。需要捕获 ECharts 实例（`const _vodChart=makeChart(...)`）后挂 `_vodChart.on('click',p=>selectVod(p.dataIndex))`。柱图数据顺序与 `VOD_TOP[i]` 必须**索引对齐**（按 vol 降序，rank1 置顶，配合 `inverse:true`）。`renderVodList()` / `selectVod(i)` 末尾各调一次 `_vpResize()`，让动态高度变化后图表 reflow。

### 卡内图表自适应卡高的标准做法

CSS grid 父容器 `align-items:stretch` 让同行两卡等高，左卡用 `display:flex;flex-direction:column`，图表元素用 flex 数字 spacer 占位实现上/下留白：

```html
<div class="card" style="display:flex;flex-direction:column">
  <div class="card-hd">...</div>
  <div style="flex:0.05"></div>          <!-- 上留白 -->
  <div id="vodVol" style="flex:0.85;min-height:0"></div>
  <div style="flex:0.1"></div>           <!-- 下留白 -->
</div>
```

> `flex:<数字>` 是 grow 比例，不是百分比；多个 grow 的兄弟之间才会按比例分。要让 ECharts 自适应高度变化，记得在改容器内容后 `chart.resize()`。

## 查看与验证

```bash
# 在浏览器打开
open overview-v3.html        # 或 design-options-themed.html

# 无头截图验证（ECharts 走 CDN，必须联网；--virtual-time-budget 等待图表渲染）
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless --disable-gpu --hide-scrollbars --force-device-scale-factor=2 \
  --window-size=1680,3400 --virtual-time-budget=6000 \
  --screenshot=/tmp/v3.png "file://$(pwd)/overview-v3.html"

# HEIC 参考图转 PNG 后再读
sips -s format png IMG_3633.HEIC --out /tmp/ref.png
# 分块放大读细节：sips -c <高> <宽> --cropOffset <Y> <X> /tmp/ref.png --out tile.png
```

## 设计约定（沿袭 v2.0）

- 配色 token 在 `:root`：蓝(主色)/紫/青/绿/金/珊瑚 各 5 档 + 灰阶 `--g1..g10`，语义别名 `--good/warn/bad` 等。新卡片优先复用既有 token 与组件类，不要硬编码色值。
- 视觉基调：浅色通透、白卡 + 柔阴影 + 圆角，高度对齐 `IMG_3633`。
- 问题优先：排序/颜色/强调以"哪里最差"为导向；三类证据(Agent评分/痛点/VOD)并列互证。
- **filter-bar 视角 chips = 看板内部使用者**（资料 / 体验设计 / 算子编程 / 推理框架 / 产品负责人 / 社区运营），沿用 v2.0 定义；**不是** CANN 的终端开发者角色（算子开发者/框架开发者/应用开发者/入门开发者）。这两套角色容易混淆，新增筛选时务必区分。

## 协作规则

- **用中文**交流。
- **视觉/设计类改动先提 2–4 个选项**让用户选（可用 AskUserQuestion），不要直接实现。
- **模拟数据先行**：缺真实结构时用模拟数据做成完整楼层，后续替换。
- **改完自动同步 CLAUDE.md + commit + push**：每次完成一组改动后，先把引入的新架构/组件/约定/坑同步到本文件（纯文案/数值微调不必更新），再按现有 commit 风格（中文 prefix `style:` / `feat:` / `fix:` + 简要描述）一次性提交并推 `origin/main`。CLAUDE.md 改动与功能改动合并到同一 commit，不要堆积。
