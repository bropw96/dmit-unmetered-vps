# 無限流量VPS：把流量額度、埠速、網路路由與公平使用條款一次看懂

很多人搜尋「無限流量VPS」，真正想找的其實不是一句「Unlimited」而已，而是：**每月到底能跑多少資料、跑到多少速度、超過額度之後會發生什麼、長時間高流量會不會被限速，以及哪個機房真的適合自己的使用情境。**

這也是為什麼只看 VPS 頁面上的「不限流量」很容易買錯。近期幾篇專門整理 unmetered / unlimited bandwidth VPS 的比較文章，反覆把 **流量額度、埠速、fair-use／AUP、超額計費與實際使用場景**列為核心判斷條件；有些服務寫著 unmetered，真正限制其實藏在埠速或長時間持續使用條款裡。

DMIT 剛好是一個很典型的例子：**目前官方公開的 Cloud Instance Pricing 並不是「每一台 VPS 都完全不限流量」的產品線，而是大量採固定 Transfer quota，另外部分 Tier 1 方案以 `Max (IN, OUT)` 標示傳輸上限。** 官方 AUP 同時寫明，長時間持續大量佔用頻寬是不允許的，DMIT 也保留因影響整體網路穩定而限制 VM 網路埠的權利。

所以，下面先把「無限」這個詞拆開，再看 DMIT 現在實際有哪些選項。

## 「無限流量」到底要看什麼？

先分清楚三個不同概念。

**流量額度（Transfer）**是某個計費週期內可以傳輸多少資料，例如 1,000GB、5,000GB、32,000GB。

**連線埠速（Port Speed）**則是資料傳輸速度，例如 1Gbps、4Gbps、10Gbps。流量不計費，不等於可以無限速傳輸。

第三個是**公平使用與服務條款**。即使某個產品寫著 unmetered，服務商仍可能限制長時間持續跑滿頻寬的工作負載。DMIT 現行 AUP 就明確禁止長時間持續佔用大量頻寬，並保留在活動影響網路穩定時限制 VM Internet Port 的權利。

這三件事最好一起看。

舉個簡單例子：每月 10TB、10Gbps 的 VPS，和「unmetered、但只有 100Mbps」的 VPS，兩者都可能被搜尋結果歸到「不限流量 VPS」這個類別裡，但實際用途完全不同。

因此，買之前至少要回答四個問題：

1. **流量是每月固定額度，還是 unmetered？**

2. **流量是 IN/OUT 分開計，還是雙向總量？**

3. **埠速是多少？**

4. **AUP 對長時間高流量有沒有額外限制？**

這也是近期「unmetered VPS」比較內容最常提醒讀者的地方。

## DMIT 現在是不是「真正的無限流量 VPS」？

如果「無限流量」的標準是**完全沒有月度 Transfer 上限，而且可以長時間滿速傳輸**，那麼目前 DMIT 公開 Pricing Page 上的大部分 Cloud Instance 並不符合這個定義。

例如 LAX 的 AS3 Premium 系列，TINY 是 1,000GB/月、Pocket 是 1,500GB、STARTER 是 3,000GB、MINI 是 5,000GB、MICRO 是 7,000GB、MEDIUM 是 15,000GB，而且官方同時標示不同的 1Gbps、4Gbps 或 10Gbps 埠速。

反過來，LAX、HKG、TYO 的 Tier 1 系列有不少產品採用 `Max (IN, OUT)`，例如 LAX AN5 Tier 1 V12C24G 顯示 **160,000GB Max (IN, OUT)**，HKG/TYO 的 Tier 1 大容量方案則一路上到 **128,000GB Max (IN, OUT)**。這類產品比較接近「超大流量 VPS」，但仍然不是沒有任何上限。

DMIT 以前或不同產品線中確實可以看到「Unmetered」字樣，但這並不能推導成所有 Cloud Instance 都是無限流量。搜尋到的 DMIT 購物車資料中，某些產品同時列出例如 `IN/OUT Max 2000GB @ 4Gbps` 與另一個 `Unmetered @ 200Mbps` 的傳輸描述，更說明「額度」和「速度」是兩個不同維度。

