# 進程間通信（IPC）

進程間通信（Inter-process Communication, 簡稱 IPC）是一種讓不同進程（通常是不同應用或程序）之間進行數據交換或訊息傳遞的技術。在操作系統中，進程是執行中的程序，而不同進程之間的通信對於實現多任務處理、分佈式應用等是至關重要的。

## 什麼是進程？
在操作系統中，**進程**是指一個正在執行的程序。每個程序啟動時，操作系統會為它分配一個進程ID，並將程序載入內存中來執行。

## 進程間通信（IPC）
進程間通信（IPC）是指不同的進程之間如何互相交換數據或發送訊息。由於操作系統會將每個程序隔離運行，因此它們無法直接訪問彼此的記憶體，這時就需要一種方式來讓進程之間進行有效的通信。

### 常見的IPC方式：
1. **管道（Pipes）**：一個進程將數據寫入管道，另一個進程從管道中讀取數據。常見於單一機器中的進程間通信。
2. **消息隊列（Message Queues）**：進程間通過消息進行通信，支持異步的數據傳遞。
3. **共享內存（Shared Memory）**：多個進程共享一塊內存區域，可以高效地交換數據。
4. **信號量（Semaphores）**：用於同步和控制進程對共享資源的訪問，防止數據競爭。
5. **套接字（Sockets）**：支持進程間在不同計算機之間進行通信，通常用於網絡通信。
6. **信號（Signals）**：進程間通過發送信號來通知或控制其他進程。
7. **文件映射（Memory-Mapped Files）**：進程可以將文件映射到內存，並共享文件中的數據。

## 為什麼IPC很重要？
進程間通信對於實現以下幾個方面至關重要：

- **多任務處理**：即同時執行多個程序。比如在多任務環境中，程序A可能需要向程序B傳遞數據，這時就需要用到IPC。
- **分佈式應用**：即多個程序或服務分布在不同的計算機上協同工作。IPC 能夠幫助這些分佈式系統中的進程進行數據交換和協調。

## 總結
進程間通信（IPC）是一種讓不同進程之間相互交換數據和訊息的技術。常見的IPC方式包括管道、消息隊列、共享內存、信號量和套接字等。進程間通信在多任務處理和分佈式應用中發揮著關鍵作用，能夠促進進程間的協作和數據共享。
