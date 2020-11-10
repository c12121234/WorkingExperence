# 應用程式打包發布工具

20201106 更新:

都在下面連結了

https://www.bilibili.com/s/video/BV1aK4y1v7fq

參考連結

https://blog.csdn.net/weixin_30548917/article/details/98513031#t1

=======================================

由於開發QT應用程式

使用兩套第三方套件 AppImage 以及linuxdeployqt

連結: 

https://github.com/probonopd/linuxdeployqt

https://github.com/AppImage/AppImageKit

使用方法：

下載linuxdeployqt的releases版本(linuxdeployqt-6-x86_64.AppImage 這個檔案)

一樣下載AppImage的執行檔 (https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage)

將兩個檔案都改成可執行

將你專案build好的binary檔隨便移到某乾淨資料夾 稱為`source路徑`+ binary檔的名稱

AppImage的執行檔稱為`AppImage路徑`

linuxdeployqt的執行檔稱為`linuxdeployqt路徑`

*注意* 你的qt kit的qmake檔所在路徑 稱為`qmake路徑`

以我目前的環境舉例:

qmake路徑: /home/desktop-tom/newQt/5.14.2/gcc_64/bin/qmake 

編譯時我使用的kit是gcc_64 所以到gcc64的bin中找對應的qmake


執行指令:

`linuxdeployqt路徑  source路徑  -qmake=qmake路徑 -unsupported-allow-new-glibc`

**若使用qml 則必須增加 -qmldir=path的路徑 path為你的專案資料夾**

**linuxdeployqt路徑  source路徑  -qmldir=path -qmake=qmake路徑 -unsupported-allow-new-glibc**

 -unsupported-allow-new-glibc 這指令是為了修正18.04版後的問題 詳見連結：https://github.com/probonopd/linuxdeployqt/issues/340
 
 好了後會在你的source路徑下跑出很多檔案 如下
 
 ![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/d001-1.png)
 
 styledemo是你的source檔 而我們新增一個styledemo.desktop檔的文件
 
 新增內容如下
 
 ![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/d002.png)
 
 image001檔名是你想要使用的icon 在文件styledemo.desktop中不需要+副檔名
 
 好了後去`AppImage路徑` cmd下指令
 
 `./ appimagetool-x86_64.AppImage source路徑的上一層 以圖片來說就是testdeploy` 
 
 打包完成，打包完後的檔案會產出在AppImage路徑下 對你的執行檔下ldd 無動態連結 yes
 
 
**ldd 若有動態連結庫找不到的情況，則 export LD_LIBRARY_PATH=/Path/To/Lib:$LD_LIBRARY_PATH然後再次執行ldd確定即可.**


新增:QML在windows下的打包

https://blog.csdn.net/baidu_33850454/article/details/88597740



