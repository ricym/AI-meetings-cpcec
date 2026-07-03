# meetings 技能更新记录

---

## v1.0 · 2026-04-28

**多会议类型配置套 + 多格式知识库**

核心改动：

- **配置套体系**：引入 profiles 目录结构，每种会议类型独立一套"模板+知识库+脚本"，存放在 `ai-meeting/profiles/<类型>/` 下
- **配置注册表**：新增 `registry.json`，注册所有配置套的元信息（id、关键词、文件路径等），支持 `default` 默认配置
- **多格式知识库**：知识库从单一 `knowledge_base.json` 拆分为多文件多格式并存：
  - JSON：6个分类文件（corrections/people/departments/places/systems/business_terms），按文件名映射用途
  - Markdown（.md）：补充背景知识（公司简介、制度说明等）
  - TXT：备注/纠错规则（`→` 格式行视为纠错规则）
  - DOCX：制度文件/组织架构文档（通过 read_docx.py 提取文本）
- **配置套匹配**：新增 `--type` 参数精确匹配 + 自然语言关键词模糊匹配 + 默认配置兜底
- **动态路径**：模板和生成脚本路径从 profile.json 动态读取，不再硬编码
- **补充知识参考**：新增 Step 2.6，处理非结构化知识（Markdown/TXT/DOCX）作为辅助参考
- **新增配置指南**：SKILL.md 末尾增加"新增会议类型配置套指南"，业务人员可自助创建新配置套
- **目录结构**：
  ```
  profiles/
  ├── registry.json
  └── 例会/
      ├── profile.json
      ├── template.docx
      ├── generate.py
      └── knowledge_base/
          ├── corrections.json
          ├── people.json
          ├── departments.json
          ├── places.json
          ├── systems.json
          ├── business_terms.json
          ├── (*.md / *.txt / *.docx 可选)
  ```

---

## v0.5 · 2026-04-15

**修复内容杜撰与责任人混入正文问题（首次实测反馈）**

- 新增"严禁杜撰"最高优先级规则：content 只能写录音中实际表达的内容，逻辑推断不得写入，每句话须能对应原文
- 新增"责任人只放末尾括号"规则：content 正文不得出现"XXX部牵头/负责"，责任人统一填入 responsible 字段并在末尾括号注明

---

## v0.4 · 2026-04-13

**扩展知识库综合运用（v2.0 KB）**

- `knowledge_base.json` 升级至 v2.0，新增5大分类：`people`（人员档案）、`departments`（部门职责）、`places`（地名）、`systems`（IT系统名）、`business_terms`（专业术语）
- Step 2 从单一"ASR纠错"扩展为5个子步骤：
  - 2.1 ASR纠错（原有）
  - 2.2 说话人识别：通过声音特征、业务领域、会议角色将 SPEAKER_XX 映射为真实人名
  - 2.3 责任归属核验：用 domains/responsibilities 字段交叉验证工作事项归属
  - 2.4 地名/系统名核验：别称→标准名替换（含版本号补全）
  - 2.5 专业术语规范：确保行业术语用法准确一致

---

## v0.3 · 2026-04-14

**书面化转写 + Word 读取**

- 新增口语→公文转写规则表（如"不能坏了规矩"→"严格遵守公司相关制度规定"）
- 新增自查原则：每条工作安排须达到红头文件标准
- 新增 `read_docx.py`：读取已填写的 Word 纪要，提取各字段供复用
- 支持 `--existing <docx路径>` 参数，已有信息与转录文本合并使用

---

## v0.2 · 2026-04-14

**修复技能结构**

- 问题：单文件 `meeting-minutes.md` 格式不被 Claude Code 识别（报 Unknown skill）
- 修复：改为文件夹结构 `meeting-minutes/SKILL.md`，技能正常加载

---

## v0.1 · 2026-04-13

**初始版本**

- 读取转录文本 + 企业知识库模糊纠错
- 信息提取（会议名称、时间、参会人、工作安排等）
- 生成结构化 JSON → 填入 Word 模板（黑体+仿宋_GB2312，16pt）
- 生成来源标注文件（每条工作安排标注时间戳、说话人、原文）
- 配套脚本：`generate_minutes.py`、`knowledge_base.json`
