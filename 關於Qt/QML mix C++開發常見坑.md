# QML mix C++開發常見坑

把碰到的問題紀錄起來，唉


## C++模組內的signal在QML內無反應:

附上stackoverflow:

https://stackoverflow.com/questions/24406333/qml-using-cpp-signal-in-qml-always-results-in-cannot-assign-to-non-existent-pr


簡言之，你的signal命名第一個字不能大寫

在QML中的Signal命名+on 然後第一個字改大寫

......我無言

範例:

```cpp
signals:
    void sendNotice(QVariant varData);
```
在QML中，觸發命名要改成`onSendNotice` 

而且assign給QML的變數，也一定要使用`varData`才能有效接到

還有，傳的參數要使用QVariant的type 很多阿雜的坑阿...





