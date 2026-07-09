---
title: "Ollama+ MCP"
---
太棒了！您注意到了台灣神學研究的最強神器——信望愛聖經資源站的 MCP（Model Context Protocol）服務。透過 `FHL_MCP_SERVER`，您的本地 AI 就不再只是瞎猜，而是可以直接透過 API 連線到信望愛資料庫，即時查詢精確的聖經節數、原文字典（Strong's Number）、註釋書與原文語法分析。 [1, 2, 3]

要讓 Ollama 本地模型使用信望愛 MCP，必須透過一個支援 MCP 協定的「客戶端（Client）」外殼。由於 AnythingLLM 對 MCP 的原生支援還在發展中，目前技術上最成熟、最推薦您的高效玩法是使用 Claude Desktop 或 LobeChat 作為介面，後台串接您的 Ollama 模型 與 信望愛 MCP。 [3, 4, 5]

以下為您整理在 Windows 電腦上的完整設定指南：

---

## 第一步：環境準備（安裝 Node.js）

信望愛的 MCP 伺服器是以 Node.js 執行。您的電腦需要先有運行環境： [3]

1. 前往 [Node.js 官網](https://nodejs.org/) 下載並安裝 LTS（長期維護）版本。
2. 安裝時一路按 Next（下一步）直到完成即可。

---

## 第二步：下載並確保 Ollama 執行中

請確保您的 Ollama 已經下載了對應的模型，並在背景執行： [6]

- 打開命令提示字元（CMD），輸入指令下載 Qwen 模型：
    
    ```bash
    ollama run qwen2.5:14b
    ```
    
    _(註：Qwen 2.5 具備極佳的工具調用 Tool Calling 能力，非常適合驅動 MCP 工具)。_ [4]

---

## 第三步：配置 MCP 設定檔

以目前社群最普及的 Claude Desktop 介面為例，我們要建立一座橋樑，讓介面同時讀到「本地 Ollama」與「線上信望愛 MCP」： [4, 5]

1. 按下鍵盤的 `Win + R` 鍵，輸入 `%APPDATA%\Claude` 並按下 Enter。
2. 在打開的資料夾中，檢查有沒有一個叫做 `claude_desktop_config.json` 的檔案。如果沒有，請按右鍵新增一個文字檔案，並將檔名連同副檔名改為 `claude_desktop_config.json`。
3. 用記事本打開它，把以下內容完全複製並貼上去（感謝 GitHub 作者 [ytssamuel/FHL_MCP_SERVER](https://github.com/ytssamuel/FHL_MCP_SERVER) 提供的開源專案）： [1]

```json
{
  "mcpServers": {
    "fhl-bible": {
      "command": "npx",
      "args": [
        "-y",
        "@ytssamuel/fhl-mcp-server"
      ]
    }
  }
}
```

_(如果是使用 LobeChat，直接在系統設定的「MCP 插件」中新增，並貼上 `npx -y @ytssamuel/fhl-mcp-server` 即可)。_ [3]

---

## 第四步：啟動與神學問答測試

重啟您的對話軟體（如 Claude Desktop 或是設定好對接 Ollama 的 LobeChat 介面），如果配置成功，對話框輸入處會出現一個「小樂高積木 / 插入符號」，代表信望愛工具已連線。 [5, 7, 8]

此時您可以直接對著使用本地 Qwen 模型的 AI 進行以下格式的專業提問： [4]

- 經文查詢：「請幫我查出約翰福音 3 章 16 節的經文，並對照和合本與新譯本。」
- 原文神學分析：「『起初神創造天地』中的『神（Elohim）』，在信望愛原文字典中的 Strong's Number 是多少？請分析它的文法與字根意義。」
- 主題查經：「請幫我搜尋聖經中關於『因信稱義』的相關經文與信望愛註釋書摘要。」

此時 AI 會觸發 MCP 協定，主動發送請求給信望愛 API，抓回最精準、毫無幻覺的聖經正文與原文字典，再交由您本地的 Ollama 模型進行精美的組織與神學論述。 [1, 4]

這個設定牽涉到設定檔的編輯。請問您在建立 json 設定檔的步驟是否順利？還是需要我詳細說明如何在 LobeChat 這種圖形化網頁介面中直接貼上信望愛 MCP 連結呢？

  

[1] [https://github.com](https://github.com/ytssamuel/FHL_MCP_SERVER)

[2] [https://bible.fhl.net](https://bible.fhl.net/)

[3] [https://lobehub.com](https://lobehub.com/mcp/yourusername-fhl-bible-mcp?activeTab=deployment)

[4] [https://zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/1907220845841069807)

[5] [https://mcp.aibase.com](https://mcp.aibase.com/tw/server/1916343927202095105)

[6] [https://www.cg.com.tw](https://www.cg.com.tw/Ollama/)

[7] [https://medium.com](https://medium.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/ai-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-%E4%BB%80%E9%BA%BC%E6%98%AF-mcp-server-1388ffa09aa9)

[8] [https://www.bnext.com.tw](https://www.bnext.com.tw/article/82706/what-is-mcp)