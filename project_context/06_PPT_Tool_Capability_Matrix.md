## 目的
把 `MTC_能力演示.pptx` 做“工具能力武器库化”前，先用 **OOXML 证据**盘点：当前 PPT 已展示哪些 PowerPoint 工具能力、哪些还没展示（或展示不充分），以便后续增量补齐。

> 证据来源：对 `MTC_能力演示.pptx` 进行 OpenXML（zip）解包，检查 `ppt/` 目录结构、`ppt/media/`、`ppt/slides/*.xml`、`ppt/slides/_rels/*.rels`、`ppt/notesSlides/` 等。

---

## 基线信息（已核验）
- 幻灯片数量：10（`ppt/slides/slide1.xml` ~ `slide10.xml`）
- 主题/母版：
  - `ppt/slideMasters/slideMaster1.xml`（1 个）
  - `ppt/slideLayouts/slideLayout1.xml`（1 个）
- 讲者备注（Notes）：存在，但内容基本为空
  - `ppt/notesSlides/notesSlide1.xml` ~ `notesSlide10.xml`
  - 备注文本占位符存在，但 `<a:t>` 为空（示例：`notesSlide1.xml`）

---

## 能力矩阵：已展示 vs 待补齐

| 能力大类 | 当前是否已展示 | OOXML 证据（示例） | 备注 / 待补齐动作 |
|---|---:|---|---|
| 本地超链接（文件/HTML） | ✅ | `ppt/slides/_rels/slide1.xml.rels`：Target=`MTC_复杂Excel模型.xlsx`、`interactive/3D_可交互销售仪表盘.html`（TargetMode=External） | 已有；需继续保持“相对路径 + Mac 兼容策略”一致性 |
| 视频嵌入（可播放） | ✅ | `ppt/media/media-7-2.mp4`；`ppt/slides/_rels/slide7.xml.rels`：Type=`.../video` + `.../media` 指向 mp4 | 已有；后续可把“如何验证播放”写进 Notes/验证页 |
| 图片（png/jpg） | ✅ | `ppt/media/image-5-1.jpg`、`image-6-1.jpg`、`image-7-1.png`、`image-7-4.png` | 已有；可扩展 GIF 动图 |
| 音频嵌入（可播放） | ❌ | `ppt/media/` 未见 `mp3/wav`；`slideX.xml.rels` 未见 Type=`.../audio` | 本轮将新增 1 个“提示音”作为可核验音频对象 |
| GIF 动图插入 | ❌（未发现） | `ppt/media/` 未见 `.gif` | 本轮将“生成 GIF + 插入 PPT”作为 Demo 点，并在 Notes 标注版本兼容性 |
| 页面切换效果（Transition） | ✅ | `ppt/slides/slide3.xml`~`slide8.xml` 包含 `<p:transition>`（blinds/wipe/push/split/randomBar/zoom），且含 `<p:sndAc>` | 已实现“每页不同转场 + 每页不同转场音效”，放映时可直接核验 |
| 元素级动画（Timing/Timeline） | ❌ | `ppt/slides/*.xml` 未发现 `<p:timing>` / `<p:seq>` / `<p:par>` 等 | 本轮默认不承诺复杂元素动画；如需“动起来”，优先 GIF 或多页分步 |
| 表格对象（PPT 原生可编辑 Table） | 未核验/倾向 ❌ | 需在执行期通过 `markitdown` + slide xml 进一步确认（当前未做逐页对象语义解析） | 本轮将从 Excel 读取数据，生成至少 1 个 PPT 可编辑表格（并给回源 Excel 链接） |
| 图表对象（PPT 原生 Chart） | 未核验 | 同上 | 若当前未展示，将补 1 页“图表+KPI 卡片”版式 |
| Speaker Notes（演讲者备注） | ⚠️ 有容器、无内容 | `ppt/notesSlides/notesSlide*.xml` 存在但 `<a:t>` 为空 | 本轮将为关键页补 Notes：讲解脚本 + 验证口径 |
| 版式库/模板化复用（多 Layout） | ❌（不足） | 仅 1 个 slideLayout | 本轮会增强“封面/分节/双栏卡片/证据矩阵/媒体页”等版式一致性（必要时新增 layout 或用组件化实现） |

---

## 结论（用于驱动后续改造）
1) 当前 PPT 已经具备：**超链接联动、视频嵌入、图片展示** 等“可核验”能力点。  
2) 当前 PPT 缺失或不充分：**音频、GIF 动图、页面切换 transition、讲者备注、模板化多版式、（可能）可编辑表格/图表对象**。  
3) 后续改造优先级：先补齐“你关心的工具能力”（音频/GIF/transition/表格/notes），再做版式系统化与 QA 自证页。
