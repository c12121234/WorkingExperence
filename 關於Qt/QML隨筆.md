# 隨便紀錄

#### ================= 1.  Calling c++ from QML : `ContextProperty`

要點：

1. class物件繼承**QObject**

2. 想被喚起的member function 需要使用 **Q_INVOKABLE**

3. 除2. 外，還可將想被喚起的function 設定為 **public slots function**

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlContextProperty/ContextProperty001.png)

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlContextProperty/ContextProperty002.png)

4. include `<QQmlContext>`

5.實體化你的c++模組 class 並使用**QQmlApplicationEngine**產生的實體 呼叫 *SetContextProperty()*

```cpp
engine->rootContext()->SetContextProperty("在QML中使用的Type",&myModel);
```
![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlContextProperty/ContextProperty003.png)

6. 在QML中 使用`Type.function` 即可

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlContextProperty/ContextProperty004.png)


#### ================= 2. QML Connections type用法：

~~一種可將connect 不一定寫在child內的寫法~~

主要目的是要接收c++ module內的signal 能在QML中使用，見第二張圖

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlConnections/Connections001.png)

就一種增加彈性的寫法吧，可能可以將一大堆Connection寫在一起方便管理，到處散落要追也不方便。

在QML中，signal前面+`on` 就有隱含連接signal slot 的意思了 所以在button中直接寫`onClicked`

就已經將接收click signal 並執行相對應的slot這行為表現出來了

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlConnections/Connections002.png)


