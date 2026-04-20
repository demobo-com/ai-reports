# 各類 Agent 技術進展總覽（2026 現況）

本文整理主要 agent 行動類別的當前技術成熟度，包括代表性 benchmark、目前能做到的事，以及仍然做不好的地方。

## 總結

截至 2026 年，最成熟的 agent 類別是 **CLI / 程式開發 agent** 與 **結構化 API / tool-use agent**。這兩類最接近可生產化，因為它們通常處在較受限的環境中，回饋訊號清楚，也比較容易驗證結果。

**瀏覽器操作** 與 **試算表 / 檔案工作流** 已有明顯進步，但穩定性仍低於 coding agent 與結構化工具調用。

**桌面 GUI / computer use**、**偏溝通型 assistant 工作流**、以及 **機器人 / 裝置控制** 目前仍明顯落後。它們可以做出驚豔 demo，但長流程可靠性仍不足。

一個重要提醒：不同 benchmark 的分數 **不能直接橫向比較**。試算表 benchmark 的 70%，不代表和 OSWorld 或 SWE-bench 的 70% 是同等能力。

---

## 1. GUI / Computer Use

### 代表 benchmark
- **OSWorld**

OSWorld 評估 agent 在 Ubuntu、Windows、macOS 上執行真實電腦任務的能力，涵蓋桌面應用、檔案操作、瀏覽器任務，以及跨應用工作流。

原始 OSWorld 論文報告：
- 人類成功率：**72.36%**
- 當時最佳模型：**12.24%**

之後 OpenAI 公布的 computer-using agent 成績為：
- **OSWorld 38.1%**

### 現在能做到什麼
目前的 computer-use agent 通常可以完成：
- 短流程桌面任務
- 基礎檔案操作
- 表單填寫
- 在不同應用之間複製資訊
- 簡單設定調整
- 視覺上明確、分支少的多步驟操作

當介面乾淨、流程不複雜時，這類系統已經越來越可用。

### 目前還做不好的地方
它們仍然容易卡在：
- GUI grounding
- 模糊或相似的介面元素
- 隱藏狀態
- 跳出視窗與 modal 中斷
- 長流程中的錯誤恢復
- 版面突然變動
- 跨應用任務中一步錯就全盤出錯

實務上常見問題包括點錯按鈕、被彈窗打斷後無法恢復、或在意外對話框後失去上下文。

### 結論
Computer use 已經不是概念展示，而是在快速進步中，但距離「放心把一般桌面工作完全交給它」還有一段距離。

---

## 2. Web / Browser Use

### 代表 benchmark
- **WebArena**
- **WebVoyager**
- **VisualWebArena**
- **BrowseComp**

這些 benchmark 主要測試網頁導航、資訊蒐集、持續搜尋、表單填寫，以及帶有視覺理解需求的瀏覽任務。

OpenAI 公布的 CUA 成績包含：
- **WebArena 58.1%**
- **WebVoyager 87.0%**

BrowseComp 則是更難的 benchmark，專門測試在真實網路中持續搜尋與找到難查答案的能力。

### 現在能做到什麼
瀏覽器 agent 現在相對擅長：
- 導航主流網站
- 從多個頁面抽取指定資訊
- 完成中等長度表單
- 比價與資訊查找
- 操作 dashboard 與 CRUD 類網頁系統
- 執行中度結構化的瀏覽流程

### 目前還做不好的地方
它們仍然容易卡在：
- 動態性高或視覺複雜的網站
- 登入驗證摩擦
- anti-bot 阻擋
- 長且分支多的流程
- 指令本身不夠清楚
- 需要結合多個系統上下文的任務
- 真正完整的 end-to-end booking / checkout

例如 agent 可能能找到幾個符合條件的航班，但在真正下單時，若牽涉會員規則、座位限制、隱藏費用或公司政策，就很容易失敗。

### 結論
Browser use 今天已經有實用價值，但一旦流程變長、需求變模糊、或狀態變得複雜，可靠性會明顯下降。

---

## 3. CLI / Terminal Use

### 代表 benchmark
- **SWE-bench Verified**
- **SWE-bench-Live**

這些 benchmark 主要評估 coding agent 在真實軟體工程任務上的表現，例如修 bug、補測試、處理 GitHub issue 等。

截至 2026 年，這是最成熟的一類 agent。
官方 SWE-bench 排行顯示頂尖系統已達到 **高 70% 區間**，而 OpenAI 報告 GPT-5.2 在 SWE-bench Verified 上達到：
- **80.0%**

### 現在能做到什麼
CLI / coding agent 已經非常實用，適合：
- 修局部 bug
- 撰寫或更新測試
- 做 repo 範圍重構
- 執行指令並解讀輸出
- 診斷 build 失敗
- 反覆修改程式碼直到測試通過
- 解決不少真實 repo 裡的 issue

### 目前還做不好的地方
它們仍然容易卡在：
- 模糊的產品需求
- 深層架構重設計
- 可觀測性差的環境
- flaky build
- 需要商業背景知識的 bug
- 大型跨系統除錯
- 非標準平台與麻煩的安裝環境

### 結論
CLI / coding agent 是目前最成熟、也最具商業價值的一類 agent，特別適合放在 sandbox 與 human review 的框架中使用。

---

## 4. API / Tool Use / MCP 類動作

### 代表 benchmark
- **BFCL**
- **τ-bench**

