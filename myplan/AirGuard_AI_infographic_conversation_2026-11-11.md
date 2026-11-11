# AirGuard AI 資訊圖表設計對話紀錄

> 整理日期：2026-11-11（臺灣時間）  
> 來源：本次 ChatGPT 對話（依對話順序整理）  
> 形式：對話摘要與決策紀錄，**非逐字逐句匯出**。附件與產生的圖片並未以二進位檔上傳此儲存庫。

## 1. 專案背景

主題為 **AirGuard AI：智慧空氣品質預測與生活決策代理人**。使用者提供《空品決策代理人.pdf》，內容分析 AirGuard AI 提案報告及簡報，參照《A2Z outline example》科研論文／提案 SOP 與期末專題提案規範。

核心定位為 Domain AI Agent：不能只做模型或聊天機器人，應能讀取領域資料、分析、推理、調度工具，提供可驗證、有來源且可解釋的戶外活動建議。採官方 AQI 資料及領域規則，與 PM2.5 1／3／6 小時模型預測明確區分；不可捏造實驗成果或健康保證。

### 系統八大模組

1. 使用者介面：地點、時間、活動條件輸入。
2. 查詢解析：辨識意圖、時間、地點、族群。
3. Planner Agent：以白名單工具規劃和調度。
4. 官方資料取得：環境部即時／歷史空品資料（AQX_P_432／488／13）。
5. 數據分析與 ML：PM2.5 1／3／6 小時預測、回測、不確定性。
6. 領域知識規則：官方 AQI 分級及一般／敏感族群活動指引。
7. 決策建議：對時段、風險做保守比較並標明依據。
8. 結果互動與稽核：顯示來源、時間戳、限制和紀錄。

### 評估規劃

- Track A：PM2.5 1／3／6 小時預測；Persistence／Seasonal Naive 基線；MAE、RMSE、Skill_MAE；按時間切分與 rolling-origin 回測，避免未來資訊洩漏。
- Track B：至少 30 個 Agent 情境測試，確認工具調度、來源、規則一致性、安全降級及回應時間。
- 目前屬 Proposal 階段，僅能記載 Expected Findings／Expected Outcomes，不可宣稱已完成實驗。

## 2. 對話時間線與修改要求

### 2026-11-11 10:18（臺灣時間）— 初始製圖

使用者：「請分析內容 做成 資訊圖表 A4 橫式 高密度 numbering」

要求：分析《空品決策代理人.pdf》並製作 A4 橫式、高資訊密度、明確編號的資訊圖表。初版內容包括：
- 核心原則與專案定位；
- A–F 六段式摘要；
- Introduction／Related Work／Deep Research；
- Proposed Scheme：八大模組、系統架構、流程、演算法；
- 模型與 Agent 雙軌評估；
- A2Z SOP 分組對照。

### 2026-11-11 10:34 左右— 參考 DietSense 排版

使用者上傳「DietSense 專題提案系統性解析資訊圖表」參考圖，要求：
「請參考上傳的圖片，將原本產出的圖片，A2Z的每一個步驟的細節都列出來。」

設計變更：A2Z 不再只有 A–F、G–L 等群組摘要，須展開 **A～Z 全部 26 個字母**。維持 A4 橫式、高密度、藍白科技風、清晰的表格與章節 Numbering。

### 2026-11-11 10:38 左右— A2Z 欄位整併

使用者明確修正：
「A2Z 完整展開：A～Z 共 26 個步驟，逐項列出名稱、核心內容及 DOCX／PPTX 對照。這個部分，我不需要核心內容及 DOCX／PPTX 對照，我要的是將其整合到A2Z的『定義/目標』這個欄位」

此為**最新且優先的版面決策**：
- 表格應為「字母／編號」、「步驟名稱（Letter）」、「**定義／目標**」三欄。
- 不要另外設「核心內容」、「DOCX 對照」、「PPTX 對照」。
- 「定義／目標」同一欄整合 SOP 步驟原意、AirGuard AI 應用目標與實際做法。
- 使用者另上傳《A2Z_outline_example_(2020.3.5).docx》作為**步驟名稱與原意的一手依據**，以及《AirGuard_AI_Final_Proposal_Presentation.pptx》作為專案內容依據。
- 先前生成圖表中個別英文步驟名稱不吻合原始 A2Z SOP；後續版本應以原始 SOP 修正。

## 3. 原始 A2Z SOP 對照要點（應優先尊重上傳 DOCX）

來源：使用者上傳《A2Z_outline_example_(2020.3.5).docx》；保留原文術語，勿以過度概括的名稱取代。

### Abstract（A–F）

- **A — Attention Getter**：動機及引題。
- **B — But**：挑戰與轉折。
- **C — Cure**：提出解法。
- **D — Development**：方法的設計基礎與內容。
- **E — Experiments**：如何評估所提方法。
- **F — Findings**：成果；本專題 Proposal 階段須標示為「預期成果」。

