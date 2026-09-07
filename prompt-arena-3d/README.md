# Prompt Arena 3D — PM Mayors

真正以 WebGL / Three.js 即時繪製的 3D 提問教學遊戲。原創低多邊形大頭角色、城堡競技場、武器、寵物與攻擊動畫，不需圖片素材或 AI API 金鑰。

## 遊戲功能

- 12 位 3D 人物（6 男、6 女）、6 種 3D 寵物。
- 成人與小朋友各 200 題，共 400 題，每題獨立 Markdown。
- 各題庫：新手村 20、簡單 50、進階 50、非常難 50、大魔王 30。
- 關卡全開；一般每場 5／10／20 題，魔王每場同一情境 3 回合。
- 答對攻擊、連擊加分、答錯解析；血量歸零可繼續學習。
- 老師模式：指定起始題目、選答後才揭曉、跳題。
- 選項亂序但答案綁定 ID；同場不重複；完成後回顧每題。
- 徽章僅存在當前瀏覽器，沒有學生個資、登入、跨裝置排行榜。
- 主介面只按對象與難度分類，不提供技巧類別篩選。

## ACTORS 的本課程定義

| 字母 | 本專案沿用定義 | 重點 |
|---|---|---|
| A | Action | 任務 |
| C | Content | 背景、資料、情境 |
| T | Style | 語氣、格式、風格 |
| O | Goal | 目的、成功標準 |
| R | Refer | 參考、範例、限制、比較基準 |
| S | Steps | 流程、檢查點 |

T 對應 Style、O 對應 Goal，依使用者提供的 2026PromptArena 評分程式保留。另融入 Zero-shot、範例提示、迭代、資訊查證及指令與資料分界。每題只評估情境所需的技巧，不以 Prompt 長度或是否塞滿六項判分。

題庫以 50 個兒童生活情境與 50 個成人工作情境，從初次提問、條件安排與回答修正等不同角度出題。遊戲呈現的 AI 回覆為預先編寫的教學模擬，不是即時生成。建議教師依實際班級閱讀程度持續調整選項與難度。

## 在 GitHub 自己增加題目

1. 開啟 `content/questions/kids/`（兒童）或 `content/questions/adults/`（成人）。
2. 選難度資料夾：`village`、`easy`、`medium`、`hard`、`boss`。
3. 複製 `docs/QUESTION-TEMPLATE.md`，改掉 ID、題目、選項、答案和解析。
4. 用 GitHub 的 **Add file → Upload files** 上傳 `.md`，或 **Create new file** 直接編輯。
5. Commit changes。若 Vercel 已連接這個 GitHub 專案，會自動驗證題庫並部署。

不需修改題目清單，新增題數會自動顯示。`answer` 是固定選項 ID（小寫 a/b/c/d），不是畫面上的 A/B/C/D 順序；遊戲會打亂畫面順序。

每題都必須有四個不同選項、正解及四份解析。系統會阻止重複 ID、漏章節、無效答案、同一情境與問題完全重複、魔王組不完整等錯誤。錯誤訊息會指出 MD 路徑。

魔王新增一組需上傳三個 MD，共用 `group`，`round` 分別填 1、2、3；各回合應交代前一輪已完成的事項，讓抽題能保持連貫。

**一般更新只改 MD，不要執行 `scripts/seed-bank.py`。** 那是首版內容編製工具，重新執行會覆寫原始 400 題。正常建置完全不執行它。

## Vercel 部署與 GitHub 自動更新

這是無外部套件依賴的靜態網站；Three.js 已附在 `dist/vendor`。

1. Vercel → Add New → Project → 匯入 GitHub 儲存庫。
2. 如果專案存在儲存庫的子資料夾，Root Directory 設成 **`prompt-arena-3d`**；若程式就在儲存庫根目錄則不需設定。
3. Framework Preset 選 Other；Node.js 選 22 或更新版本。
4. Build Command：`node scripts/build-bank.mjs`；Output Directory：`dist`。
5. 不需設定環境變數或 API 金鑰。選 Deploy。
6. 在 Project Settings → Git 確認連接的儲存庫與 Production Branch。之後更新正式分支的 MD 就會重新部署。

`vercel.json` 已提供相同建置設定。若以直接上傳方式部署 Vercel，該次上線本身不代表已經建立 GitHub 自動部署連線；仍須完成上述 Git 匯入／連接。

公開 GitHub 與前端題庫會公開答案；本專案是教學練習遊戲，不是需要保密答案的正式考試。

## 本地執行與檢查

需要 Node.js 22+。

```bash
node scripts/build-bank.mjs
node --test scripts/game.test.mjs
python -m http.server 8000 --directory dist
```

開啟 `http://localhost:8000`。不能直接雙擊 HTML，因為 ES modules 與題庫載入需要 HTTP 服務。

`dist/` 中的 HTML、CSS 與 JS 是實際網站原始碼；`dist/questions.json` 由 Markdown 建置。請改 Markdown 而非 JSON。

## 瀏覽器與素材

需要 WebGL 2 與瀏覽器硬體加速。無法初始化時會顯示提示，不會把 2D 圖片冒充 3D。3D 人物是程式建立的原創模型，參考影片的對戰構圖與玩法，並非影片原模型的複製。

Three.js 使用 MIT 授權，詳見 `dist/vendor/LICENSE-three.txt`。網站字體優先使用 Noto Sans TC，下載失敗時使用系統中文字體；遊戲與 3D 不依賴字體下載。

教育參考：
- https://dayofai.org/curriculum-resources
- https://dayofai.org/units/ai-foundations-grades-k-2-ages-5-7
- https://experience-ai.org/en/units

上述資源用於課程方向參考；本題庫為本專案編寫，不宣稱是上述機構官方題庫。
