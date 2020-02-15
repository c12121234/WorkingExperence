# template的宣告及實作分離

## 概述

過去學的概念是，template的宣告和實作要一起放在.h裡面

當然是一定能編譯，但當你只想修個一行卻得要編10分鐘 這顯然很不合理

於是找到了個方法 `explicit instantiation` 噹噹!

## 作法

標頭檔

來個範例(註解掉qt的相關機制)

```cpp
#ifndef OUTPUT_H
#define OUTPUT_H

#include <QObject>


template<typename T>
class OutPut
{
    //Q_OBJECT
public:
    explicit OutPut();
    T DoOutPut();

//signals:

};

#endif // OUTPUT_H

```
實作

```cpp
#include <QDebug>
#include "output.h"


template<typename T>
OutPut<T>::OutPut()
{
    qInfo()<<"general constructor";
}

template<>
OutPut<int>::OutPut()
{
    qInfo()<<"int constructor";
}

template<>
OutPut<double>::OutPut()
{
    qInfo()<<"double constructor";
}

/*
template<>
OutPut<long>::OutPut()
{
    qInfo()<<"long constructor";
}*/

template<typename T>
T OutPut<T>::DoOutPut()
{
    qInfo()<<"this is general template.";
    return 10;
}

template<>
int OutPut<int>::DoOutPut()
{
    qInfo()<<"this is int template.";
    return 100;
}

template<>
double OutPut<double>::DoOutPut()
{
    qInfo()<<"this is double template.";
    return 10.8;
}

template OutPut<long>::OutPut();
template long OutPut<long>::DoOutPut(void);

```

main.cpp

```cpp
#include <QCoreApplication>
#include <QDebug>
#include "output.h"
int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    OutPut<long> lout;
    OutPut<int> iout;
    OutPut<double> dout;
    qInfo()<<"lout:"<<lout.DoOutPut();
    qInfo()<<"iout:"<<iout.DoOutPut();
    qInfo()<<"dout:"<<dout.DoOutPut();
    return a.exec();
}

```

## 說明

在`main.cpp`中，OutPut樣板調用了`long` `int` `double` 

`int`和`double`偏特化，而`long`則屬於一般template

因此，偏特化的`constructor`和`DoOutPut()`都得**明確**寫出來

一般的則只要在底下`explicit instantiation`

即:
```cpp
template OutPut<long>::OutPut();
template long OutPut<long>::DoOutPut(void);
```

寫出這兩行 long使用到的ctor和function即可

## 輸出
![img](picture)
