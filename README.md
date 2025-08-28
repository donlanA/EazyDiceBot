簡易骰子機器人
===

## 簡介

使用`Docker`、`Render`，可同時在`Discord`與`Line`使用的簡易骰子機器人。

## 參考資料

- [HTKRPG](https://github.com/hktrpg/TG.line.Discord.Roll.Bot)
- [RoboYabaso](https://github.com/retsnimle/RoboYabaso)（大部分的程式架構來自於此）
- [如何建立自己的Line骰子機器人](https://docs.google.com/document/d/1dYnJqF2_QTp90ld4YXj6X8kgxvjUoHrB4E2seqlDlAk/edit)
- [Render 部署 Discord Bot](https://hackmd.io/@3Q1PwoaDQXSlvMLWWzaBww/S1pEto_ap)
- GitHub Copliot（AI太好用了你們知道嗎）

## 會用到的網站

- Github
- [Line Offical Account Manager](https://manager.line.biz/)
- [Line Developer](https://developers.line.biz/console/)
- [Discord Developer Portal](https://discord.com/developers/applications/)
- [Docker Hub](https://www.docker.com/products/docker-desktop/)（需下載應用程式）
- [Render](https://render.com/)
- [UptimeRobot](https://uptimerobot.com/)（監控機器人用的，不一定需要）

## 步驟

1. 安裝套件
   ```
   npm install discord.js
   npm install express
   npm install dotenv
   ```

2. 取得 Token，建立 env 檔

   - `Discord`的`Token`：
     1. [Discord Developer Portal](https://discord.com/developers/applications/) > 你創的角色 > Bot > Token > Reset Token
     2. 此外，同樣在`Bot`裡面，把`Message Content Intent`打開，機器人才能傳訊息
     3. 在`OAuth2`勾選`Bot`跟你要的權限，取得機器人的邀請連結。詳見[Render 部署 Discord Bot](https://hackmd.io/@3Q1PwoaDQXSlvMLWWzaBww/S1pEto_ap)
   
   - `Line`的`Token`：
     1. [Line Offical Account Manager](https://manager.line.biz/) > 你創的角色 > 右上角的設定 > Messaging API > 開始使用
     2. 接著到 [Line Developer](https://developers.line.biz/console/) > 你創的角色 > Messaging API > 滑到最下面 Channel access token > 按旁邊的 issue
  
   `Discord`跟`Line`的步驟做完都會產生一串亂碼，就是各自的`Token`。

   這個是不能分享出去的，所以我們要用另外的方式儲存。

   在根目錄建一個叫做`.env`（沒有副檔名）的檔案，儲存`Line`跟`Discord`的`Token`。格式如下：
   ```
   DISCORD_TOKEN=你的 Discord token
   LINE_TOKEN=你的 Line Channel Access Token
   ```

4. 修改 & 測試

   機器人回覆邏輯都在`replies.js`，還有一點點在`discordbot.js`，是在特定伺服器使用自訂表情符號的功能。
   
   改好之後輸入以下指令進行本地測試：
   ```
   npm start
   ```

   本地測試`Discord`機器人是會上線的，`Line`要等部屬到`Render`才行。

5. 上傳 Docker hub

   感覺做得差不多了就能上傳了，可以先用`docker -v`測試一下是否有成功安裝`Docker`。
   ```
   docker build -t 你的github名稱/你的機器人名稱:v1 .
   docker push 你的github名稱/你的機器人名稱:v1
   ```
   之後有修改程式，也是跑這兩行就能更新。
   
7. 部署到 Render、UptimeRobot

   這邊就跟程式本體沒關係了，[Render 部署 Discord Bot](https://hackmd.io/@3Q1PwoaDQXSlvMLWWzaBww/S1pEto_ap) 有非常詳細的說明。

   部署成功之後，`Discord`機器人就可以跟本地測試時一樣自由使用。

8. 啟用 Line Webhook

   部署到`Render`後會有一串網址，大概會是`https://你的機器人名稱-v1.onrender.com`，在後面加上`/line`就能切換成`Line`機器人的模式。

   也就是將`https://你的機器人名稱-v1.onrender.com/line`這串貼到：

   [Line Developer](https://developers.line.biz/console/) > 你創的帳號 > Messaging API > Webhook settings > Webhook URL

   按一下`Verify`，出現`Success`就是成功了。



