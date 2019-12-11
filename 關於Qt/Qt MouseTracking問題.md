# Qt MouseTracking問題

想使用此功能時，多半會和override mouseEvent有關

例如移動滑鼠時，碰到某些icon要進行某些行為

移動滑鼠時的事件: void Qwidget::**mouseMoveEvent**(QMouseEvent *event);

在constructor內設置 setMouseTracking(true)後，只要移動就會觸發event

恩，看起來是這樣。

結果用起來不是這樣。

問題是出在MainWindow內有偷藏一個central widget

這個元件也要設定setMouseTracking(true)

就這樣。
