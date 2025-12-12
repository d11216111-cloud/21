# 21點（Blackjack） 單檔示範 — 軟體開發規格（Software Development Specification）

**版本：** 1.0  
**語言：** 繁體中文  
**目標：** 提供一份簡潔且可驗證的開發規格，並附上一個 single-file (index.html) 範例，示範 deterministic PRNG + Fisher‑Yates 洗牌與抽牌的基本 UI。

---

## 1. 範圍（Scope）

- 此規格涵蓋單檔示範（index.html）之功能與行為，僅實作「隨機抽牌（含 deterministic seed 支援）」與最小 UI，非完整 Blackjack 遊戲引擎。
- 可用作教學、回放重現或作為進一步完整遊戲之基礎。

---

## 2. 功能性需求

- **R1:** 使用者可輸入 seed（任意字串或數字）；當提供相同 seed 且相同規則時，洗牌與抽牌序列應可重現（deterministic）。
- **R2:** 提供「Shuffle」按鈕，使用 seed 初始化 PRNG 並以 Fisher‑Yates 洗牌。
- **R3:** 提供「Draw」按鈕，每次從牌堆頂端抽一張並顯示於「已抽」區域。
- **R4:** 顯示剩餘牌數與已抽牌序列（可視化簡單字串或圖示）。
- **R5:** 提供「Reset」按鈕，復原為未洗牌狀態、清除已抽牌、保留先前輸入的 seed（視使用者選擇）。
- **R6:** 提供「Random seed」按鈕（或勾選），可不輸入 seed 時產生隨機 seed 並顯示，方便分享或回放。

---

## 3. 非功能性需求

- **N1:** 單檔（index.html）應無需外部資源（CSS/JS CDN 皆不引用），以方便離線開啟。
- **N2:** 程式碼清楚註解（中文註解）並具易讀性，方便後續擴充。
- **N3:** UI 需簡潔、在桌面和手機瀏覽器都能基本可用（響應式為加分）。

---

## 4. 技術與演算法規範

- **PRNG：** 採用小型且常用的 deterministic PRNG（例如 mulberry32），輸入 seed（字串時先 hash（簡單 hash32）轉為整數）。
- **洗牌：** 使用 Fisher‑Yates，隨機來源改為 PRNG（nextFloat()），確保同 seed 及相同牌陣輸出相同順序。
- **牌組：** 預設 52 張標準撲克牌（4 suits × 13 ranks），以 rank+suit 字串表示（例如 "A♠", "10♥", "K♦"）。
- **序列化：** 索引與文字輸出用固定格式（例如 "♠A" 或 "A♠"）以方便比對。

---

## 5. UI/UX 規格（最小）

- **Header：** 應用名稱與簡短說明。
- **控制列：**
  - Seed 輸入欄位（可輸入字串或數字）
  - Shuffle 按鈕
  - Random seed 按鈕（會生成並填入 seed）
  - Draw 按鈕（抽一張）
  - Reset 按鈕
- **顯示區：**
  - 當前牌堆剩餘張數
  - 已抽牌面板（顯示已抽之牌）
  - 完整牌堆（選用：可視化全部牌順序以便驗證 deterministic）
- **簡短說明文字：** 解釋 seed、deterministic 洗牌 與 使用方式

---

## 6. 接受準則（Acceptance Criteria）

- **AC1:** 在同一瀏覽器環境，輸入相同 seed、按下 Shuffle 後，按相同次序 Draw 每張牌都與先前的結果一致（可透過在兩次 run 輸出已抽序列比對）。
- **AC2:** 若不輸入 seed 並使用 Random seed，產生之 seed 應顯示，且能被用於回放。
- **AC3:** Reset 能使畫面回到初始（未抽）狀態，但不強制清除 seed（設為可選行為）。
- **AC4:** 所有互動皆在單一 index.html 中完成，且不依賴伺服器。

---

## 7. 測試要點（Test Cases）

### TC1: Deterministic 重現
- **操作：** 輸入 seed = "test42"，Shuffle，連續 Draw 5 張（記錄序列）。Reload 分頁，再輸入相同 seed、Shuffle、Draw 5 張。
- **預期：** 兩次序列完全一致。

### TC2: Random seed 與分享
- **操作：** 按 Random seed，複製顯示的 seed，Reload → 貼上該 seed → Shuffle → Draw，檢查序列。
- **預期：** 序列一致。

### TC3: Reset 行為
- **操作：** Shuffle → Draw 3 → Reset → Draw（若自動再 shuffle 則結果應與 reset 規格一致）
- **預期：** Reset 之後牌堆回初始、已抽牌清空。

### TC4: 邊界：抽完全部 52 張再 Draw
- **預期：** 提示無牌可抽、Draw 不會拋例外。

---

## 8. 擴充點（非必須）

- 支援多副牌（deckCount）
- 加入簡單 blackjack hand 評估（點數計算）
- 回放（Replay）功能：匯出 seed 與 action list（JSON）

---

## 9. 交付物

- **SOFTWARE_SPEC.md**（本檔）
- **index.html**（單檔示範）

---

## 10. 版本與作者

- **作者：** Copilot（示範）
- **日期：** 2025-12-12
- **版本：** 1.0
