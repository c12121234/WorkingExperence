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


#### ================= 6.  qmlRegisterUncreatableType : 

用在不能實體化的class，當想要引用到qml中使用時

如某 pure virtual base class，想用此class來描述容器內的type的行為時，就可使用

(多型的運用)

```cpp
//player.h

Q_INVOKABLE virtual void play() =0;
```

```cpp
//main.cpp
//Register Types

qmlRegisterUncreatableType<Player>("com.blikoon.Football", 1,0, "Player",
                                       "Can not create Player in QML.Not allowed.");

    qmlRegisterType<Striker>("com.blikoon.Football", 1,0, "Striker");
    qmlRegisterType<Defender>("com.blikoon.Football", 1,0, "Defender");
    qmlRegisterType<FootBallTeam>("com.blikoon.Football", 1,0, "FootBallTeam");
```

```
//main.qml
Component.onCompleted: {
        console.log("We have :" + team2.players.length + " players in the team "+ team2.title)
        for(var i = 0; i<team2.players.length;++i)
        {
            team2.players[i].play()
        }
    }
}
```

#### ================= 7.  Q_CLASSINFO  Default Property: 

很難說明 大略使用方式是`Q_CLASSINFO("DefaultProperty","屬性名")`

而可以在QML中預設使用這個屬性(新建type時就是預設這屬性)

```
FootBallTeam {
        id : team1
        title: "Rayon Sports"
        coatch: "Coatch Name"
        captain: Striker{
            name: "Captain"
            position: "Middle Field"
            playing: true
        }

        players: [

            Defender{
                name: "Player1"
                position: "Middle Field"
                playing: true
            },
            Striker{
                name: "Player2"
                position: "Middle Field"
                playing: true
            },
            Defender{
                name: "Player3"
                position: "Middle Field"
                playing: true
            },
            Striker{
                name : "Daniel"
                position: "None"
                playing: false
            }



        ]
    }

    FootBallTeam {
        id : team2
        title: "APR"
        coatch: "Coatch Name"
        captain: Striker{
            name: "Captain"
            position: "Middle Field"
            playing: true
        }

        Defender{
            name: "Player4"
            position: "Middle Field"
            playing: true
        }
        Striker{
            name: "Player5"
            position: "Middle Field"
            playing: true
        }
        Defender{
            name: "Player6"
            position: "Middle Field"
            playing: true
        }
        Striker{
            name : "Daniel2"
            position: "None"
            playing: false
        }
    }
```

team1是原本用法，可看到設定了players這屬性

team2則不需要，就差在這。

#### ================= 8.  物件的focus屬性: 

若想要捕捉鍵盤的pressed和release signal

則該物件的focus必須設為true

```cpp
Column
    {
        anchors.centerIn: parent
        spacing: 5
        focus: true
        Keys.onPressed:
        {
            switch(event.key)
            {
            case Qt.Key_0:
                console.log("0")
                mPressNum0.border.color = "red"
                break
            case Qt.Key_1:
                console.log("1")
                mPressNum1.border.color = "red"
                break
            case Qt.Key_2:
                console.log("2")
                mPressNum2.border.color = "red"
                break
            case Qt.Key_3:
                console.log("3")
                mPressNum3.border.color = "red"
                break
            case Qt.Key_4:
                console.log("4")
                mPressNum4.border.color = "red"
                break
            case Qt.Key_5:
                console.log("5")
                mPressNum5.border.color = "red"
                break
            case Qt.Key_6:
                console.log("6")
                mPressNum6.border.color = "red"
                break
            case Qt.Key_7:
                console.log("7")
                mPressNum7.border.color = "red"
                break
            case Qt.Key_8:
                console.log("8")
                mPressNum8.border.color = "red"
                break
            case Qt.Key_9:
                console.log("9")
                mPressNum9.border.color = "red"
                break
            default:
                break
            }
        }
```

注意onPressed這slot，它接收一個event

所以可在onPressed內部使用event

額外注意，若event還想讓別的handle處理

則必須在onPressed內部設定 `event.accepted = false`

#### ================= 9.  重疊的mouseArea之滑鼠事件傳遞: 

在QML中，物件順序是程式碼越下方的越晚create，若兩塊mouseArea重疊

則晚create的會在上方覆蓋下方的mouseArea。因此若要讓滑鼠事件"**穿透**"到下方的mouseArea的話

則必須設定`propagateComposedEvents: true` 而且較上層的滑鼠事件也必須設定 `mouse.accepted = false`

