
# 使用 Docker 快速安裝 librenms

docker 本身是在 Linux 環境中執行，所以以下操作，是在真實 Linux主機、虛擬機或是WSL環境下為例。

# 安裝 Docker Engine

1. 設定 docker 軟體來源
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. 安裝 docker 套件
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. 測試 docker 是否安裝成功，下面指令會執行測試用的 docker 程式，如果可以正常並顯示歡迎訊息，就表示 docker engine 已經安裝成功
```bash
 sudo docker run hello-world
```

## 使用 docker 執行 librenms

1. 下載並解壓縮 librenms 的 composer 檔：
```bash
mkdir librenms
cd librenms
wget https://github.com/librenms/docker/archive/refs/heads/master.zip
unzip master.zip
cd docker-master/examples/compose
```
2. 啟動 Docker 容器
```bash
sudo docker compose -f compose.yml up -d
```

3. 開啟瀏覽器連線到  http://localhost:8000  （如果有外部 ip，建議使用外部 ip不要使用 localhost），設定我們要使用的 librenms 帳號密碼，這裡我們都設定為 librenms:librenms

![[1738554880725.png]]

3. 完成安裝....

## 備份或遷移使用 docker 安裝的 Librenms

依照預設安裝路徑，我們只要備份　docker-master/examples/compose　這個資料夾即可將整個 librenms 備份或帶走，只要複製到另一個 Linux 主機或系統，再執行前面步驟2方式啟動 Docker 容器，整個 librenms 就恢復了。

因為資料夾內系統使用者，所以我們要打包資料夾，必須用 sudo 打包
```bash
 sudo tar -zcvf compose.tar.gz compose
```
解壓縮
```bash
tar -zxvf compose.tar.gz
```
 
## 進入 docker 容器的 shell 環境

```bash
sudo docker exec -i -t librenms /bin/bash
```

