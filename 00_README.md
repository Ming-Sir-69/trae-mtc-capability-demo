本目录为「MTC：SOLO 能力证明与演示」产物输出区。

主要文件：
- `MTC_能力演示.pptx`：PPT 演示稿（含图片/视频/超链接/Excel 附件入口等）
- `MTC_复杂Excel模型.xlsx`：Excel MTC（More Than Code）武器库示例（多表、复杂函数、图表、条件格式、数据验证、外部联动、质量门禁、说明页等）
- `interactive/`：可交互图表（HTML，本地打开）
- `assets/`：PPT/Excel 使用到的图片与视频素材
- `project_context/`：需求、证据来源、能力边界说明（外置上下文）

---

## Excel（MTC_复杂Excel模型.xlsx）怎么用

### 入口
- 打开后进入 `Analysis`（仪表盘）Sheet：
  - 下拉选择 **区域**（B3）、**情景**（B4）、**产品**（D3）
  - 点击顶部“快速入口”超链接跳转到：
    - `interactive/3D_可交互销售仪表盘.html`
    - `assets/MTC_复杂Excel模型.pdf`
    - `MTC_能力演示.pptx`

### 武器库能力点（Excel 层面）
- **控制面板（数据验证）**：区域/情景/产品三类下拉控件驱动联动
- **跨表公式链路**：Assumptions → Data（单价/收入）→ Analysis（聚合与指标）
- **表格对象（Excel Table）**：Data 区域封装为 `tblData`（可筛选/排序/条纹样式）
- **条件格式**：收入列/KPI/门禁结果的色阶与提示
- **图表对象**：Analysis 里 3 张图（区域、区域+产品、产品累计）
- **质量门禁**：`QualityGate` Sheet 输出 PASS/FAIL 与总状态（Poka‑Yoke）
- **说明页**：`Explain` Sheet 解释模型结构/关键公式，并嵌入图片素材

### 兼容性提示
- “快速入口”在 Excel for Mac 下存在差异：
  - HTML/PDF：使用 Excel 原生超链接关系（相对路径）更稳定；
  - PPT：部分 Mac 环境下直接文件超链接可能报“发生了意外错误”，因此当前用 `ms-powerpoint:ofe|u|` 协议强制拉起 PowerPoint 打开。
  - 若仍失败，优先排查同目录是否残留 `~$MTC_能力演示.pptx`（Office 锁文件），删除后重试。
