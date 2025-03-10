**貳、研究過程**

本專題旨在設計一套自動跟隨購物車系統，透過多模組整合提升購物過程中的便利性與安全性。系統採用模組化設計，各功能模組間協同運作，並由 Raspberry Pi 5 作為整個系統的控制核心。以下為本研究的主要研發步驟及各模組的搭配與程式運用說明：

### 1. 系統規劃與硬體平台搭建  
- **中央控制單元**：選用 Raspberry Pi 5 8GB，提供足夠的運算能力與豐富的 I/O 接口。搭配 IO Expansion HAT（驅動主控：STM32），進一步擴充 Raspberry Pi 的 I/O 接口，方便連接多個外部模組。  
- **連接方式**：所有模組皆透過杜邦線與 IO Expansion HAT 接口連接，確保電路整潔、接線標準化。

### 2. 視覺辨識與深度感測模組  
- **Orbbec Astra Pro Plus 3D 深度攝像頭**：利用深度攝像頭不僅取得高品質的 RGB 影像，還可獲得每個像素的深度資訊。  
  - **程式運用**：採用 OpenCV 與相關深度圖處理庫，進行目標追蹤與距離計算，從而實現自動跟隨功能。  
  - **搭配理念**：深度資訊可幫助購物車精確測量與使用者間的距離，進而實現智能避障與距離維持。

### 3. 運動控制模組  
- **萬向輪 + 直流馬達 (4組)**：提供全方位移動能力，使購物車能夠實現前進、後退、轉向與原地旋轉等操作。  
- **L298N 馬達驅動模組 (2組)**：利用 L298N 雙H橋驅動 IC 分別控制左右兩組直流馬達，並透過 PWM 技術調整轉速與方向。  
  - **程式運用**：採用 Python 與 RPi.GPIO 庫控制 GPIO 輸出訊號，實現對馬達驅動的精確調控。  
  - **搭配理念**：直流馬達與 L298N 驅動模組的組合能提供穩定的動力輸出，保證購物車在不同路徑下的流暢運動。

### 4. 定位與姿態感測  
- **ATGM336H-5N GPS 模組**：提供實時定位資訊，幫助記錄購物車運行軌跡，並在大範圍環境下輔助導航。  
- **GY-61 ADXL335 三軸重力加速度感測器**：用於監測購物車的傾斜狀態與運動加速度，確保系統在運動過程中的穩定性。  
  - **程式運用**：透過 I2C 通訊協定讀取感測數據，並利用 Python 進行數據校正與即時監控。  
  - **搭配理念**：GPS 與加速度感測器的結合可實現對購物車運動狀態的全方位監測，避免因路面不平或急轉彎而導致系統失控。

### 5. 語音輸出與人機互動  
- **Gravity: Digital Speaker Module**：採用高保真 8002 功放晶片，實現語音提示與人機互動功能，如當購物車需要避障或轉彎時給予語音提示。  
  - **程式運用**：透過 Raspberry Pi 的音頻輸出接口驅動該模組，結合預先錄製的提示音檔，達到語音輸出的效果。  
  - **搭配理念**：語音提示能讓使用者直觀地了解購物車的狀態與操作指令，增強整體使用體驗。

### 6. 其他控制元件  
- **四路繼電器**：可用於控制其他高功率模組或外接設備（例如燈光、額外驅動模組），增加系統的擴充性。  
- **2P2段迷你開關**：作為手動切換開關，可在系統異常或需手動介入時，切換不同操作模式。  
  - **程式運用**：透過 IO Expansion HAT 將迷你開關狀態讀入 Raspberry Pi，根據狀態觸發不同控制程序。  
  - **搭配理念**：手動切換功能可以在自動模式之外提供備援方案，確保系統在特殊狀況下仍能穩定運作。

