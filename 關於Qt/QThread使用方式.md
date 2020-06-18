# QThread使用方式

[連結](https://blog.csdn.net/an505479313/article/details/50351745)

節錄部份內容:

QueuedConnection:slot函數在接收者所依附執行緒執行。

DirectConnection:slot函數在發送信號的執行緒執行。

autoConnection:兩object同執行緒，如directConnection 不同執行緒則如同QueuedConnection

須注意:Qthread所在執行緒，和所**管理**的執行緒不同

所在執行緒會在create Qthread那行時，所處的執行緒中

例如:

```cpp
int main()
{
    QThread t1; // <--t1所在的執行緒為main thread
    QThread* pt2 = new QThread(); // pt2同上
    
    ClassA a;
    ClassB b;
    
    a.MoveToThread(&t1); //a會在子執行緒t1上
    b.MoveToThread(pt2);//b會在子執行緒pt2上
}

```

而若是使用繼承QThread 複寫*run*函式的方式的話

只有run函式內處於子執行緒

slot函數仍然處於創建時thread時的執行緒

