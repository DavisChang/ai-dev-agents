# 貢獻指南 / Contributing Guide

感謝你有意願貢獻 ai-dev-agents！此專案旨在打造一個真正理解專案上下文的 AI 開發輔助系統。我們非常歡迎任何形式的貢獻，無論是回報 Bug、改善文件、新增 Agent 功能或優化偵測邏輯。

Thank you for your interest in contributing to ai-dev-agents! This project aims to build an AI development assistant system that truly understands project context. We welcome contributions of any kind, whether it's reporting bugs, improving documentation, adding Agent features, or optimizing detection logic.

## 🤝 行為準則 / Code of Conduct

本專案致力於提供一個友善、包容的環境。請在參與討論與貢獻時保持尊重。

This project is committed to providing a friendly and inclusive environment. Please remain respectful when participating in discussions and contributions.

## 🚀 貢獻方向 / What to Contribute

你可以從以下幾個方向著手：
You can start from the following directions:

### 1. 改善 Agent 規則 / Improve Agent Rules
- **修正 Prompt**：調整 `.cursor/rules/*.mdc` 中的指示，讓 Agent 輸出更精確、更符合預期。
  - **Refine Prompts**: Adjust instructions in `.cursor/rules/*.mdc` to make Agent outputs more precise and aligned with expectations.
- **特定技術棧支援**：針對特定的框架（如 Next.js, Django, Go, Spring Boot）增加更好的開發指引。
  - **Specific Stack Support**: Add better development guidelines for specific frameworks (e.g., Next.js, Django, Go, Spring Boot).
- **優化上下文讀取**：改進 `00-project-context.mdc` 的策略，讓 Agent 能更聰明地載入相關文件。
  - **Optimize Context Loading**: Improve the strategy in `00-project-context.mdc` to allow the Agent to smartly load relevant documents.

### 2. 擴展偵測能力 / Extend Detection
- **支援新技術棧**：在 Bootstrap Agent (`01-bootstrap-context.mdc`) 中增加對新語言或框架的偵測邏輯。
  - **Support New Stacks**: Add detection logic for new languages or frameworks in the Bootstrap Agent (`01-bootstrap-context.mdc`).
- **提高準確度**：改善現有的專案結構辨識邏輯（例如更準確地識別 Monorepo 或微服務架構）。
  - **Improve Accuracy**: Enhance existing project structure identification logic (e.g., accurately identifying Monorepo or microservices architectures).
- **豐富知識庫**：在 `.ai-context/` 中增加新的欄位或檔案類型，讓 AI 掌握更多資訊。
  - **Enrich Knowledge Base**: Add new fields or file types in `.ai-context/` to give AI more information.

### 3. 文件與範例 / Documentation & Examples
- **工作流範例**：分享你使用此系統完成開發任務的實際案例。
  - **Workflow Examples**: Share actual cases of using this system to complete development tasks.
- **使用指南**：撰寫針對特定技術棧的整合教學。
  - **Usage Guides**: Write integration tutorials for specific technology stacks.
- **多語言翻譯**：協助將文件翻譯為其他語言。
  - **Translation**: Assist in translating documentation into other languages.

## 🛠️ 開發流程 / Development Process

### 步驟 1：Fork & Clone / Step 1: Fork & Clone
1. Fork 本倉庫到你的 GitHub 帳號。
   - Fork this repository to your GitHub account.
2. Clone 你的 Fork 到本地：
   - Clone your Fork locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/ai-dev-agents.git
   cd ai-dev-agents
   ```

### 步驟 2：建立分支 / Step 2: Create Branch
請為每個功能或修復建立獨立的分支：
Please create a separate branch for each feature or fix:
```bash
git checkout -b feature/improve-bootstrap-logic
# 或 / or
git checkout -b fix/typo-in-readme
```

### 步驟 3：進行修改 / Step 3: Make Changes
- **Agent 規則**：修改 `.cursor/rules/` 下的 `.mdc` 檔案。
  - **Agent Rules**: Modify `.mdc` files under `.cursor/rules/`.
- **知識庫模板**：修改 `.ai-context/` 下的 YAML 或 Markdown 檔案。
  - **KB Templates**: Modify YAML or Markdown files under `.ai-context/`.
- **文件**：修改 `docs/` 或 `README.md`。
  - **Docs**: Modify `docs/` or `README.md`.

### 步驟 4：測試驗證 (Critical!) / Step 4: Verify (Critical!)
由於本專案會影響 AI 的行為，**測試非常重要**。
Since this project affects AI behavior, **testing is crucial**.

在提交之前，請務必在至少 **2 種不同類型的專案**（例如一個前端 React 專案、一個後端 Python 專案）中測試你的修改：
Before submitting, be sure to test your changes in at least **2 different types of projects** (e.g., one frontend React project, one backend Python project):

1. 將修改後的 `.ai-context` 和 `.cursor/rules` 複製到測試專案中。
   - Copy the modified `.ai-context` and `.cursor/rules` to the test projects.
2. 執行相關 Agent（如 Bootstrap 或 Inventory）。
   - Run relevant Agents (e.g., Bootstrap or Inventory).
3. 驗證 AI 的輸出是否符合預期，且沒有破壞現有功能。
   - Verify that the AI output meets expectations and does not break existing functionality.

### 步驟 5：提交 Pull Request / Step 5: Submit Pull Request
1. 將修改 Push 到你的 Fork：
   - Push changes to your Fork:
   ```bash
   git push origin feature/improve-bootstrap-logic
   ```
2. 在 GitHub 上發起 Pull Request (PR)。
   - Open a Pull Request (PR) on GitHub.
3. 在 PR 描述中說明：
   - In the PR description, explain:
     - 包含的變更內容。 (Changes included.)
     - 你在哪些類型的專案上進行了測試。 (Which project types you tested on.)
     - 相關的 Issue 編號（如果有的話）。 (Related Issue number, if any.)

## 📝 風格指南 / Style Guide

- **Agent 規則 (.mdc) / Agent Rules (.mdc)**：
  - 保持指示簡潔明確。 (Keep instructions concise and clear.)
  - 使用 YAML frontmatter 定義觸發條件。 (Use YAML frontmatter to define trigger conditions.)
  - 每個規則檔建議控制在 300 行以內。 (Keep each rule file under 300 lines.)
- **文件 (Markdown) / Documentation (Markdown)**：
  - 請保持格式一致。 (Keep format string consistent)
  - 使用清楚的標題層級。 (Use clear heading levels.)

## 🐛 回報 Bug / Reporting Bugs

如果你發現 Bug，請建立一個 Issue，並提供以下資訊：
If you find a Bug, please create an Issue and provide the following information:

- 你的作業系統與環境。 (Your OS and environment.)
- 重現步驟。 (Steps to reproduce.)
- 預期行為與實際行為。 (Expected vs. actual behavior.)
- 相關的錯誤訊息或截圖。 (Relevant error messages or screenshots.)

再次感謝你的貢獻！讓我們一起讓 AI 開發體驗更美好。
Thank you again for your contribution! Let's make the AI development experience better together.
