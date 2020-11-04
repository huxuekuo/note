

# new HashMap<>()







# put method



首先判断表是否未初始化, 如果没有初始化默认初始化容量为`16`具体代码

```java
  if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
```



然后判断table表的hash位置是否为null, 如果为空新增一个节点

```java
   if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
```



如果表的中hash位置不为空, 判断旧的k是否与新的k一致, 如果一致覆盖old的val,

如果key不一致,判断旧的节点是不是链表节点, 如果不是 创建链表, 如果是在链表中添加数据



# get method

