# 軟體工程期中報告 黃靖雅
AI 實用百寶箱 (AI Toolbox)
==============================

一個使用 Python 製作的多功能 AI 工具箱，整合了 Prompt 管理、文字生成、向量搜尋、API、CLI、與 GUI。
適合開發者、研究者快速建立屬於自己的 AI 助手或文字應用系統。

------------------------------------------------------------
【功能特色】
------------------------------------------------------------
1. Prompt 管理：統一管理提示模板 (JSON 格式)
2. 向量檢索：使用 sentence-transformers + FAISS
3. 模型推論：Hugging Face transformers pipeline
4. REST API：FastAPI 建立本地或雲端服務
5. CLI 工具：Typer 命令列執行
6. GUI 介面：Streamlit 可視化操作
7. Docker 支援：可容器化部署

------------------------------------------------------------
【專案結構】
------------------------------------------------------------
ai-toolbox/
├─ README.txt
├─ requirements.txt
├─ Dockerfile
├─ app/
│  ├─ utils.py               # 公用工具（logging, timer）
│  ├─ prompt_manager.py      # Prompt 模板管理
│  ├─ embed_search.py        # 向量生成與檢索
│  ├─ hf_infer.py            # 模型推論封裝
│  ├─ serve_api.py           # FastAPI 服務
│  ├─ cli.py                 # Typer CLI
│  └─ streamlit_app.py       # Streamlit GUI
└─ models/                   # (可選) 模型與索引資料

------------------------------------------------------------
【安裝與執行】
------------------------------------------------------------
1. 建立虛擬環境並安裝依賴：
   python -m venv venv
   source venv/bin/activate   (Windows: venv\Scripts\activate)
   pip install -r requirements.txt

2. 建立 Embedding 索引：
   from app.embed_search import EmbedSearch
   texts = ["這是第一段", "這是第二段"]
   es = EmbedSearch()
   es.build(texts)

3. 啟動 API 服務：
   uvicorn app.serve_api:app --reload
   (在瀏覽器開啟 http://127.0.0.1:8000/docs)

4. 使用 CLI：
   python -m app.cli gen --prompt_text "你好"
   python -m app.cli search --query "早安"

5. 啟動 GUI：
   streamlit run app/streamlit_app.py

------------------------------------------------------------
【Docker 部署】
------------------------------------------------------------
Dockerfile 範例：

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . /app
CMD ["uvicorn", "app.serve_api:app", "--host", "0.0.0.0", "--port", "8000"]

執行：
   docker build -t ai-toolbox .
   docker run -p 8000:8000 ai-toolbox

------------------------------------------------------------
【延伸應用】
------------------------------------------------------------
- 對話機器人：整合上下文與記憶模組
- 文件問答 (RAG)：結合 Embedding 檢索與 LLM 生成
- 自動摘要、改寫、分類
- Prompt 儲存與版本控制 (Git)
- Streamlit Dashboard 監控生成次數與耗時

------------------------------------------------------------
【安全與維護建議】
------------------------------------------------------------
- 使用 .env 儲存 API 金鑰與機密
- 防止 prompt injection 攻擊
- Logging 與錯誤追蹤
- Prompt 檔版本化管理

------------------------------------------------------------
【未來方向】
------------------------------------------------------------
- 支援 OpenAI / Claude API
- 整合 Chroma / Milvus 向量資料庫
- 加入 Prompt Lab 可視化管理介面
- 增強 GPU 模型部署效能
