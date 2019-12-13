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

查了資料，應該是要用gMock模擬出 有空再搞。先workaround吧。
