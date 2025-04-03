# Ollama 簡介 🤖

* [Youtube Tutorial - Ollama 簡介(等待新增)]()

![alt tag](https://ollama.com/public/blog/embedding-models.png)

Ollama 可以簡單理解為 AI 界的 Docker。

下載方式 💾 , 這邊是 linux

[https://ollama.com/download](https://ollama.com/download)

如果你安裝完, 它發現你的電腦沒有 GPU, 會顯示以下的內容提醒你, 但還是可以使用 ⚠️⚠️

```text
WARNING: No NVIDIA/AMD GPU detected. Ollama will run in CPU-only mode.
```

查看你的版本

```cmd
> ollama -v
ollama version is 0.6.2
```

Ollama 預設使用的埠號 (port) 是 `11434` 🔌

## 什麼是 GGUF 格式？ 📄

在 Ollama 中下載和執行的模型，通常都是 GGUF 格式。

GGUF (GPT-Generated Unified Format) 是一種特別為大型語言模型設計的檔案格式。

把它想像成一個 AI 模型的「壓縮檔」或「容器」：它把運行模型所需的所有東西（模型的參數、結構、分詞器資訊等）打包在一個單一檔案裡。

主要優點：讓模型更容易、更快速地在各種不同的硬體上載入和執行，特別是在一般的 CPU 或不同品牌的 GPU（NVIDIA, AMD, Apple Silicon 等）上，

不需要複雜的設定。它的設計優化了載入速度和記憶體使用效率。

與 Ollama 的關係：Ollama 主要就是使用 GGUF 格式的模型。當你執行 ollama pull 時，下載回來的通常就是一個 .gguf 檔案。

## 安裝模型以及把玩 Ollama 🚀

如何下載 models

例如我想要下載 deepseek-r1

![alt tag](https://i.imgur.com/qHo6Sik.png)

這邊我們先 pull 就好

```cmd
ollama pull deepseek-r1:1.5b
```

然後可以使用 `ollama list` 查看目前的所有以下載的模型

```cmd
> ollama list
NAME                       ID              SIZE      MODIFIED
deepseek-coder:latest      3ddd2d3fc8d2    776 MB    2 weeks ago
nomic-embed-text:latest    0a109f422b47    274 MB    2 weeks ago
deepseek-r1:1.5b           a42b25d8c10a    1.1 GB    2 weeks ago
phi3:latest                4f2222927938    2.2 GB    2 weeks ago
```

如果你想試玩某個模型, 可以使用 run ▶️

```cmd
> ollama run deepseek-coder:latest
```

![alt tag](https://i.imgur.com/UsLpnT0.png)

為了讓其他工具（例如 Dify）可以連接到 Ollama，建議設定 OLLAMA_HOST 環境變數，

讓 Ollama 服務監聽所有網路介面。

這邊也有大家常見的一些問題

[How do I configure Ollama server?](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server)

依照你對應的 os 設定, 像我這邊是 linux

Setting environment variables on Linux

```cmd
sudo systemctl edit ollama.service
```

然後加入

```cmd
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
```

最後重新啟動

```cmd
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

## Python 連接 Ollama

使用這個套件 [ollama-python](https://github.com/ollama/ollama-python)

```cmd
pip install ollama
```

```python
from ollama import chat
from ollama import ChatResponse

response: ChatResponse = chat(model='deepseek-r1:1.5b', messages=[
  {
    'role': 'user',
    'content': '什麼是 docker',
  },
])
print(response['message']['content'])
# or access fields directly from the response object
print(response.message.content)
```

![alt tag](https://i.imgur.com/VngQmcA.png)