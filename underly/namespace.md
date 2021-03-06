## 名字空間
名字空間是 Linux 內核一個強大的特性。每個容器都有自己單獨的名字空間，執行在其中的應用都像是在獨立的作業系統中執行一樣。名字空間保證了容器之間彼此互不影響。

### pid 名字空間
不同使用者的程式就是透過 pid 名字空間隔離開的，且不同名字空間中可以有相同 pid。所有的 LXC 程式在 Docker 中的父程式為Docker程式，每個 LXC 程式具有不同的名字空間。同時由於允許嵌套，因此可以很方便的實作嵌套的 Docker 容器。

### net 名字空間
有了 pid 名字空間, 每個名字空間中的 pid 能夠相互隔離，但是網路端口還是共享 host 的端口。網路隔離是透過 net 名字空間實作的， 每個 net 名字空間有獨立的 網路設備, IP 地址, 路由表, /proc/net 目錄。這樣每個容器的網路就能隔離開來。Docker 默認采用 veth 的方式，將容器中的虛擬網卡同 host 上的一 個Docker 網橋 docker0 連接在一起。

### ipc 名字空間
容器中程式交互還是采用了 Linux 常見的程式間交互方法(interprocess communication - IPC), 包括信號量、消息隊列和共享內存等。然而同 VM 不同的是，容器的程式間交互實際上還是 host 上具有相同 pid 名字空間中的程式間交互，因此需要在 IPC 資源申請時加入名字空間訊息，每個 IPC 資源有一個唯一的 32 位 id。

### mnt 名字空間
類似 chroot，將一個程式放到一個特定的目錄執行。mnt 名字空間允許不同名字空間的程式看到的文件結構不同，這樣每個名字空間 中的程式所看到的文件目錄就被隔離開了。同 chroot 不同，每個名字空間中的容器在 /proc/mounts 的訊息只包含所在名字空間的 mount point。

### uts 名字空間
UTS("UNIX Time-sharing System") 名字空間允許每個容器擁有獨立的 hostname 和 domain name, 使其在網路上可以被視作一個獨立的節點而非 主機上的一個程式。

### user 名字空間
每個容器可以有不同的使用者和組 id, 也就是說可以在容器內用容器內部的使用者執行程式而非主機上的使用者。

*註：關於 Linux 上的名字空間，[這篇文章](http://blog.scottlowe.org/2013/09/04/introducing-linux-network-namespaces/) 介紹的很好。