### 7. 程式設計與整合  
- **程式語言**：以 Python 為主要開發語言，利用其豐富的庫（例如 OpenCV、RPi.GPIO、pySerial 等）實現各模組之間的協同運作。  
- **模組整合**：各功能模組分別負責視覺辨識、運動控制、定位監控與人機互動，通過中心控制器 Raspberry Pi 5 進行整合與指令調度。  
- **數據通訊**：通過 I2C、UART 與 GPIO 等接口實現與各外部模組的數據交互，同時利用物聯網技術實現資料的遠端傳輸與存儲。

### 8. 測試與調試  
在初步搭建完硬體平台與程式系統後，我們進行了多次實地測試：
- **視覺追蹤測試**：調整 Orbbec 深度攝像頭參數，確保在各種光線條件下均能準確獲取使用者位置與距離數據。  
- **運動控制測試**：通過調整 L298N 驅動模組與 PWM 控制參數，確保購物車在不同路徑下均能流暢運動，並在遇到障礙時自動進行避障處理。  
- **綜合測試**：將所有模組整合在一起進行系統測試，觀察各模組協同工作時的穩定性與反應速度，並根據測試結果不斷調整與優化程式邏輯與硬體佈線。

通過這一系列的研發過程，我們不僅掌握了各模組之間的協同運作原理，還深入理解了如何透過軟體與硬體整合來實現智能跟隨、運動控制與安全監測等核心功能。這些寶貴的經驗將為未來的系統擴充與技術創新提供堅實的基礎。

**各模組特點說明**

1. **中央控制單元 – Raspberry Pi 5 8G**  
   - **高效能運算**：搭載 8G 記憶體，提供足夠的運算資源以處理視覺辨識、運動控制及資料傳輸等多重任務。  
   - **豐富接口**：具有多個 USB、GPIO 以及網路接口，可輕鬆與各種外部模組進行連接。  
   - **模組化擴充**：結合 IO Expansion HAT（驅動主控：STM32），進一步擴展 I/O 端口，方便整合其他感測器與驅動元件。

2. **視覺辨識與深度感測模組 – Orbbec Astra Pro Plus 3D 深度攝像頭**  
   - **深度處理**：搭載 MX6000 深度處理晶片，能夠提供高精度的深度資訊，有效輔助目標追蹤與避障。  
   - **高品質影像**：同時輸出 RGB 影像，使視覺辨識系統能夠進行更精確的物件識別。  
   - **即時數據**：深度與彩色影像數據同步輸出，便於進行綜合運算與實時控制。

3. **運動控制模組 – 萬向輪、直流馬達與 L298N 雙H橋驅動模組**  
   - **全向運動能力**：萬向輪配合四組直流馬達能夠實現全方位移動、精確轉向與原地旋轉，滿足各種複雜路徑的運動需求。  
   - **穩定驅動**：L298N 驅動模組採用雙H橋驅動技術，搭配 PWM 調速，使馬達運行平穩且具有較高的反應速度。  
   - **擴展性高**：兩組 L298N 模組可分別控制左右兩側的馬達，具備獨立調控能力，便於細緻調整運動策略。

4. **定位與姿態感測模組**  
   - **GPS 定位**：ATGM336H-5N GPS 模組提供實時定位資訊，適用於開放環境下的路徑追蹤與定位記錄。  
   - **姿態監控**：GY-61 ADXL335 三軸加速度感測器能夠監測購物車的傾斜與震動狀態，確保運動穩定性，並防止因路面不平導致的系統失控。

5. **語音與人機互動模組 – Gravity: Digital Speaker Module**  
   - **高保真音效**：採用高保真 8002 功放晶片，提供清晰、響亮的語音提示，增強使用者與系統之間的互動。  
   - **實時反饋**：結合預錄音檔或即時生成語音，能在系統操作（如轉向、避障）時給予直觀的聲音反饋。

6. **其他控制與介面元件**  
   - **四路繼電器**：用於控制高功率設備或外部設備，提供額外的系統擴充功能。  
   - **2P2段迷你開關**：具備兩個觸點與兩個位置，作為手動介入或模式切換的備援裝置，方便使用者在特殊狀況下切換操作模式。  
   - **連接介面**：所有模組皆採用杜邦線與 IO Expansion HAT 接口進行連接，確保佈線整齊且易於維護與更換。
