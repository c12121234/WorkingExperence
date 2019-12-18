# 搭配googleTest常見問題


## 1. QObject problem
新鮮熱騰騰剛碰到

想說怎麼存取的tag通通變成private

![image](https://github.com/c12121234/WorkingExperence/blob/master/%E9%97%9C%E6%96%BCQt/pic/googleTestMixQtProblem1.png)


回去看header檔 wtf

![image](https://github.com/c12121234/WorkingExperence/blob/master/%E9%97%9C%E6%96%BCQt/pic/googleTestMixQtProblem2.png)

沒問題啊 黑人問號

自己查半天沒資料，突然覺得那MACRO很可疑 就註解掉看看

![image](https://github.com/c12121234/WorkingExperence/blob/master/%E9%97%9C%E6%96%BCQt/pic/googleTestMixQtProblem4.png)

...

我勒還真的可以了

![image](https://github.com/c12121234/WorkingExperence/blob/master/%E9%97%9C%E6%96%BCQt/pic/googleTestMixQtProblem3.png)

~~查了資料，應該是要用gMock模擬出 有空再搞。先workaround吧~~

找到解決方法了！ 以圖片的ChessBoard來舉例

我們要先找到moc_chessbard.cpp這個檔案，裡面有關於QOBJECT展開後的東西

把這檔案加入 sources來源就行了。

![image](https://github.com/c12121234/WorkingExperence/blob/master/%E9%97%9C%E6%96%BCQt/pic/googleTestMixQtProblem5.png)

## 2. 將待測專案(檔案)和測試專案連結

一樣，把待測檔案加入到SOURCES 表頭檔就加到HEADERS 路徑就加到INCLUDEPATH

路徑的表現方式留意，如 ../ 代表往上一層 

![image](https://github.com/c12121234/WorkingExperence/blob/master/%E9%97%9C%E6%96%BCQt/pic/googleTestMixQtProblem6.png)

紅線部分都自行加入的

## 3. 測試專案和待測專案的切換(?)

將moc.xxx.cpp加到測試專案的source後 似乎.o檔就會產生在測試專案的資料夾底下了

然後會造成原待測專案link error = =

目前work around方法是先暫時將測試專案那些屬於待測專案的路徑註解掉

或將.o檔貼到測試專案底下(大概)

有空再找別的方法。
