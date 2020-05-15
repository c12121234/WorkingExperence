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


#### ================= 3.  Calling c++ from QML : `Q_PROPERTY`

利用`Q_PROPERTY`的MACRO來將c++ module內的成員可被QML使用、改寫

https://doc.qt.io/qt-5/properties.html

常用的屬性有`READ` `WRITE` `NOTIFY` 當然有更多，請看上方連結

QML區塊：

```
Column
    {
        anchors.centerIn: parent
        spacing: 5
        Label
        {
            id:mlblMovie
            anchors.horizontalCenter: parent.horizontalCenter
            font.pixelSize: 25
            text:Movie.title
        }

        Label
        {
            id:mlblMainCharacter
            anchors.horizontalCenter: parent.horizontalCenter
            font.pixelSize: 25
            text:Movie.mainCharacter
        }


        Row
        {
            spacing: 5
            TextField
            {
                id:txtTitle
            }

            Button
            {
                id:btnTitle
                text: "setting title"
                onClicked: Movie.title = txtTitle.text
            }
        }

        Row
        {
            spacing: 5
            TextField
            {
                id:txtCharacter
            }

            Button
            {
                id:btnCharacter
                text: "setting MainCharacter"
                onClicked: Movie.mainCharacter = txtCharacter.text
            }
        }
    }
```

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlQProperty/QProperty001.png)

圖內的title和mainCharacter就是被設定為movie的property 可在QML中呼叫

![img](https://github.com/c12121234/WorkingExperence/blob/master/pic/qmlQProperty/QProperty002.png)


#### ================= 4.  Calling c++ from QML : `SetContextObject`

和setContextProperty()很像 我目前還不知道差在哪...

`engine->rootContext()->setContextObject(&obj);`

#### ================= 5.  Calling jsFunction from c++ : 

```cpp
void JSFunctionCaller::callJSMethod(QString param)
{
    QVariant returnValue;
    QVariant cppParameter = QVariant::fromValue(param);

    QMetaObject::invokeMethod(m_pQMLRootObject,"qmlJSFunction",
                              Q_RETURN_ARG(QVariant,returnValue),
                              Q_ARG(QVariant,cppParameter));
    qDebug()<<"c++ talking,done call QML javascript,the return value is"<<returnValue.toString();
}
```
在cpp中呼叫js function的流程:

將QML的rootobject接收起來，可使用`engine->rootObjects().first()`或`engine->rootObjects()[0]`
(因此 js function要寫在root元件內)

1. 使用`QVariant`來接收 js function的return值以及參數

2. 使用`QMetaObject::invokeMethod`來映射js function的結構 return值使用`Q_RETURN_ARG` 參數使用`Q_ARG` 並且使用`QVariant`外裹一層



