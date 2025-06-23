# 小黑窗步驟：

1. 安裝依賴 輸入 `sudo apt-get install lib32gcc-s1` 
2. 安裝 steamcmd 
	- 輸入 `sudo apt update` 
	- 輸入 `sudo apt install software-properties-common` 
	- 輸入 `sudo apt-add-repository non-free` 
	- 輸入 `sudo dpkg --add-architecture i386` 
	- 輸入 `sudo apt update` 
	- 輸入 `sudo apt install steamcmd` 遇到 *y/n* 就打 `y`，這個會有一個協議要確認，按 `TAB` 到 OK 然後 `ENTER` 即可，接著選 I AGREE，一樣 `ENTER` 
3. 輸入 `steamcmd` 
4. 完成後下方會有一個Steam大於符號 
5. 接著 ctrl+c，按enter回到主畫面 
6. 輸入指令查看主畫面有沒有steam資料夾 輸入 `ls` 
7. 安裝 palword 的伺服器，這個會跑得比較久 (之後更新伺服器檔案也是用這指令就好) 

```sh
steamcmd +login anonymous +app_update 2394010 validate +quit
```

8. 跑完之後我們要先運行一次，產生資料夾 
	- 輸入cd 
	- 輸入cd ~/Steam/steamapps/common/PalServer 
	- 輸入./PalServer.sh 注意!!若是這邊有出現錯誤，請執行以下指令： 

```sh
mkdir -p ~/.steam/sdk64/ steamcmd +login anonymous +app_update 1007 +quit cp ~/Steam/steamapps/common/Steamworks\ SDK\ Redist/linux64/steamclient.so ~/.steam/sdk64/
```

之後再執行一次 `./PalServer.sh` 這個指令，成功之後會出現四行 `S_API` ... 
```sh
./PalServer.sh
```

9. ctrl+c 關掉伺服器，我們現在要去修改伺服器的資訊 

```sh
cd ~/Steam/steamapps/common/PalServer/Pal/Saved/Config/LinuxServer
```

輸入 `ls` 查看資料夾下的文件確認有 `PalWorldSettings.ini` 
輸入 `vi PalWorldSettings.ini` 開啟記事本，去連結下載這個然後複製起來貼上：

```sh
vi PalWorldSettings.ini
```

[https://supr.link/ocNwz](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbG5oa2d2bTFCOXRSaHZXaTV3d2cwVkRLT3k0QXxBQ3Jtc0tuQVc3RUFhV2p3bFNsbGZnT3I4U0ZSVERGWkZDbjBONkFGTUY3NTFFR1BUVHFTWEtRN285b0ZLN1FSZ1dQMzkyRHFESDBvUnhkTFVvMmRfMDd6RlFxMjc3MENXOTRza0lYQ2ktd0xwSWxOVHNwNEJVWQ&q=https%3A%2F%2Fsupr.link%2FocNwz&v=tOQCqQw6ojY) 需要更改的參數： 
1. AdminPassword="管理員密碼" 
2. ServerPassword="你的伺服器密碼" 
3. PublicIP="你的IP" 

> 直接在 [[vim]] 修改也可以。

10. 開服 
	- 輸入screen 
	- 輸入cd Steam 
	- 輸入cd ~/Steam/steamapps/common/PalServer 
	- 輸入./PalServer.sh 
	- 按住Ctrl+A 再按D 跳出去 開始爽玩！！！ 

▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬ 

# 事後修改伺服器參數：

0. 先把伺服器關掉 
1. 把PalServer/Pal/Saved/Config/LinuxServer裡面的"PalWorldSettings.ini"複製出來 

```sh
cd ~/Steam/steamapps/common/PalServer/Pal/Saved/Config/LinuxServer
```

2. 打開"PalWorldSettings.ini"，並依照下面網址改成你喜歡的參數 [https://supr.link/DNFp1](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbElSZGEwMl9ZcU8zM3d5UUNsMkloSy13Vnpqd3xBQ3Jtc0trV1AzcTJHX1NaNFVMUTF1TWx3aHNKc1p2LWt0ZFd3bW15VTZtUFY3UWNxR2h3T3FHNVNocC13ZGFXX2ZPRS1fVkx3Z194VUVEVXhJQjBYUXRxSmNTZVNOYlowb0MzN0tYbjJMVUItMkxsYjAxRXVnbw&q=https%3A%2F%2Fsupr.link%2FDNFp1&v=tOQCqQw6ojY) 
3. 把伺服器裡面的"PalWorldSettings.ini"刪掉，再把你改好的貼回去 
4. 更改權限 
- a.輸入 `cd` 回到主畫面 
- b.輸入 `sudo su` 
- c.輸入 chmod -R 777 /home/你的google帳號/Steam/steamapps/common/PalServer
```sh
chmod -R 777 /home/gaildev48/Steam/steamapps/common/PalServer
```
- d.按Ctrl+D結束root權限 

▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬ 

# 開服步驟： 

1. 輸入 `screen` 
2. 輸入 `cd Steam` 
3. 輸入 `cd ~/Steam/steamapps/common/PalServer` 
4. 輸入` ./PalServer.sh` 

```sh
cd ~/Steam/steamapps/common/PalServer
```

```sh
./PalServer.sh
```

5. 按住 Ctrl+A 再按D 跳出去。備註：Ctrl+C是關閉伺服器 

▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬ 

# 如何更新版本： 

1. 先關服 
2. 輸入 cd 
3. 輸入 steamcmd +login anonymous +app_update 2394010 validate +quit 
4. 重新開服，搞定 

▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬ 

# 用得到的指令： 

1. 查看現在的視窗 輸入screen -r 
2. 跳到指定視窗 輸入screen -r XXXX 
3. 回到主帳號資料夾 輸入cd 
4. 其他的問ChatGPT，他會好好教你 

▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬ 

# 注意事項 

1. 記得額度要去查看一下，快到了記得備份！ 建議跟朋友玩完就主動關掉伺服器然後備份！ 
2. 影片仔細看完，不跳過，才能完整學會，心急的人玩單機就好。 
3. 不要一直在下方留言問"誰有信用卡能借我"，我會扁人。

```sh
cd ~/Steam/steamapps/common/PalServer/Pal/Saved/SaveGames/0/24611FCADE664A3CB3C2BCCFBE9A1F4E
```

# 自動備份

1. 準備好 WinSCP script。
	- [Automate file transfers (or synchronization) to FTP server or SFTP server :: WinSCP](https://winscp.net/eng/docs/guide_automation) 
	- 如何透過 generating tool 寫 script:  [Generate Session URL/Code/Transfer Code Dialog](https://winscp.net/eng/docs/ui_generateurl) 
2. if script works, then use 工作排程器(Task Scheduler) in Windows 7~11.
	- [Schedule file transfers (or synchronization) to FTP/SFTP server :: WinSCP](https://winscp.net/eng/docs/guide_schedule) 