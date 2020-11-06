QML版本：

使用的qt 版本:5.14.2

在main.cpp中加入

```cpp
qputenv("QT_IM_MODULE", QByteArray("qtvirtualkeyboard"));
```

在要使用的qml file中導入

```qml
import QtQuick.VirtualKeyboard 2.2
```

若要對語系增加或減少則可以再

```qml
import QtQuick.VirtualKeyboard.Settings 2.2
```

virtualkeyboard settings為singleton

使用方式為

`VirtualKeyboardSettings.屬性或方法`

直接使用

------------

在QML中使用`InputPanel`呼叫出virtualkeyboard的實體

寬高大小那些再自己調整吧

