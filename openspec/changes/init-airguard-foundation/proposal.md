# Proposal

## Why

AirGuard AI 專案定位為 Domain AI Agent，核心精神在於整合環境部官方監測資料、領域規則與時序分析，為戶外運動者提供可信且可解釋的空氣品質決策建議。
為了依照 `myplan/DESIGN.md` 所規範的八大模組架構開展實作，本變更旨在建立專案的基礎工程骨架、定義跨模組強型別資料契約（Data Contracts），並優先落實模組 ④「官方資料取得」與模組 ⑥「領域知識規則」，為後續時序模型預測（模組 ⑤）與 Agent 任務規劃器（模組 ③）奠定可靠基礎。

## What Changes

- **專案環境與相依性設定**：配置 `pyproject.toml`，規範 Python 3.10+ 執行環境，安裝 Pydantic v2、Requests/Httpx、Pandas 等基礎相依套件。
- **核心資料模型定義（Core Contracts）**：依據 DESIGN.md 定義 `QuerySpec`、`Observation`、`OfficialAQI`、`ForecastBundle`、`RuleEvidence` 與 `ResponseEnvelope` 之 Pydantic 資料模型與驗證邏輯。
- **官方資料取得層實作（Data Acquisition）**：
  - 封裝環境部開放資料平臺 API 客戶端（支援 `AQX_P_432` 每小時 AQI 與 `AQX_P_13` 小時污染物值）。
  - 提供本地快取與重試機制，防止過度請求與網路瞬斷。
  - 實作資料檢驗器（Data Validator），確保時間戳（UTC/Asia-Taipei）、數值單位（μg/m³）與測站 ID 一致性。
- **領域知識規則引擎（Domain Rules）**：
  - 依據 2025-01-01 起生效之環境部 AQI 最新修訂門檻，實作 AQI 分級判定。
  - 實作一般族群與敏感族群在不同污染分級下的戶外活動指引查詢。
  - 嚴格落實防越權防線：區隔官方發布 AQI 與模型 PM2.5 預測，禁止直接將單一污染物濃度冒充官方 AQI。

## Capabilities

### New Capabilities
- `core-contracts`: 定義系統八大模組間溝通的強型別資料契約與驗證標準。
- `data-acquisition`: 取得環境部官方即時與歷史空品資料、具備快取、重試與資料品質驗證。
- `domain-rules`: 依據環境部 2025 最新法規判定 AQI 分級與各族群活動指引。

### Modified Capabilities
<!-- None -->

## Impact

- **新增模組代碼**：`src/airguard/contracts/`、`src/airguard/data/`、`src/airguard/domain/`。
- **新增配置檔案**：`pyproject.toml`。
- **依賴套件**：引入 `pydantic>=2.0`、`requests>=2.31`、`pandas>=2.0`。
- **向下相容**：此為基礎建置，不影響現有 `myplan/` 下的文檔資料。