### Introduction（G–L）

- **G — General**：研究現況。
- **H — However**：研究挑戰，詳細延伸 B。
- **I — In literature**：既有文獻的解法與分類。
- **J — Judgement**：評論方法優缺點、SWOT。
- **K — Keypoint**：設計精神、novelty 與主要貢獻。
- **L — List the organization**：交代文章架構。

### Related Work（M–O）

- **M — Methods**：相關方法依發展與複雜度分類。
- **N — New proposed**：新趨勢／新穎解法。
- **O — Organize**：整理、比較、表格化歸納優缺點。

### Proposed Scheme（P；內含 PA–PM）

- **P** 為 Proposed Scheme 大階段，**不是只一項細節**。
- **PA — Aim / Statement**：設計目標。
- **PB — Based on / Background / Be focus on / Benefit**：設計根據、焦點與效益。
- **PC — Cure / Cause**：解法與原因。
- **PD — Design**：系統設計架構。
- **PE — Paper element**：元素／組成。
- **PF — Formulation / Foundation**：功能、形式化或基礎。
- **PG — Graph**：架構圖。
- **PH — How**：如何達成設計目標。
- **PI — Implementation / Interaction**：元件互動、流程圖或偽代碼。
- **PJ — Jump to an example**：案例。
- **PK — Key points contribution**：關鍵貢獻。
- **PL — Later how to do simulation**：後續模擬／評估方案。
- **PM — Math proof**：數學證明（若適用；不要無據宣稱已有）。

### Simulation（Q–V）

- **Q — Quality and Quantity**：實驗量與品質指標、績效指標。
- **R — Related method to compare**：對比演算法／方法。
- **S — Simulation Setup**：實驗設置。
- **T — Tuning what parameters in simulations**：參數調整。
- **U — Useful results**：有用的結果（本提案尚未實驗，只能列預期分析）。
- **V — Verify the effectiveness of our proposed scheme**：有效性驗證規劃。

### Conclusion（W–Z）

- **W — What proposed**：回顧動機、問題與提出的方法。
- **X — eXcel**：特色、擅長情境與解法優點。
- **Y — Yields**：成果與實驗數據；Proposal 階段只列預期成果。
- **Z — Zen**：研究結論、貢獻和未來工作。

## 4. 最新資訊圖表規格（後續持續編修依據）

- **紙張**：A4 橫式。
- **內容密度**：高；但以繁體中文文字的正確性、可閱讀性優先。
- **語言**：繁體中文（臺灣用語），保留必要英文技術術語。
- **風格**：參考 DietSense 範本；藍白／深藍科技風、區塊化、表格化、強調清楚的 numbering。
- **主體章節**：① 核心原則與定位、② A–F 摘要、③ 文獻與 Deep Research、④ 系統架構與流程、⑤ 評估規劃、⑥ A2Z 完整展開、⑦ 整體結論。
- **A2Z 主表欄位**：字母／編號、步驟名稱、定義／目標；每個 A～Z 都應單獨有一列。
- **A2Z 定義／目標**：不要另設核心內容或 DOCX／PPTX 頁碼欄；將 SOP 原意與 AirGuard AI 的對應做法寫在本欄。
- **P 段細節**：原始 SOP 另外列有 PA–PM，若版面允許應以小型內嵌面板補充，不要誤當 A～Z 之外另 13 個主要字母步驟。
- **學術誠實**：明確標示目前 Proposal 階段；模型效果、系統成果及實驗均屬預期，不可虛構。
- **需留意**：AI 生成圖片可能產生錯字或細字錯置，正式版本應先核稿，再輸出可印製的高解析度檔案。

## 5. 已提及／上傳的參考資料

1. 《空品決策代理人.pdf》：針對 AirGuard AI 提案報告與簡報之系統性解析。
2. 《DietSense資訊圖表.png》：使用者指定的版型參考。
3. 《A2Z_outline_example_(2020.3.5).docx》：A2Z 步驟與 PA–PM 定義原始依據。
4. 《AirGuard_AI_Final_Proposal_Presentation.pptx》：AirGuard AI 24 頁提案簡報。
5. 曾由助理生成數版 AirGuard AI 資訊圖，但**圖片本身尚未加入 GitHub 儲存庫**。

## 6. 後續行動

1. 依原始 A2Z SOP 校對每一字母的英文名稱與順序。
2. 將「定義／目標」同時寫清楚 SOP 意義與 AirGuard AI 專案對應做法。
3. 保留 P（Proposed Scheme）內部 PA–PM 作為技術細節，避免被簡化成不符原文的 Q～V 名稱。
4. 校對排版密度與文字，避免因 AI 圖片文字生成產生錯誤。
5. 若需正式交付，可進一步製作可編輯的 SVG／PDF／HTML 排版，而非只依靠生成圖像。

---
本檔為基於當前對話內容的整理紀錄；並非 ChatGPT 系統完整聊天紀錄匯出，也不包含附件原檔。
