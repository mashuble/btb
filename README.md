# BTB
How to setup a bitcoin trading bot

### Open a [Coinbase](https://www.coinbase.com/join/5a3108cabe6cae01470f94ec) Account (Get $10 by signup with this link)

- Verify your account
- Put some money ($3000 recommended)
- Buy LiteCoin

### Open a [Binance](https://www.binance.com/?ref=16153326) Account

- Verify your account
- Enable Google Auth
- Send your LiteCoin in your Binance account
- Once you received them
- Sell them for BTC

### Create the API Keys

- Go to your Binance profile page
- On API section, click on "Configuration"
- Create one API Key named "api_pt_key" 
- Vérify to have "Read Info" enabled
- Create an another one named "api_pt_trading_key"
- Verify to have "Read Info" and "Trading" enabled

### Buy the bot (~$700)
------

###### The main bot (0.3 BTC)
https://profittrailer.com/

When you bought it choose Binance and give your Binance "api_pt_key" API Key

###### The PT Feeder plugin (0.2 BTC)
https://cryptoprofitbot.com/

### Take a VPS on [Digital Ocean](https://m.do.co/c/f4ab380ccaba) (Get $10 by signup with this link). 
------

Once you're signed in
- Click on create
- Droplet
- One-click apps
- NodeJS 6.11.5 on 16.04
- Choose the standard at 10$ or 15$
- Choose "Singapore" in "Datacenter Region"
- Add "Monitoring" in "Options"
- Add your ssh key
- Name your droplet (eg: pt-binance-btc)
- Once you have the ip address
- ssh root@DROPLET_IP_ADDRESS

#### Server Installation

```
 $ apt-get update
 $ apt-get install default-jdk -y
 $ apt-get install unzip -y
 $ npm install pm2@latest -g
 $ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
 $ mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
 $ sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
 $ apt-get update
 $ apt-get install dotnet-sdk-2.0.2 -y
 $ apt-get update

 $ wget https://github.com/taniman/profit-trailer/releases/download/v1.2.6.8/ProfitTrailer.zip
 $ wget https://github.com/mehtadone/PTFeeder/releases/download/pt-feeder-v1.3.2.194/pt-feeder-v1.3.2.194.zip
 $ unzip pt-feeder-v1.3.2.194.zip -d pt-feeder
 $ unzip ProfitTrailer.zip
 $ mv ProfitTrailer pt
 $ rm -Rf pt/trading
 $ rm -Rf pt-feeder/config
 $ chmod +x pt/ProfitTrailer.jar
 $ chmod +x pt-feeder/pt-feeder.dll
 $ wget https://github.com/mashuble/bitcoin-trading-bot/releases/download/1.0.0/PT-Files.zip
 $ unzip PT-Files.zip
 $ rm PT-Files.zip 
 $ mv PT\ Files/Profit\ Trailer\ Files/ pt/trading/
 $ mv PT\ Files/Feeder\ Files/ pt-feeder/config/
 $ rm -Rf PT\ Files/
 $ ufw allow 8081
 ```
 
#### Bot Configuration

##### Telegram Configuration

This is necessary to get feedback from your bot.
1. Install Telegram on your phone or computer

2. Talk to @BotFather (NOT to be confused with TheBotFather channel!!!) - Once there type ```/newbot``` to create a bot. Answer the questions by typing answers and pressing enter. 
You will receive a message containing your bot token.

3. Start a chat with your bot's name (for example @MyTPBot) and press start button

4. Send a dummy message to the bot. 
You can use this example: /my_id @your_bot_name
(I tried a few messages, not all the messages work. The example above works fine. Maybe the message should start with /)

5. Go to following url: https://api.telegram.org/botXXX:YYYY/getUpdates
replace XXX:YYYY with your bot token

6. Look for "chat":{"id":-zzzzzzzzzz,
-zzzzzzzzzz is your Telegram Chat ID (with the negative sign).

##### Application File (Profit Trailer)

```
vim pt/application.properties
```
Erase what you have and Paste this

```
server.port = 8081

telegram.botToken = YOUR_TELEGRAM_BOT_TOKEN
telegram.chatId = YOUR_TELEGRAM_CHAT_ID
telegram.postNewOrders = true

trading.exchange = BINANCE
server.timeZoneOffset = +01:00
server.sitename = PT Binance
server.password = CHOOSE_A_VERY_STRONG_PASSWORD
server.enableConfig = true

# how many days of log history to show?
trading.logHistory = 2

#Put here your licensed API key
default_apiKey = YOUR_BINANCE_api_pt_key_API_KEY
default_apiSecret = YOUR_BINANCE_api_pt_key_API_SECRET

#Put here a second api key that will be used to do all the buying and selling.
#This api key does NOT need to be activated
trading_apiKey = YOUR_BINANCE_api_pt_trading_key_API_KEY
trading_apiSecret = YOUR_BINANCE_api_pt_trading_key_API_SECRET
```

##### Configuration File (Profit Trailer)
```
vim pt/configuration.properties
```

Erase what you have and Paste this
 
```
#sell options
sell_fillOrKill = false
sell_immediateOrCancel = true

#buy options
buy_fillOrKill = false
buy_immediateOrCancel = true

hideDust=true

testMode=false

BTC_dust = 0.00205
ETH_dust = 0.0105
BNB_dust = 0.0105
USDT_dust = 1.05
```

###### Host Settings File (PT Feeder)

```
vim pt-feeder/config/hostsettings.json
```

Erase what you have and Paste this

```
{
  "Host": {
    "TrexPairsLocation": "",
    "PoloPairsLocation": "",
    "BinancePairsLocation": "/root/pt/trading",
    "HostName": "BTC",
    "LicenseKey": "YOUR_PT_FEEDER_API_KEY",
    "ServerUrls": "http://localhost:8083",
    "MarketConditionCheckInMinutes": "3",
    "CalculateTrailingValues": "true",
    "UseMaxCostPercentage": "true",
    "UseMinBuyBalancePercentage": "true",
    "TelegramBotId": "YOUR_TELEGRAM_BOT_ID",
    "TelegramChatId": "TELEGRAM_CHAT_ID"
  },
  "Serilog": {
    "MinimumLevel": "Information"
  }
}
```

#### Bot starting

```
cd /root/pt-feeder/
pm2 start pm2-PT-Feeder.json 
# Wait 3-4min
# Look at the logs in /root/pt-feeder/logs/
# tail -f FichierDeLog.log
cd /root/pt/
pm2 start pm2-ProfitTrailer.json 
```

if you want to list all the instances you can do ```pm2 ls```

if you want to restart / reload an instance ```pm2 reload ID_INSTANCE```

if you want to stop an instance ```pm2 stop ID_INSTANCE```

### Go to DROPLET_IP_ADDRESS:8081
-----

Write your password

### Watch the logs
-----

Make sure to watch the logs to see if any problem occur

```
tail -f /root/pt/logs/ProfitTrailer.log
```

### Let the money in $$$

------

### Crypto

```
ETH 0x2D071d383a2C93E622b6c6b54Ece8dCFEF951053
BTC 1ERPdyWYaguPgy3Vcr7wSqUFt3152PJBKM
BCH 17zAmjPDBurRtWULccyHkgeoRTrK5BU3SN
LTC LQaHrXhvrRhM1rYqeqnM2ikR2iXGu7wW4W
```

Thank you!
