# deletelater的用法

普通指標刪除的用法是

```cpp
void foo()
{
    delete m_ptr;
    m_ptr = nullptr;
}
```

在delete後要把原指標賦值nullptr，delete後只是釋放記憶體，但還是指向某個未知記憶體區塊

所以必須給一個null值來確保下次使用時不會出狀況

ex:

```cpp
if(m_ptr)
{
//...
}
```

如果沒給予null值，經過delete後m_ptr**是**可以進入此if區塊的，那就糟糕了...

因此 回到Qt的deletelater上，若要刪除某個指標並且賦值null的話，該怎麼做呢？

```cpp
connect(objAptr,&widgetA::disconnect,ptr,&QObject::deleteLater);
connect(objAptr,&widgetA::disconnect,objAptr,&widgetA::clearptr);

void widgetA::clearptr()
{
    ptr = nullptr;
}
```

註:disconnect視為某個結束訊號 
