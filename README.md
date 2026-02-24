# 馬鈴薯發芽辨識

### 開發環境：

Python 3.9 , TensorFlow 2.10 , OpenCV, NumPy, Roboflow, Ngrok API, Line Bot

### 專案成果：

1. 資料工程：自行拍攝與搜集網路圖片，建立並標記 1,009 張訓練資料集 (478張自攝 + 531張網路) 。因版權與檔案大小考量，未包含在此儲存庫中。**但專案內的 `data` 資料夾附有 80 張已縮放之微型資料集，供快速驗證程式碼使用。**
2. 模型訓練：使用 TensorFlow 進行二階段模型訓練，使用預訓練模型與調整超參數，使模型的準確率達到 96%。
3. 結合 Roboflow 的物件偵測功能，讓模型透過攝影機實現即時偵測功能。
4. 透過 Ngrok 將訓練好的模型部署 Web 服務，並串接 Line Bot API，做出使用者可上傳圖片即時辨識的聊天機器人。



![image](./media/potato-1.png)

![image](./media/potato_LineBot.jpg)

### 專案目錄結構

```text
├── data/                    # 範例資料集 (共 80 張已縮放之影像，供快速測試)
│   ├── train/               # 訓練集 (64 張)
│   │   ├── sprouted/        # 發芽的馬鈴薯圖片
│   │   └── not_sprouted/    # 未發芽的馬鈴薯圖片
│   └── validation/          # 驗證集 (16 張)
│       ├── sprouted/        
│       └── not_sprouted/
├── media/                   # 存放 README 顯示用的圖片與 Demo
├── model_training.ipynb     # 模型訓練流程、超參數調整與驗證
├── PytorchGputest.ipynb     # 測試 GPU 是否讀取
├── roboflow_camera.py       # 透過本機攝影機進行即時物件偵測的腳本
├── linebot.ipynb            # Line Bot 後端邏輯處理與 Ngrok 串接測試
├── requirements.txt         # 專案執行所需的 Python 套件清單
└── README.md                # 專案說明文件
```
### 快速開始
#### 1. 環境安裝
請先將本專案 Clone 到本機，並建議建立一個虛擬環境 (Virtual Environment) 來安裝相依套件：
```Bash
git clone https://github.com/CindyTu405/potato_sprout_detector.git
cd potato_sprout_detector
pip install -r requirements.txt
```

#### 2. 環境變數與金鑰設定 (Configuration)
若要順利執行攝影機即時偵測與 Line Bot 功能，你需要準備以下 API Keys，在根目錄建立.env，
並替換程式碼中的對應欄位：
```
# ngrok
NGROK_AUTHTOKEN = ''

# Line Basic settings -> Channel secret
LINE_CHANNEL_SECRET = ''
# Line Messaging API -> Channel access token
LINE_CHANNEL_ACCESS_TOKEN = ''

ROBOFLOW_API_KEY = ''

LINE_LIFF_ID = ''
```

#### 3. 執行專案
**選項 A：本機攝影機即時偵測**
確認已接上 WebCam 並設定好 Roboflow API Key 後，於終端機執行：
```bash
python roboflow_camera.py
```

**選項 B：啟動 Line Bot 服務**
1. 開啟並依序執行 linebot.ipynb 內的儲存格。
2. 程式啟動後，會自動透過 Ngrok 建立通道，請在終端機 (或 Jupyter 輸出區) 找到印出的公開網址 (Webhook URL)。
3. 將該 Ngrok 網址填入 Line Developers 後台的 Webhook URL (記得補上 /callback 路由)。
4. 加入 Line Bot 好友，即可上傳照片進行測試！