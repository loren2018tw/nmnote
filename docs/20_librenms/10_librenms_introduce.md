# LibreNMS - SNMP 網路管理系統介紹
## 什麼是 LibreNMS?
LibreNMS 是一套支援 SNMP 協定的「網路管理系統」（NMS）。 官方網站： [https://www.librenms.org/]()

## 什麼是 SNMP : 簡易網路管理通訊協定 (SNMP) 

簡易網路管理通訊協定 (SNMP) [^1] ，是由網際網路架構委員會 (IAB) 在 RFC1157 中定義的應用程式層通訊協定，用於交換網路裝置之間的資訊。

網路上有各式各樣的裝置，例如網管型交換器、一般主機、網路型印表機、無線AP... ，這些裝置都有需要關注的資訊，例如處理器負載、處理器溫度、RAM使用率、網卡port的流量、交換器是否斷線、封包的 ip或 macaddress、印表機墨水剩餘量... 

這些網路裝置如果有支援 snmp 協定的服務，我們的 NMS (這裡就是使用 Librenms) 就可以定期主動跟這些裝置要資料，然後集中在 NMS 一次觀看這些受控裝置的狀態。也正因為 SNMP 是標準協定，所以可以管理不同廠商的網路裝置，無須使用特定網管公司的系統。


**GET** ： NMS 使用 port 161 傳送請求給網路裝置，裝置使用 port 162 回傳資料給 NMS
![](snmp-get-response.gif)

**TRAP** ： 網路裝置也可以主動傳送資料給 NMS
![](snmp-trap.gif)

## SNMP 版本

SNMP 協定的版本越新，安全性就越高，目前最多裝置已經都支援到 v2c版本，所以我們都會使用 v2c 版本。

各版本主要是在安全性上面有不同，v2==c== 使用 ==c==ommunity(社群名稱) 作為安全性管理，可以把這個名稱當作「密碼」的意思，受控裝置的 snmp 服務會設定一個 community 字串， NMS 只要知道這個字串，就可以成功跟裝置索取資料。

我們也可以將  community 設定為 ==public==，這樣等同不設定密碼的意思， NMS 會預設使用 public 當作 community 與受控裝置溝通。

另外 v2c 還會設定可以存取的 ip網段，如果設定成 0.0.0.0 表示不限制 nms 的 ip。

## 參考資料
[^1]: [SNMP教程](https://www.manageengine.com/tw/network-monitoring/what-is-snmp.html)
