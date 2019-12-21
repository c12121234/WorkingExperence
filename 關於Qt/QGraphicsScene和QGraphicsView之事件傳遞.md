# QGraphicsScene和QGraphicsView之事件傳遞

因為這問題卡了好幾小時，紀錄一下。

事件接收順序是這樣：

QGraphicsView發出event->傳到QGraphicsScene，而QGraphicsScene接到後將其傳出->在Mainwindow內接到，作相對應的handle.

以滑鼠點擊事件為例，QGraphicsView上的MousePressEvent傳出，在Scene接到後會以QGraphicsSceneMouseEvent來接收

(event事件中的 QEvent::GraphicsSceneMousePress) 。

最後MainWindow接到的是**MouseEvent** 而不是**QGraphicsSceneMouseEvent**

很怪對吧。所以卡好久= =

我是在Mainwindow使用 event filter來接收event 並做處理

但撈到的是mouseevent，沒辦法處理scene上的特殊資料

做了static_cast 想當然爾是直接crash

印log也是正常 view的event先 在來scene的event 明明scene的event有發出卻一直收不到

提一下為什麼一定得要到QGraphicsSceneMouseEvent

因為view的座標系和scene的坐標系不一樣，而view的物件座標還會因視窗大小改變

(應該是說，如果你某個物件在view x:3 y:5 把mainwindow full sceen後 該物件的座標就不會在3,5了，而scene沒這問題)

原本架構是在mainwindow的event filter接收資料後，利用signal傳到內部的controller類別去處理

event filter的用法是，選擇你要監聽的物件(這裡以QObject*的形式)

在Qt架構下，所有class都是QOBJECT

一開始以為是監聽對象選錯，我是選擇監聽View

但我改監聽scene一樣無消無息。

最後是發現原來scene本身也可以安裝event filter(view則不行)

改在scene監聽消息就能正確接收到QGraphicsSceneMouseEvent了 可喜可賀 可喜可賀
