# 錯誤GL/gl.h: No such file or directory的解決方法(以及cannot find -lGL解決方法)

#### 問題描述：

在linux環境下，

QtCreator第一次編譯時，報錯GL/gl.h: No such file or directory

####  原因分析：

說明系統裡面缺少OpenGl庫

#### 解決方法：

`sudo apt-get install mesa-common-dev`

再次執行，又報錯：cannot find -lGL

執行 `sudo apt-get install libgl1-mesa-dev`

至此，成功

#### 參考資料：

https://blog.csdn.net/u010168781/article/details/80896797