BFCL 主要衡量 function calling 本身的準確率。
τ-bench 更貼近真實世界，會測試多輪互動、政策限制、以及在零售、航空客服等場景下的工具調用能力。

### 現在能做到什麼
Tool-using agent 現在相對擅長：
- 在受限工具集合中選對工具
- 從清楚指令中抽出參數
- 在流程明確時串接多個工具
- 操作 CRM、SQL、搜尋、日曆、工單等結構化系統

### 目前還做不好的地方
它們仍然容易卡在：
- 使用者需求不完整
- 政策與規則密集的決策
- 工具部分失敗後的恢復
- 長流程中的狀態追蹤
- 多次重跑時不一致
- 同時處理商業規則、對話與行動執行

這正是「能生成合法 API call」與「能穩定完成實際營運工作」之間的差距。

### 結論
單純 tool calling 已經很強，但真正可靠的多步驟 agentic tool use 仍然明顯脆弱。

---

## 5. File / Document / Spreadsheet Operations

### 代表 benchmark
- **SpreadsheetBench**

SpreadsheetBench 主要評估在複雜工作簿中進行除錯、建模、視覺化等端到端試算表任務。

已公開結果包括：
- **Gemini in Google Sheets：70.48%**
- **Excel Agent Mode：57.2%**
- **ChatGPT agent：45.5%**（某公開設定中的 spreadsheet editing 成績）

### 現在能做到什麼
試算表與檔案 agent 現在常能做到：
- 讀取並總結試算表
- 撰寫公式
- 修復部分損壞的工作簿
- 建立圖表
- 進行多步驟 workbook 編輯
- 回答結構化檔案中的問題

### 目前還做不好的地方
它們仍然容易卡在：
- 模糊的試算表慣例
- 細微的財務假設
- 非常混亂的真實世界表格
- 版面美觀與呈現品質
- 可刷新性與可稽核性
- 在複雜模型中保留人類可理解的邏輯

至於文件、PDF、簡報，目前 benchmark 仍不像 coding 或 browser 那樣成熟。能力有，但評測體系比較分散。

### 結論
試算表與檔案工作流已經開始有明顯實用性，但可靠性很依賴原始檔案的結構與整潔程度。

---

## 6. Communication / Digital Assistant Workflows

### 代表 benchmark / proxy
- **TheAgentCompany**
- **τ-bench**
- **LiveClawBench**

這些 benchmark 或類 benchmark 環境涵蓋 email、排程、溝通、瀏覽與跨系統協調等助理型工作。

TheAgentCompany 報告指出，在其設定中表現最好的 agent 也只有大約 **30%** 任務能完全自主完成。

### 現在能做到什麼
這類 agent 現在適合：
- inbox triage
- 會前整理
- 摘要 thread
- 起草回覆
- 抽取 action items
- 在條件明確時協助排程

### 目前還做不好的地方
它們仍然容易卡在：
- 社交語境與語氣細節
- 隱藏偏好
- 指令不完整
- 跨系統協調
- 任務中途條件變更
- 記憶與個人化一致性
- 端到端 executive-assistant 等級的自治能力

### 結論
偏溝通型 agent 已經很適合做 copilot，但距離成為能獨立處理真實組織混亂工作流的 unsupervised assistant 還不夠成熟。

---

## 7. Device / Hardware / Robotics Control

### 代表 benchmark
- **BEHAVIOR**
- **EmbodiedBench**

這些 benchmark 主要測試 embodied agent 在家務與機器人任務中的導航、規劃與操作能力。

EmbodiedBench 報告顯示，即使是當時測試中最好的模型 GPT-4o，平均成功率也只有：
- **28.9%**

### 現在能做到什麼
Embodied agent 現在通常可以：
- 在模擬環境中做高層規劃
- 理解指令
- 完成簡單的導航或操作
- 做出條件受控下很亮眼的 demo

### 目前還做不好的地方
它們仍然容易卡在：
- 靈巧的低階操作
- 長流程任務執行
- 部分可觀測環境
- 物理誤差累積
- 抓取失敗後的恢復
- sim-to-real transfer
- 非結構化真實環境中的穩定表現

### 結論
機器人與裝置控制 agent 仍是整體上最不成熟的一類，特別是在通用可靠性方面。

---

## 實務成熟度排序

若從實際可落地程度來看，排序大致如下：

1. CLI / coding
2. API / 結構化 tool use
3. 試算表 / 檔案工作流
4. Web / browser use
5. Communication / assistant workflows
6. GUI / full computer use
7. Device / robotics control

這個排序反映的是實務成熟度，不是市場 hype。

---

## 各類別共同規律

### Agent 最擅長的情況
- 目標清楚
- 工具集合有限
- 流程長度短到中等
- 中間狀態可驗證
- 規則與偏好明確

### Agent 最容易失敗的情況
- 意圖沒有說清楚
- 流程很長且分支多
- 需要跨很多步驟維持狀態
- 失敗後必須自行恢復
- 需要協調多個系統
- 隱藏偏好或商業規則很重要

---

## 最後結論

誠實地說，2026 年的真實圖景是：

- Agent 已經不只是 demo。
- 在受限環境中，它們已經有明確價值。
- 但它們距離成為在整個數位與實體世界中都可靠的通用自主工作者，還有很大距離。

對產品團隊來說，近期最值得切入的方向仍然是那些目標、工具與評估都比較受限的場景：
- coding
- 結構化 tool use
- 試算表
- 定義明確的 browser workflow
