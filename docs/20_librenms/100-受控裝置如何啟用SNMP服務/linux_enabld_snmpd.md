
# Linux 啟用 snmpd

以下使用 Ubuntu 24.04 為例

##  安裝 SNMP 套件

```bash
sudo apt update
sudo apt install snmp snmpd
```

## 編輯 snmp 設定檔

SNMPD 的主要設定檔位於 `/etc/snmp/snmpd.conf`。您需要編輯此檔案以符合您的需求。建議先備份原始設定檔：

```bash
sudo mv /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.original
```

然後，使用您喜歡的文字編輯器（如 nano）開啟 `snmpd.conf`：

```bash
sudo nano /etc/snmp/snmpd.conf
```

貼上以下設定檔，  public 社群名稱類似於 SNMP 的密碼，可以修改為自己的設定， default 表示允許所有遠端 NMS 主機查詢本機資訊。
syscontact、syslocation 如果要設定，可以刪除前方的 # 號字元，前方有 # 號表示註解。

```
rocommunity public default
#syscontact root@localhost
#syslocation My Server
```


>[!Tip]
>如果要限制 NMS ip 範圍，可以參考下方設定， rocommunity 可以同時有多行，community_string_1 跟 community_string_2 可以是相同的
>```
>rocommunity community_string_1 192.168.0.0/24
>rocommunity community_string_2 10.1.0.0/16
>```


## 儲存變更並重新啟動 SNMPD

```bash
sudo systemctl restart snmpd
```