更重要的是，官方 AUP 不是裝飾品。對於需要長時間大量跑資料的用途，條款本身就應該算進成本。

## DMIT 的真正差異，其實是網路路由

DMIT 現在的產品架構很大程度上不是單純比「幾 GB RAM」，而是把機房、硬體平台與 Network Series 拆開。

官方目前把 Network Series 分成 **Premium、Eyeball、Tier 1**。Premium 使用包含 China Telecom CN2 GIA 在內的高品質路由；Eyeball 則採合理努力的中國住宅 ISP 路由；Tier 1 則更偏向亞太、美洲與全球的一般高頻寬、低延遲連線。

對「無限流量VPS」搜尋者來說，這件事尤其重要。

假設你的主要用量是：

* 備份、鏡像、下載、跨區同步

* 大量 API 傳輸

* 大型檔案分發

* CI/CD artifact、容器映像或套件鏡像

* 歐美與亞太之間的大流量交換

那麼 Tier 1 的高傳輸額度未必比 Premium 更差，反而可能比較貼近需求。

但如果 VPS 主要服務中國內地使用者，網路路由的重要性就可能高於「多幾 TB」。DMIT 官方對 Premium Network 的定位，就是以中國內地與亞太的延遲、丟包和路由品質為重點。

### 三條路線怎麼理解？

**Premium**：優先看中國內地與亞太的連線品質，官方使用 CN2 GIA 等 Premium Transit。

**Eyeball**：成本較低，仍針對中國住宅網路做優化，但保證程度低於 Premium。HKG 的 Eyeball 目前仍標為 Beta，官方也提醒網路路由仍在調校，不建議要求高穩定度的正式生產環境使用。

**Tier 1**：更偏全球一般流量、高傳輸與跨區用途，不以中國內地專項路由為主要賣點。LAX 官方尤其把 Tier 1 定位在亞太與美洲之間的高頻寬連線。

---

## 全套餐對比表

下面的表格依目前 DMIT 官方 Pricing Page 與各地 Data Center 頁面整理，包含目前公開展示的 Cloud Instance 配置；其中「暫時缺貨」也保留，因為它仍出現在官方價格頁上。DMIT 官方同時提醒，Pricing 表中的價格與產品可能因調整而存在更新延遲，因此下單前仍應以進入訂單頁看到的實際價格與庫存為準。

> 下面的購買連結均使用本篇提供的 AFF 入口。針對各 SKU 的獨立 deeplink，這次無法從公開資料可靠驗證對應規則，因此不自行編造產品 ID 或參數。

