# 去除ubuntu資料夾有鎖頭標誌

轉載自:https://www.itread01.com/content/1549487891.html

1、linux下資料夾有鎖頭，去掉鎖頭的辦法 去除Ubuntu資料夾有鎖標誌   

由於在root許可權下下載的東西，所以在普通使用者下有鎖標誌，雖說在root下什麼都可以操作使用，

只要切換到root賬戶操作就ok了，但是看著就很彆扭，如何去除？ 1.看到有網友說使用   sudo chmod -R 777     

別且說可能有危險，這不廢話麼，對檔案遞迴做改變許可權為可讀可寫可執行，當然沒有鎖了。 

但是這就會使得原來的檔案的許可權發生變化。   

2.可以把它拷貝到有windows的電腦上……呵呵，這樣當然可以，linux下的許可權什麼的自然全消失。 

3.使用命令改變檔案的組使用者   sudo chown 你的使用者名稱 檔名     

例如：   sudo chown jack common      

事實證明這樣是可以的，但是這樣只是改變common這個資料夾的Group，

裡邊的檔案或者資料夾還是有鎖的。所以要   sudo chown jack common/ -R 

