# Bitcoin Trading Bot
How to setup a bitcoin trading bot

### Buy the bot (~700€)

###### The main bot (0.3 Bitcoin)
https://profittrailer.com/

###### The PT Feeder plugin (0.2 Bitcoin)
https://cryptoprofitbot.com/

### Take a VPS on digital Ocean https://m.do.co/c/f4ab380ccaba (Get $10 by signup with this link). 

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
- ssh root@IP_ADDRESS

```
apt-get update
apt-get install default-jdk -y
apt-get install unzip -y
npm install pm2@latest -g
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
apt-get update
apt-get install dotnet-sdk-2.0.2 -y
apt-get update

wget https://github.com/taniman/profit-trailer/releases/download/v1.2.6.8/ProfitTrailer.zip
wget https://github.com/mehtadone/PTFeeder/releases/download/pt-feeder-v1.3.2.194/pt-feeder-v1.3.2.194.zip
unzip pt-feeder-v1.3.2.194.zip -d pt-feeder
unzip ProfitTrailer.zip
mv ProfitTrailer pt
rm -Rf pt/trading
rm -Rf pt-feeder/config
chmod +x pt/ProfitTrailer.jar
chmod +x pt-feeder/pt-feeder.dll
wget http://jasonappleton.com/wp-content/uploads/2018/01/PT-Files.zip
rm PT-Files.zip 
unzip PT-Files.zip
mv PT\ Files/Profit\ Trailer\ Files/ pt/trading/
mv PT\ Files/Feeder\ Files/ pt-feeder/config/
rm -Rf PT\ Files/
wget http://jasonappleton.com/wp-content/uploads/2018/01/Feeder-Crow.zip
unzip Feeder-Crow.zip
vim pt/application.properties
vim pt/configuration.properties 
#vim appsettings.json
vim pt-feeder/config/hostsettings.json
cd /root/pt-feeder/
pm2 start pm2-PT-Feeder.json 
# Attendre 3-4min
# Regarder les logs dans /root/pt-feeder/logs/
# tail -f FichierDeLog.log
cd /root/pt/
pm2 start pm2-ProfitTrailer.json 
ufw allow 8081
```
