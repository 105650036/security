透過先前使用的掃描結果(information collecting-scan)可以得知port80以及port22都有開放

要注意 但port80(http)本身叫難藏就算換port 只要有對外連線的服務都還事會被發現

port 22 是ssh用來遠端連線 就可以藏例如換成port2222

首先就針對port22做攻擊演示

由於是遠端連線我們並不知道帳號所以我們要先找到可以用來登入的帳號

使用enum4linux 192.168.213.129(目標IP)

![13](https://user-images.githubusercontent.com/49279418/117274124-57b57c00-ae8f-11eb-9bbd-5264248dd539.png)

可以看到有找到兩個使用者kay 跟 jan 從前面的UID來看一個是1000一個是1001看起來1001是後面新增的使用者權限上可能較小

被保護的嚴密性應該也較低 所以把這個帳號(jan)當做目標

接著就來準備暴力破密 使用的字典檔是內建的rockyou

(這邊就有可以防範的點 1.不要使用字典檔可能有的密碼 2.提高密碼強度ex:13碼以上 有大小寫及特殊字符 3.不允許多次失敗ex:3次以上就鎖)

鎖次數的部分還是有可能透過拉長時間的方式 如:一天就試兩次 所以還是要做好log的確認檢查是否有異狀

使用hydra -l jan -P /usr/share/wordlists/rockyou.txt(使用的字典檔) 192.168.213.129(目標IP) ssh(登入方式 有ssh telnet等)

![14](https://user-images.githubusercontent.com/49279418/117276940-f5aa4600-ae91-11eb-9f3e-78ee68c3a023.png)

使用ssh的方式登入 並試著提權(取得權限的垂直提權) 但此帳號沒有提權的能力(這部分屬於管理面iso27001的存取控管 也是最重要的一部分)

![15](https://user-images.githubusercontent.com/49279418/117277785-bd573780-ae92-11eb-860f-994e3863bf4b.png)

所以只好去找之前有看到的另一個帳號kay的相關資訊(取得權限的水平擴散) 不斷探索過後可以找到存取ssh認證的檔案

![16](https://user-images.githubusercontent.com/49279418/117278941-cac0f180-ae93-11eb-9ab0-349c99a5a175.png)

使用scp jan@192.168.213.129:/home/kay/.ssh/id_rsa /root/Desktop 把此檔案複製到本機端 

並使用python ssh2john.py id_rsa > /root/Desktop/id_rsa.hash 破密得到內容

![17](https://user-images.githubusercontent.com/49279418/117301824-9a864c80-aead-11eb-8f12-5ae85c76e1cd.png)


最後使用john --wordlist=/usr/share/wordlists/rockyou.txt --format=SSH id_rsa.hash 破密使得我們可以透過憑證登入

![18](https://user-images.githubusercontent.com/49279418/117301919-b0940d00-aead-11eb-81f6-a28d9c6e10be.png)

登入後去內部查看真正的密碼並使用sudo su提權 成功得到root 權限

![19](https://user-images.githubusercontent.com/49279418/117302479-42037f00-aeae-11eb-8e58-d2cc3defc954.png)





