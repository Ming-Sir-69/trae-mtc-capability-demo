## 证据来源
本文件内容来自本次对话中已展示的系统提示词（System/Developer Messages），用于“可核验地”说明：我在当前 Trae SOLO 环境内可调用的能力、工具与限制。

## 关键能力（摘录与归纳）
### 1) 可用工具（开发工具）
- 文件类：Read / apply_patch / DeleteFile / LS / Glob / Grep
- 运行命令：RunCommand（不用于写文件；写文件必须用 apply_patch）
- Web：WebSearch / WebFetch（受合规限制，遇到限制不得绕过）
- 子代理：Task（Explore/Plan/general_purpose_task）
- 交互确认：AskUserQuestion
- 任务跟踪：TodoWrite

### 2) 可用“技能（Skills）”
文档/办公：
- `pptx`：生成/编辑 PPT（强调：从零创建建议用 PptxGenJS）
- `xlsx`：创建/编辑复杂 Excel（openpyxl/pandas + LibreOffice 公式回算）
- `docx` / `pdf`：Word/PDF 处理
可视化：
- `chart-visualization`：生成图表图片
浏览器自动化：
- `agent-browser` / `webapp-testing`：网页交互与测试（受当前任务实际需要与合规限制约束）

### 3) 关键限制（必须遵守）
- 不能用 RunCommand 的 echo/cat/heredoc 写文件；必须用 apply_patch
- WebFetch/WebSearch 若提示域名受限，不得用其它方式绕过
- 输出文件必须保存到用户可见的工作区（本次为 `MTC/`）

