# Design

## Context

本變更落實 `myplan/DESIGN.md` 中規劃之基礎設施、八大模組資料契約、模組 ④ 官方資料取得層與模組 ⑥ 官方領域知識規則庫。專案需在兼顧時空一致性（UTC 與 Asia/Taipei）與學術誠實原則的前提下，為後續之時序回測（模組 ⑤）與 Planner Agent（模組 ③）提供強型別接口。相關動機請參見 `proposal.md`。

## Goals / Non-Goals

**Goals:**
- 建立 Python 現代化套件架構（`pyproject.toml`、`src/airguard/` 各子模組）。
- 實作基於 Pydantic v2 的核心資料模型（`QuerySpec`, `Observation`, `OfficialAQI`, `ForecastBundle`, `RuleEvidence`, `ResponseEnvelope`）。
- 實作環境部 API 客戶端（`AQX_P_432` 即時 AQI、`AQX_P_13` 小時污染物值），支援本地檔案快取與指數退避重試。
- 實作 2025 年新制環境部 AQI 分級與一般／敏感族群活動建議查詢，保證確定性計算。

**Non-Goals:**
- 不在此變更實作機器學習模型訓練或推論（留待後續 `ml-forecasting` 提案）。
- 不在此變更實作 LLM Prompt 調度與自然語言生成（留待後續 `agent-planner` 提案）。
- 不在此變更實作 Web UI 介面（留待模組 ① 開發階段）。

## Decisions

### 1. 使用 Pydantic v2 定義核心資料契約
- **決策**：所有跨模組資料結構皆繼承自 `pydantic.BaseModel`，並強制驗證時間戳與非負數值。
- **理由**：避免動態 dict 傳遞造成的鍵名錯誤與型別隱性問題，支援自動序列化為 JSON Schema。
- **替代方案**：標準庫 `dataclasses`（缺乏內建進階驗證與型別轉換）。

### 2. 資料取得層採本地磁碟快取與 TTL 機制
- **決策**：API 查詢結果以 JSON 格式儲存於 `.cache/` 目錄，設定每小時自動更新（TTL=3600s）。
- **理由**：降低環境部 API 請求頻率，符合 API 呼叫禮貌原則，並在網路中斷或上游故障時提供優雅降級資料。
- **替代方案**：記憶體快取（行程結束即消失，無法供重複測試使用）。

### 3. 預測目標與 AQI 分級分離計算
- **決策**：預測模型僅針對 PM2.5 物理濃度（μg/m³）進行預測；AQI 分級嚴格由領域規則模組依據 2025 最新門檻換算。
- **理由**：2025 年環境部修訂 AQI 門檻，跨年度標籤若直接以歷史 AQI 分級訓練會產生制度性洩漏；解耦後能保證物理意義與法規一致。

## Risks / Trade-offs

- **[環境部 API 欄位異動或格式更動]** → 在取得層加入 Schema 檢驗，若欄位缺失則回報 `QualityReport` 並降級為部分可用，不拋出未捕獲例外。
- **[跨時區時間混淆]** → 內部時間統一以 UTC 儲存，所有向使用者展示的介面均明確標註 `Asia/Taipei`（UTC+8）。
- **[上游 API 延遲或服務中斷]** → 設定 10 秒連線超時與最多 3 次指數退避重試，超時後回退至合法快取並標記 `stale`。
