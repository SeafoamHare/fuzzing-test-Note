# Fuzzing 技術：進程內與進程外

本文將解釋兩種常見的Fuzzing技術：**進程內 Fuzzing** 和 **進程外 Fuzzing**。這兩種方法都用於通過自動輸入隨機、異常或格式錯誤的數據來發現軟件中的漏洞，但它們在Fuzzing引擎如何與目標應用程序互動上有所不同。

## 1. 進程內 Fuzzing

### 概述
在Google的安全博客文章 [Guided in-process fuzzing of Chrome components](https://security.googleblog.com/2016/08/guided-in-process-fuzzing-of-chrome.html) 中提到：

> By in-process, we mean that we don’t launch a new process for every test case, and that we mutate inputs directly in memory.

**進程內 Fuzzing** 是指Fuzzer在與目標應用程序相同的進程中運行。Fuzzer直接與目標應用程序的內部狀態、內存和變量進行交互，同時執行程序。這種方法通常用於測試那些具有嵌入式API或設計為在同一進程空間中運行的應用程序。

### 優點
- **效率高**：由於Fuzzer與目標應用程序位於相同的進程中，進程間通信（IPC）開銷最小。
- **精細控制**：Fuzzer可以輕鬆訪問並操控目標應用程序的內部狀態，實現更精確的測試。

### 缺點
- **風險較高**：如果目標應用程序崩潰或出現錯誤，Fuzzer的進程也可能受到影響，從而中止整個Fuzzing過程。
- **錯誤處理**：目標應用程序需要具備更複雜的錯誤處理和保護機制，確保即使目標應用程序崩潰，Fuzzer也能繼續運行。

### 使用場景
- 測試需要直接與內存或內部狀態交互的嵌入式軟件或庫。
- 在性能要求高、需要最小開銷的情況下進行測試。

---

## 2. 進程外 Fuzzing

### 概述
在 **進程外 Fuzzing** 中，Fuzzer與目標應用程序運行在不同的進程中。Fuzzer生成輸入並通過進程間通信（IPC）機制，如套接字、管道或文件，將數據發送到目標應用程序。目標應用程序在自己的進程中運行，而Fuzzer則監控目標應用程序的行為。

### 優點
- **隔離性**：Fuzzer和目標應用程序運行在不同的進程中，因此即使目標應用程序崩潰，Fuzzer也不會受到影響，能夠繼續運行。
- **穩定性**：由於進程間的隔離性，Fuzzer的環境不容易受到目標應用程序崩潰或錯誤的影響。

### 缺點
- **性能開銷**：進程間通信需要一定的性能開銷，因為不同進程之間的通信通常比同一進程內的交互要慢。
- **配置複雜性**：設置和管理進程間通信可能會增加Fuzzing設置的複雜度。

### 使用場景
- 測試對穩定性和隔離性要求較高的系統，如大規模生產應用程序或多組件系統。
- 測試需要運行在分離進程環境中的網絡應用程序或系統，以確保安全性。

---

## 進程內與進程外 Fuzzing 的比較

| 特性                      | 進程內 Fuzzing                    | 進程外 Fuzzing                    |
|---------------------------|----------------------------------|----------------------------------|
| **性能**                  | 高，通信開銷小                    | 低，由於IPC開銷較大               |
| **隔離性**                | 低，Fuzzer與目標在同一進程中運行  | 高，Fuzzer與目標在不同進程中運行  |
| **中斷風險**              | 高，目標應用崩潰會影響Fuzzer      | 低，目標應用崩潰不會影響Fuzzer    |
| **設置複雜度**            | 低，與應用程序直接交互            | 高，需要設置和管理IPC            |
| **適用場景**              | 在低開銷環境中進行精細測試        | 在隔離環境中進行穩定測試          |

---

## 結論

**進程內 Fuzzing** 和 **進程外 Fuzzing** 各有優缺點。選擇使用哪一種方法取決於被測試應用程序的具體需求、所需的隔離程度以及Fuzzing過程中的性能限制。

- **進程內 Fuzzing** 更適合那些對速度和直接內存訪問有較高要求的應用程序。
- **進程外 Fuzzing** 更適合對穩定性、隔離性和容錯性有較高要求的應用程序。

通過了解這些技術，您可以根據需求選擇最合適的Fuzzing方法，有效地發現潛在的漏洞。

