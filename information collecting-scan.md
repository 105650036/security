首先選定使用工具有以下可以使用
nmap, zenmap 
sparta
owasp zap
nessus
wmap
nmap vulnscan 
arachni
openvas
wacsan
snmp-check
sslscan

在此我們以nmap(其中zenmap為nmap的UI版本)為例(其他工具流程大致相同 只是能找到的結果有所不同)
掃描有兩種可能 一種是來自內部的滲透測試 一種是來自外部
外部的話代表攻擊者以鎖定目標 但若是內部的話則要選定範圍 可以用以下方法
首先 使用ifconfig(windows是 ipconfig)來取得當前在區網內的位址
![未命名](https://user-images.githubusercontent.com/49279418/117000618-eef5c480-ad13-11eb-88e6-91f363d61a16.png)

使用arp-scan IP範圍 來搜尋在同個區域網段內的設備(也可以使用nmap掃描)
(所以在外時不要輕易連接WIFI 很容易被取得資訊 特別是筆電可能藏有公司資料->可以在準備階段的教育訓練來避免)
![1](https://user-images.githubusercontent.com/49279418/117001510-071a1380-ad15-11eb-8315-d889a7ef32c5.png)

可以看到用此指令找到192.168.2.101 及192.168.2.104(192.168.2.1為保留IP)同時觀察靶機(目標主機)
確實有被找到 找到標的後就可以開始掃描
![2](https://user-images.githubusercontent.com/49279418/117001943-8871a600-ad15-11eb-89f5-d2e588e85c4a.png)

使用nmap -sV -O -A -p- 192.168.2.104 指令去掃描目標位址 (以下為部分結果)
(-sV -O...等為可調用選項 通常越多越吵雜越容易被發現 真實世界的攻擊可能更加安靜 更不易被察覺)
![3](https://user-images.githubusercontent.com/49279418/117003858-f7e89500-ad17-11eb-85fc-f91caa722812.png)
![4](https://user-images.githubusercontent.com/49279418/117003867-fa4aef00-ad17-11eb-9e94-21af86e81859.png)

可以看到許多開放的port(由於是靶機所以特別多) 以及每個port 的詳細資訊如版本(可以被拿來查詢是否有漏洞) 所以對於版本是否要異動就需要做風險評鑑(資安風險管理)

實際上需要開放給外部的port(看的到的)可能不需要那麼多 有一些功能只需要給內部 這時候就可以透過防火牆(資安軟硬體)來阻擋來自外部的連線
或是有一些功能只想開放給特定知情人事那就可以設定外部詢問時不回覆(有開放 不開放 不回覆等三種結果) 來迷惑攻擊者
此外也可以透過honey pot(資安軟硬體) 來觀察是否有惡意的掃描(因為一般人不會去碰到這個port)

而蒐集情報完畢了攻擊方就會準備去做武裝病毒 派送病毒的動作(有些漏洞本身就可以遠端操控不需用到病毒)

