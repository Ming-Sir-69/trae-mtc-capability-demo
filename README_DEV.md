# Trae MTC 能力梳理及演示 · 开发文档

> 详细使用说明请参阅 [00_README.md](00_README.md)。

## 项目背景

梳理 Trae SOLO 在 MTC（More Than Code）模式下的文档工程能力，通过构建实际项目产物来验证 Excel、PPT、HTML 等多格式文档的生成能力。

## Excel 武器库能力点

- **控制面板（数据验证）**：区域/情景/产品三类下拉控件驱动联动
- **跨表公式链路**：Assumptions → Data → Analysis
- **表格对象（Excel Table）**：Data 区域封装为 `tblData`
- **条件格式**：收入列/KPI/门禁结果的色阶与提示
- **图表对象**：Analysis 里 3 张图（区域、区域+产品、产品累计）
- **质量门禁**：QualityGate Sheet 输出 PASS/FAIL（Poka-Yoke）
- **说明页**：Explain Sheet 解释模型结构/关键公式

## Excel 使用方式

打开 `MTC_复杂Excel模型.xlsx`，进入 `Analysis` Sheet：
- 下拉选择 **区域**（B3）、**情景**（B4）、**产品**（D3）
- 点击顶部"快速入口"超链接跳转到 3D仪表盘 / PDF / PPT

### 兼容性提示

- HTML/PDF：使用 Excel 原生超链接（相对路径）
- PPT：Mac 环境下用 `ms-powerpoint:ofe|u|` 协议强制拉起
- 若失败，排查 `~$MTC_能力演示.pptx`（Office 锁文件），删除后重试

## 项目结构

```
├── MTC_能力演示.pptx           # PPT演示稿
├── MTC_复杂Excel模型.xlsx       # Excel武器库
├── interactive/                 # 可交互HTML仪表盘
├── assets/                      # 素材（图片/视频/音效）
├── project_context/             # 需求与能力边界
├── 作品封面图.png
└── 作品渲染图/
```

## 后续开发方向

- [ ] 增加更多Excel武器库模板（财务模型、项目排期）
- [ ] PPT自动排版能力增强
- [ ] 多项目联动演示
