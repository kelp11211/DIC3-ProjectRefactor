# Tasks

## 1. 專案工程骨架與配置

- [ ] 1.1 建立 `pyproject.toml` 並定義依賴套件（pydantic, requests, pandas, pytest），透過指令驗證設定語法
- [ ] 1.2 建立 `src/airguard/` 與模組目錄結構（`contracts/`, `data/`, `domain/`, `ml/`, `agent/`），驗證模組可正確被 Python import

## 2. 核心資料契約實作 (Core Contracts)

- [ ] 2.1 實作 `QuerySpec`、`Observation`、`OfficialAQI` 之 Pydantic 模型，驗證時區與非負數值檢驗邏輯
- [ ] 2.2 實作 `ForecastBundle`、`RuleEvidence` 與 `ResponseEnvelope` 模型，驗證包含誤差帶與來源稽核追蹤欄位
- [ ] 2.3 撰寫契約單元測試 `tests/test_contracts.py`，驗證合法與非法資料實例化行為

## 3. 官方資料取得層實作 (Data Acquisition)

- [ ] 3.1 實作環境部 API 客戶端 `src/airguard/data/client.py`，支援 `AQX_P_432` 與 `AQX_P_13` 下載
- [ ] 3.2 實作本地磁碟快取與重試裝飾器，驗證連線失敗時能平滑讀取快取並標註 stale
- [ ] 3.3 實作長表時間序列轉換與品質檢驗器 `src/airguard/data/validator.py`，驗證單位與極值檢核功能
- [ ] 3.4 撰寫資料層單元測試 `tests/test_data_acquisition.py`，使用 Mock 驗證資料解析與異常處理

## 4. 領域知識規則庫實作 (Domain Rules)

- [ ] 4.1 實作 2025 最新 AQI 分級判定引擎 `src/airguard/domain/aqi.py`，驗證 6 級門檻邊界值
- [ ] 4.2 實作一般族群與敏感族群活動建議對照表 `src/airguard/domain/guidance.py`，驗證指引文字與官方一致性
- [ ] 4.3 實作醫療防越權檢查器，驗證用藥與個人診斷關鍵字能被 100% 阻斷並轉介
- [ ] 4.4 撰寫領域規則單元測試 `tests/test_domain_rules.py`，驗證規則計算與防護邊界

## Workflow follow-up

- 實作完成後執行 `pytest` 確保所有測試通過
- 執行 `openspec validate init-airguard-foundation` 確認規格完整性
- 執行 `/opsx-archive` 將能力規格合併至 `openspec/specs/`
