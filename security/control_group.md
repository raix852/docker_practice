## 控制組
控制組是 Linux 容器機制的另外一個關鍵組件，負責實作資源的審計和限制。

它提供了很多有用的特性；以及確保各個容器可以公平地分享主機的內存、CPU、磁盤 IO 等資源；當然，更重要的是，控制組確保了當容器內的資源使用產生壓力時不會連累主機系統。

盡管控制組不負責隔離容器之間相互訪問、處理數據和程式，它在防止拒絕服務（DDOS）攻擊方面是必不可少的。尤其是在多使用者的平臺（比如公有或私有的 PaaS）上，控制組十分重要。例如，當某些應用程式表現異常的時候，可以保證一致地正常執行和效能。

控制組機制始於 2006 年，內核從 2.6.24 版本開始被引入。
