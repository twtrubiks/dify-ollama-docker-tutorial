# Dify 安裝與連接 Ollama 設定指南 (Docker Compose) 🚀

* [Youtube Tutorial - Dify 安裝與連接 Ollama 設定指南 (Docker Compose)](https://youtu.be/4oa3g1p1DjA)

## Dify、Ollama 與模型關係 🤖

```plaintext
+------------+    +------------------+      +-------------------------+
|  使用者     | --> |  Dify 應用介面   | -->  |  Ollama 本地伺服器 (API)  |
+------------+    +------------------+      +--------------+----------+
                                                           |
                    +--------------------------------------+----------+
                    |                                                 |
                    v                                                 v
        +--------------------------------+    +-----------------------------------------+
        |  大型語言模型 (LLM)              |    |  Embedding 模型                          |
        |  (例: deepseek-coder)          |    |  (例: nomic-embed-text, 文本向量化、RAG)   |
        +--------------------------------+    +-----------------------------------------+
                    |                                                 |
                    +-------------------------------------------------+
                                            |
                                            v
                            +-----------------------------+
                            |       Dify 處理請求與回應     |
                            +-----------------------------+
                                            |
                                            v
                                        (返回給使用者)
```

### 什麼是 RAG 技術？ 📚

RAG 是「檢索增強生成 (Retrieval-Augmented Generation)」的縮寫。你可以把它想像成讓 AI 模型在回答問題或生成內容前，先去「查資料」的技術。

簡單來說，它的運作方式像這樣：

提問與檢索(Retrieval) 🔍： 當你問一個問題時，系統不是馬上讓 AI 直接回答。它會先拿你的問題，去一個指定的資料庫（例如你上傳的文件、產品手冊、公司知識庫等）裡面，找出最相關的幾段資訊。這一步通常會用到 Embedding 模型 來理解問題和資料內容的語意相似度。

增強與生成(Augmented Generation)： 系統把你的原始問題，加上剛剛從資料庫裡找到的相關資訊，一起交給 大型語言模型 (LLM)。

回答 💡： LLM 就像一個聰明的學生，它參考了這些「開書考」的資料，再加上自己原本的知識，生成一個更準確、更貼合特定知識範圍的答案給你。

#### 為什麼要用 RAG？

提高準確性 🎯 ： 讓 AI 的回答基於最新的、特定的、或是你私有的資料，而不是僅僅依賴它訓練時學到的舊知識，減少「胡說八道」(幻覺) 的情況。

知識更新 🔄： 不需要重新訓練龐大的 LLM，只要更新外部的資料庫，就能讓 AI 掌握新資訊。

可追溯性 🔎： 因為答案是基於檢索到的資料生成的，更容易知道 AI 回答的依據來源。

在 Dify 這類平台中，你可以輕易地建立 RAG 應用，例如上傳公司文件，製作一個能回答內部問題的客服機器人，而背後的模型運算就可以交由像 Ollama 這樣的本地服務來處理。

## 安裝 Dify (Docker Compose)

Dify 下載方式, 這邊是 linux 🐧

[Deploy with Docker Compose](https://docs.dify.ai/getting-started/install-self-hosted/docker-compose)

這邊安裝很簡單, 直接複製貼上即可

```cmd
# 這邊我們用最新的版本, 寫文章時是 1.1.3
git clone https://github.com/langgenius/dify.git

cd dify/docker

# 複製一份 env
cp .env.example .env

docker compose up -d
```

第一次要 pull image, 會比較久 🕐

之後就可以瀏覽你的本機 [http://0.0.0.0/](http://0.0.0.0/)

### 設定 Ollama 模型 ⚙️

然後開啟設定 model, 這邊我們用 ollama

![alt tag](https://i.imgur.com/3KgSESt.png)

目前本機 ollama 有這些模型,

![alt tag](https://i.imgur.com/S0V302w.png)

Model 使用 `deepseek-r1:1.5b`

網址這邊注意一下, 上一隻影片有提醒大家要

[How do I configure Ollama server?](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server)

這邊我是填入本機的 server, 也就是 `http://192.168.85.196:11434`

如果你連不上去, 也可以嘗試 `http://host.docker.internal:11434`
(但這個 docker compose 需要調整, 我自己是沒設定成功)

如下

![alt tag](https://i.imgur.com/N6iv2l5.png)

只要你保存成功, 就是沒問題的 ✅

如果有任何錯誤, 會出現, 通常問題是沒設定 Ollama server 請參考上面

![alt tag](https://i.imgur.com/aphPTOa.png)

```text
An error occurred during credentials validation: HTTPConnectionPool(host='host.docker.internal', port=11434): Max retries exceeded with url: /api/chat (Caused by NameResolutionError("<urllib3.connection.HTTPConnection object at 0x70f903838ef0>: Failed to resolve 'host.docker.internal' ([Errno -2] Name or service not known)"))
```

Text Embedding 的部份我們也設定

Model 使用 `nomic-embed-text:latest`

網址一樣是 `http://192.168.85.196:11434`

![alt tag](https://i.imgur.com/7t3iUIe.png)