| 地區／系列 | 套餐 | 核心配置 | 流量 | 埠速 | 價格 | 狀態 | 購買 |
| --- | --- | --- | ---: | ---: | ---: | --- | --- |
| LAX AS3 Premium | TINY | 1 vCore / 2GB / 20GB SSD | 1TB | 1Gbps | $10.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Premium | Pocket | 2 vCore / 2GB / 40GB SSD | 1.5TB | 4Gbps | $16.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Premium | STARTER | 2 vCore / 2GB / 80GB SSD | 3TB | 10Gbps | $34.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Premium | MINI | 4 vCore / 4GB / 80GB SSD | 5TB | 10Gbps | $62.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Premium | MICRO | 4 vCore / 4GB / 160GB SSD | 7TB | 10Gbps | $87.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Premium | MEDIUM | 6 vCore / 8GB / 160GB SSD | 15TB | 10Gbps | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Premium | MINI | 4 vCore / 4GB / 80GB SSD | 5TB | 10Gbps | $72.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Premium | MICRO | 4 vCore / 4GB / 160GB SSD | 7TB | 10Gbps | $102.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Premium | MEDIUM | 6 vCore / 8GB / 160GB SSD | 15TB | 10Gbps | $239.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Premium | LARGE | 8 vCore / 16GB / 320GB SSD | 25TB | 10Gbps | $459.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Premium | GIANT | 12 vCore / 24GB / 640GB SSD | 50TB | 10Gbps | $929.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Premium | MINI | 4 vCore / 4GB / 80GB SSD | 5TB | 10Gbps | $79.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Premium | MICRO | 4 vCore / 4GB / 160GB SSD | 7TB | 10Gbps | $110.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Premium | MEDIUM | 6 vCore / 8GB / 160GB SSD | 15TB | 10Gbps | $289.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Premium | LARGE | 8 vCore / 16GB / 320GB SSD | 25TB | 10Gbps | $499.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Premium | GIANT | 12 vCore / 24GB / 640GB SSD | 50TB | 10Gbps | $1,009.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Eyeball | TINY | 1 vCore / 2GB / 20GB SSD | 1.5TB | 2Gbps | $10.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Eyeball | Pocket | 2 vCore / 2GB / 40GB SSD | 3TB | 4Gbps | $16.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Eyeball | STARTER | 2 vCore / 2GB / 80GB SSD | 5TB | 10Gbps | $34.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Eyeball | MINI | 4 vCore / 4GB / 80GB SSD | 10TB | 10Gbps | $62.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Eyeball | MICRO | 4 vCore / 4GB / 160GB SSD | 14TB | 10Gbps | $87.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Eyeball | MEDIUM | 6 vCore / 8GB / 160GB SSD | 30TB | 10Gbps | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Eyeball | MINI | 4 vCore / 4GB / 80GB SSD | 10TB | 10Gbps | $72.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Eyeball | MICRO | 4 vCore / 4GB / 160GB SSD | 14TB | 10Gbps | $102.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Eyeball | MEDIUM | 6 vCore / 8GB / 160GB SSD | 30TB | 10Gbps | $239.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Eyeball | LARGE | 8 vCore / 16GB / 320GB SSD | 50TB | 10Gbps | $459.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN4 Eyeball | GIANT | 12 vCore / 24GB / 640GB SSD | 100TB | 10Gbps | $929.90/月 | 暫時缺貨 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Eyeball | MINI | 4 vCore / 4GB / 80GB SSD | 10TB | 10Gbps | $79.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Eyeball | MICRO | 4 vCore / 4GB / 160GB SSD | 14TB | 10Gbps | $110.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Eyeball | MEDIUM | 6 vCore / 8GB / 160GB SSD | 30TB | 10Gbps | $289.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Eyeball | LARGE | 8 vCore / 16GB / 320GB SSD | 50TB | 10Gbps | $499.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Eyeball | GIANT | 12 vCore / 24GB / 640GB SSD | 100TB | 10Gbps | $1,009.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 Volume | V2C2G | 2 vCore / 2GB / 40GB SSD | 5TB Max IN/OUT | 10Gbps | $14.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 Volume | V2C4G | 2 vCore / 4GB / 80GB SSD | 10TB Max IN/OUT | 10Gbps | $23.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 Volume | V4C4G | 4 vCore / 4GB / 120GB SSD | 20TB Max IN/OUT | 10Gbps | $36.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 Volume | V4C8G | 4 vCore / 8GB / 160GB SSD | 40TB Max IN/OUT | 10Gbps | $52.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 Volume | V8C16G | 8 vCore / 16GB / 240GB SSD | 80TB Max IN/OUT | 10Gbps | $119.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 Volume | V12C24G | 12 vCore / 24GB / 320GB SSD | 160TB Max IN/OUT | 10Gbps | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 General | G2C4G | 2 vCore / 4GB / 80GB SSD | 4TB Max IN/OUT | 10Gbps | $16.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 General | G4C8G | 4 vCore / 8GB / 160GB SSD | 8TB Max IN/OUT | 10Gbps | $36.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 General | G8C16G | 8 vCore / 16GB / 320GB SSD | 12TB Max IN/OUT | 10Gbps | $79.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 General | G12C24G | 12 vCore / 24GB / 480GB SSD | 240TB Max IN/OUT* | 10Gbps | $119.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AN5 Tier 1 General | G16C32G | 16 vCore / 32GB / 640GB SSD | 320TB Max IN/OUT | 10Gbps | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Tier 1 | WEE | 1 vCore / 1GB / 20GB SSD | 1TB Max IN/OUT | — | $36.90/年 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Tier 1 | TINY | 1 vCore / 1GB / 20GB SSD | 2TB Max IN/OUT | — | $6.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Tier 1 | STARTER | 2 vCore / 2GB / 40GB SSD | 4TB Max IN/OUT | — | $12.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Tier 1 | MINI | 2 vCore / 4GB / 80GB SSD | 8TB Max IN/OUT | — | $21.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| LAX AS3 Tier 1 | MICRO | 4 vCore / 4GB / 120GB SSD | 16TB Max IN/OUT | — | $32.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Premium | MINI | 4 vCore / 4GB / 80GB SSD | 1.5TB | 1Gbps | $149.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Premium | MICRO | 4 vCore / 4GB / 160GB SSD | 2TB | 1Gbps | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Premium | MEDIUM | 6 vCore / 8GB / 160GB SSD | 2.5TB | 1Gbps | $279.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Premium | LARGE | 8 vCore / 16GB / 320GB SSD | 3TB | 1Gbps | $359.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Premium | GIANT | 12 vCore / 24GB / 640GB SSD | 6TB | 1Gbps | $759.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Premium | TINY | 1 vCore / 1GB / 20GB SSD | 500GB | 1Gbps | $39.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Premium | STARTER | 1 vCore / 2GB / 40GB SSD | 1TB | 1Gbps | $79.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Premium | MINI | 2 vCore / 4GB / 60GB SSD | 1.5TB | 1Gbps | $126.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Premium | MICRO | 4 vCore / 4GB / 80GB SSD | 2TB | 1Gbps | $179.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Premium | MEDIUM | 4 vCore / 8GB / 160GB SSD | 2.5TB | 1Gbps | $239.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Eyeball | MINI | 4 vCore / 4GB / 80GB SSD | 2.2TB | 1Gbps | $149.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Eyeball | MICRO | 4 vCore / 4GB / 160GB SSD | 3TB | 1Gbps | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Eyeball | MEDIUM | 6 vCore / 8GB / 160GB SSD | 4TB | 1Gbps | $279.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Eyeball | LARGE | 8 vCore / 16GB / 320GB SSD | 4.5TB | 1Gbps | $359.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AN5 Eyeball | GIANT | 12 vCore / 24GB / 640GB SSD | 9TB | 1Gbps | $759.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Eyeball | TINY | 1 vCore / 1GB / 20GB SSD | 800GB | 1Gbps | $39.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Eyeball | STARTER | 1 vCore / 2GB / 40GB SSD | 1.5TB | 1Gbps | $79.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Eyeball | MINI | 2 vCore / 4GB / 60GB SSD | 2.2TB | 1Gbps | $126.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Eyeball | MICRO | 4 vCore / 4GB / 80GB SSD | 3TB | 1Gbps | $179.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Eyeball | MEDIUM | 4 vCore / 8GB / 160GB SSD | 4TB | 1Gbps | $239.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | WEE | 1 vCore / 1GB / 20GB SSD | 1TB Max IN/OUT | — | $36.90/年 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | TINY | 1 vCore / 1GB / 20GB SSD | 2TB Max IN/OUT | — | $6.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | STARTER | 1 vCore / 2GB / 40GB SSD | 4TB Max IN/OUT | — | $12.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | MINI | 2 vCore / 2GB / 60GB SSD | 8TB Max IN/OUT | — | $21.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | MICRO | 4 vCore / 4GB / 80GB SSD | 16TB Max IN/OUT | — | $32.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | MEDIUM | 4 vCore / 8GB / 160GB SSD | 32TB Max IN/OUT | — | $49.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | LARGE | 8 vCore / 16GB / 320GB SSD | 64TB Max IN/OUT | — | $99.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| HKG AS3 Tier 1 | GIANT | 8 vCore / 24GB / 640GB SSD | 128TB Max IN/OUT | — | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | TINY | 1 vCore / 1GB / 20GB SSD | 500GB | 1Gbps | $21.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | STARTER | 1 vCore / 2GB / 40GB SSD | 1TB | 1Gbps | $45.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | MINI | 2 vCore / 4GB / 60GB SSD | 2TB | 1Gbps | $89.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | MICRO | 4 vCore / 4GB / 80GB SSD | 4TB | 1Gbps | $189.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | MEDIUM | 4 vCore / 8GB / 160GB SSD | 6TB | 1Gbps | $320.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | LARGE | 8 vCore / 16GB / 320GB SSD | 8TB | 1Gbps | $429.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Premium | GIANT | 8 vCore / 24GB / 640GB SSD | 15TB | 1Gbps | $829.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | WEE | 1 vCore / 1GB / 20GB SSD | 1TB Max IN/OUT | — | $36.90/年 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | TINY | 1 vCore / 1GB / 20GB SSD | 2TB Max IN/OUT | — | $6.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | STARTER | 1 vCore / 2GB / 40GB SSD | 4TB Max IN/OUT | — | $12.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | MINI | 2 vCore / 2GB / 60GB SSD | 8TB Max IN/OUT | — | $21.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | MICRO | 4 vCore / 4GB / 80GB SSD | 16TB Max IN/OUT | — | $32.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | MEDIUM | 4 vCore / 8GB / 160GB SSD | 32TB Max IN/OUT | — | $49.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | LARGE | 8 vCore / 16GB / 320GB SSD | 64TB Max IN/OUT | — | $99.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |
| TYO AS3 Tier 1 | GIANT | 8 vCore / 24GB / 640GB SSD | 128TB Max IN/OUT | — | $199.90/月 | 可下單 | [ 查看方案](https://bit.ly/DmiT) |

* LAX AN5 Tier 1 General 的 `G12C24G` 在目前官方 Pricing 頁面文字中顯示為 **240000GB Max (IN, OUT)**；因這個數值與同系列前後配置的增長規律並不一致，這裡刻意保留官方頁面的原始展示，不自行改成「24TB」或其他推測值。

DMIT 官方目前的完整價格頁也提醒「products and prices in the table may not be updated in time due to adjustment」，所以這份表適合拿來比較結構，不應取代結帳頁最後價格。

## 真正要大量流量，怎麼挑 DMIT？

### 需求是「流量多」，先看 Tier 1

如果主要工作是備份、下載、鏡像、跨區同步或其他單純需要大量資料傳輸的服務，LAX AN5 Tier 1 Volume 很容易成為搜尋「無限流量VPS」後應該先看的產品。

例如 V2C2G 是 2 vCPU、2GB RAM、40GB SSD，每月 5TB Max (IN, OUT)，$14.90；V4C8G 則是 4 vCPU、8GB RAM、160GB SSD，40TB Max (IN, OUT)，$52.90；V12C24G 則有 160TB Max (IN, OUT)，$199.90。

這種配置的重點不是「無限」兩個字，而是**單位價格能換到多少實際可用 Transfer**。

### 需要更多 CPU / RAM，不只是流量，看看 General

AN5 Tier 1 General 的設計方向相反：同樣是 Tier 1，但把更多預算放在 CPU、RAM 與 SSD。

例如 G16C32G 有 16 vCore、32GB RAM、640GB SSD 與 320TB Max (IN, OUT)，官方價格是 $199.90/月。

這類方案比較適合運算與流量同時上來的工作負載，而不是單純把 VPS 當檔案傳輸管道。

### 如果中國內地訪客是核心，Premium 的價值不只在流量

DMIT 官方將 Premium Network 定位在中國內地與亞太的低延遲、低丟包路由；LAX 官方頁面還明確列出與 China Telecom、China Unicom、China Mobile International 的高容量連接。

所以，如果是：

* 面向中國內地的網站
* 跨境電商
* API / SaaS
* 亞洲遊戲伺服器
* 直播或影音服務

真正應該比較的可能是**流量額度 + 路由品質**，而不是一看到 Tier 1 的 TB 數更大就直接選。

### HKG Eyeball 要注意 Beta 狀態

香港 Eyeball 目前官方標為 Beta，並明確提醒路由仍在調整，不建議對穩定性要求很高的正式生產環境使用。

這是一個很容易被「香港低延遲＋大流量」搜尋結果蓋掉的限制。若是測試、開發或對成本比較敏感的工作，Beta 本身未必是問題；若是高可用業務，則應把它直接當成購買前的風險條件。

## DMIT 的硬體差異也不能忽略

DMIT 目前公開的硬體平台包括 **AN5、AN4、AS3**。

AN5 採 AMD EPYC 9005 系列，也就是 Zen 5，配 DDR5 與 PCIe 5.0 NVMe；官方定位是整個產品線中更偏向高效能的旗艦平台。AN4 採 AMD EPYC 9004 系列、Zen 4；AS3 則採 AMD EPYC 7003 系列、Zen 3，官方把 AS3 定位為較具成本效益的成熟平台。

所以看到兩個套餐的流量相近時，不應只比 Transfer。

例如一台 5TB 的 AN5 與另一台 5TB 的 AS3，表面上的「流量」相同，但 CPU 架構、記憶體平台、儲存介面都可能不同。對網站、API、資料庫或編譯工作來說，這些差異可能比多 1TB 或少 1TB 更實際。

## 「便宜到像無限流量」的方案，有什麼要小心？

近期專門比較 unmetered VPS 的文章裡，有一個非常一致的提醒：**unmetered 並不等於無條件地把網路跑滿。**

有些服務商的「unmetered」本質是「不按 GB 計費，但你只能跑固定的埠速」；有些則會寫 fair use；也有些會在持續大量傳輸後進行流量管理。

因此，對任何標榜「不限流量」的 VPS，都可以直接問：

> 不限的是「資料量」，還是「速度」？
> 超過某個持續使用量後，會不會限速？
> 有沒有 AUP 的「長時間大量頻寬」條款？
> IN/OUT 是怎麼計算？

這四個問題通常比「品牌說不說 Unlimited」更有用。

## DMIT 有沒有目前可以直接用的優惠碼？

本次檢索沒有核實到仍有效的 2026 年 9 月公開優惠碼，因此不建議把舊文章裡的 coupon code 當成現在有效的折扣。

能確認的官方促銷頁之一是 2025 Christmas Event，當時 LAX Tier 1 AN5 系列曾提供最高 20% 折扣與帳戶金返還；但該活動屬 2025 年限定活動，不能拿來當成 2026 年現行優惠。

換句話說，現在比較可靠的做法是進入訂單頁，直接確認結帳時是否出現當期促銷，而不是複製幾個搜尋引擎裡仍存在、但活動早已結束的折扣碼。

[👉 查看目前 DMIT 方案與實際結帳價格](https://bit.ly/DmiT)

## 退款政策其實跟「無限流量」搜尋高度相關

這點很容易被忽略。

DMIT 目前的服務條款規定，新購服務在 **3 天內**、且 VM 使用的傳輸量不超過 **30GB** 時，可以申請全額退款，但支付閘道手續費可能扣除；新購後 30 天內則存在部分退款規則。官方文件也說明，退款後實例會停止並刪除，資料無法恢復。

對高流量 VPS 使用者而言，這代表「先跑幾天把它當壓力測試，再決定要不要留」並沒有想像中自由。

尤其你真正想測的如果就是：

* 高頻寬下載
* 大量跨區傳輸
* 長時間串流
* 高頻率備份
* 長時間 VPN / Relay 類型工作

那麼 30GB 的退款測試額度其實很快就可能用完。

此外，官方 AUP 對長時間大量佔用頻寬本身也有明確限制，因此高流量工作負載一定要先看產品額度與 AUP，而不是只看價格頁第一眼的數字。

## 目前網路評價怎麼看？

DMIT 的第三方評價需要特別小心解讀，因為**樣本非常小**。

本次查到的 Trustpilot 頁面只有 4 則評價，頁面顯示 2.6/5，且有 3 則是在最近 12 個月留下；2026 年 3 月至 5 月的幾則新評價，主要提到斷線、客服處理與退款等問題。

這些內容可以作為風險訊號，但不能直接當成所有 DMIT 使用者的整體體驗。原因很簡單：**4 則評論遠不足以代表整個客戶群。**

另一方面，近期一些技術型第三方文章與社群討論，較常提到的是 DMIT 的路由品質、亞洲到美國之間的網路表現，以及不同 Tier 之間的差異；也有使用者在 Reddit 討論中提到自己使用 DMIT 與其他 VPS 服務做跨區連線。

因此，比起「DMIT 評價到底好不好」，更實際的做法是把問題拆成：

**你的機房需求、路由需求、頻寬需求、容忍的服務條款，以及客服／退款風險，哪一項最重要？**

## 哪些情況其實不需要「無限流量 VPS」？

這是搜尋這個關鍵字時很容易忽略的一點。

如果網站每月只有 100GB 到 500GB 流量，那麼為了「不限流量」多付幾倍價格，很可能沒有意義。

近期的頻寬情境分析也指出，普通內容網站的月流量可能遠低於影音、軟體鏡像或大型檔案配送；真正到了數 TB、數十 TB，Transfer 才會成為明顯的成本項目。

可以用這種方式判斷：

**低流量網站**：先看 CPU、RAM、儲存與機房。

**數 TB／月**：開始比較 Transfer 與 port speed。

**10TB～50TB／月**：Tier 1、Volume 類產品會變得非常值得比較。

**長期數十 TB、甚至更高**：除了 VPS，還應比較專用伺服器、物件儲存＋CDN、下載節點或專門的流量型基礎設施。

因為到了這個量級，「VPS 每月多少錢」已經不是唯一成本。

## FAQ：關於無限流量VPS的幾個常見問題

### DMIT 有真正不限流量的 VPS 嗎？

目前公開 Cloud Instance Pricing 中，主流方案主要是固定 Transfer 或 `Max (IN, OUT)` 額度，不應直接理解成無條件 Unlimited。部分 DMIT 產品資料中確實能看到 Unmetered 的描述，但它與固定 Transfer、port speed 和 AUP 是分開的條件。

### `Max (IN, OUT)` 是什麼意思？

它代表官方以 IN/OUT 綜合方式標示最大傳輸額度。例如 LAX AN5 Tier 1 V2C2G 是 5,000GB Max (IN, OUT)。

### 10Gbps 就代表每個月可以跑非常大的流量嗎？

不是。

10Gbps 是埠速，不是月度 Transfer 額度。你仍然需要看方案本身的 GB/TB 限額、`Max (IN, OUT)` 或其他傳輸條款。

### Tier 1 一定比 Premium 好嗎？

不能這樣直接判斷。

Tier 1 的定位偏向高頻寬、全球與亞太／美洲的一般路由；Premium 更重視中國內地與亞太的專項路由品質。要看你的訪客與流量實際從哪裡來。

### HKG Eyeball 可以拿來跑正式業務嗎？

官方目前把 HKG Eyeball 標成 Beta，並提醒網路路由仍在調校，不建議需要高穩定性的 production workload 使用。

### DMIT 有免費試用嗎？

本次檢索沒有核實到一個可直接視為「免費試用」的現行公開活動。官方目前比較明確的是退款政策：新購 3 天內、使用不超過 30GB Transfer 的情況可申請全額退款，但仍須符合其他條款。

## 結論：找「無限流量」之前，先決定自己到底需要什麼

「無限流量VPS」這個搜尋詞很容易讓人先盯著 Unlimited 三個字，但實際購買時，真正該比較的是：

**每月 Transfer 額度 → IN/OUT 計算方式 → Port Speed → AUP → 機房 → Network Series → 硬體平台。**

就 DMIT 目前的產品結構來看，它更適合用「**大流量 + 路由需求 + 硬體需求**」去理解，而不是簡單歸類成一家「無限流量 VPS 商家」。

如果你的第一優先是大量資料傳輸，可以先研究 LAX AN5 Tier 1 Volume、General 與各地 Tier 1；如果核心使用者在中國內地或亞太，則 Premium 的路由特性值得放在 Transfer 數字之前考慮。若只是一般網站或 API，則不必為了「無限」兩個字刻意買過大的流量方案。

最後，DMIT 的官方價格頁本身也提醒價格可能因調整而滯後，所以真正準備付款前，最好再確認一次**庫存、帳單週期、Transfer、網路系列與結帳頁實際價格**。

[👉 查看 DMIT 目前所有可購買方案](https://bit.ly/DmiT)
