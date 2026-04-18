## 本轮升级目标
把 `MTC_能力演示.pptx` 从“内容陈列”升级为“**PPT 工具能力武器库化模板**”：让审阅者明确知道我能在 PPT 里做到哪些工具级能力，并且每个能力点都能被复现验证。

---

## 本轮新增/强化的 PPT 工具能力点（可核验）
1) **视频嵌入（可播放）**
   - 证据：PPT 内嵌 `assets/3D_图表旋转演示.mp4`
   - 验证：放映模式点击播放

2) **音频嵌入（可播放）**
   - 证据：新增 `assets/mtc_beep_1s.wav` 并嵌入 PPT
   - 验证：
     - 点击“播放提示音”文字按钮区域（音频对象透明覆盖在文字按钮上，实现点击文字播放）
     - 同时：部分页面切换时会自动播放“转场音效”（见第 4 点）

3) **GIF 动图生成 + 插入**
   - 证据：
     - `assets/3D_图表旋转演示.gif`（由 `assets/3D_图表旋转演示.mp4` 转换得到）
     -（保留）`assets/excel_preview_anim.gif`（由 `assets/excel_preview-*.jpg` 合成）
   - 验证：放映模式观察动效（不同 PowerPoint 版本对动图支持可能有差异）

4) **页面切换 Transition**
   - 证据：对部分页面加入 OOXML `<p:transition>`（slide3~slide8）
   - 验证：放映模式可观察到页面切换效果
   - 补充（本轮增强）：
     - 每页不同转场：blinds / wipe / push / split / randomBar / zoom
     - 每页不同转场音效：通过 `<p:sndAc>` 绑定不同 wav（slide3~8 各不相同）

5) **Excel 表格数据 → PPT 可编辑 Table（非截图）**
   - 证据：从 `MTC_复杂Excel模型.xlsx` 的 `QualityGate` 抽取表格数据，生成 PPT 内原生表格对象
   - 验证：在 PPT 内直接编辑表格单元格；并可回源打开 Excel

6) **Speaker Notes（演讲者备注）**
   - 证据：每页写入 Notes：讲解脚本 + 验证口径
   - 验证：PowerPoint 打开“备注窗格”查看

7) **可复用组件与版式一致性**
   - 组件：标题条、卡片容器、CTA 按钮、证据矩阵页
   - 目标：让后续新增能力点时，只需“复制版式 + 填充证据”，持续扩展为武器库

---

## 新增 supporting assets（可复用资源）
- `assets/transparent_1px.png`：1x1 透明 PNG（用于把音频“点击区域”做成文字按钮点击）
- `assets/sfx_click.wav`：点击文字播放音效（更自然）
- `assets/sfx_tr_01.wav` ~ `assets/sfx_tr_06.wav`：转场音效库（每页不同）
- `assets/excel_preview_anim.gif`：Excel 预览图合成动图（用于 GIF Demo）
- `assets/3D_图表旋转演示.gif`：3D 演示视频转 GIF（用于 GIF Demo）

---

## 兼容性备注（与 00_error/00_README 对齐）
- Excel for Mac 打开 PPT：优先使用 Office URI Scheme（避免 file URI 公式不稳定）；详见 `item_fille/00_error/trae mtc能力梳理及演示/...md` 与 `00_README.md`
- GIF 动图：不同 PowerPoint 版本对动图支持差异较大，建议以“放映模式”效果为准
- 元素级动画（旋转、飞入等）：需要更复杂的 OOXML timing/时间线节点；本版不做稳定性承诺，已在 PPT 的“边界说明”页标注，并用 transition + GIF/视频作为替代

---

## 经验沉淀（可复用坑型 + 最佳实践）
### 1) “PowerPoint 打开提示需要修复”如何定位与修复
- 典型根因：媒体封面图（尤其是音频的 cover PNG）损坏或 OOXML 关系不一致。
- 快速定位方法：解包 pptx（zip）→ 检查 `ppt/media/*` 能否被图片库正常校验（PIL verify），并检查 `ppt/slides/_rels/*.rels` 是否存在指向缺失文件的关系。
- 本项目的具体坑型与解法已固化在：
  - `item_fille/00_error/trae mtc能力梳理及演示/trae mtc能力梳理及演示20260415-20260415.md`

### 2) PptxGenJS 音频“点击文字播放”的实现模式
- 目标：用户点击“文字按钮区域”播放，而不是点一个小喇叭图标。
- 稳定实现：在文字按钮区域放置音频对象，并用透明 cover 覆盖点击区域（cover 使用 1x1 透明 PNG）。
- 关键注意：PptxGenJS 的 `cover` 需要 **data URI**（`data:image/png;base64,...`），否则可能生成损坏 PNG 并触发 PowerPoint 修复。

### 3) “每页不同转场动画 + 每页不同转场音效”的 OOXML 实现模式
- 动画：通过 `ppt/slides/slideN.xml` 的 `<p:transition>` 子节点实现（blinds/wipe/push/split/randomBar/zoom 等）。
- 音效：通过 `<p:transition><p:sndAc>...<p:snd r:embed="rIdX"/>` 绑定音频，并在对应的 `slideN.xml.rels` 增加 `relationships/audio` 指向 `ppt/media/*.wav`。
- 经验结论：相比“元素级动画时间线”，转场（transition）与转场音效（sndAc）更容易稳定落盘、也更容易跨平台核验。

### 4) “视频转 GIF”最佳实践（避免锯齿/颜色劣化）
- 推荐 pipeline：`ffmpeg` 抽帧 + palettegen/paletteuse（可显著改善 GIF 颜色与抖动）。
- 产物应作为 supporting asset 放在 `assets/`，便于后续复用/替换。
