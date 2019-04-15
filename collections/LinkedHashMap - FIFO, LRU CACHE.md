# LinkedHashMap - FIFO 캐시

&nbsp;

**java.util.LinkedHashMap**
```java
public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V> {
    
    //...

    protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
        return false;
    }
}
```

&nbsp;

## FIFO CACHE

```java
// 주의: Thread-Safe 하지 않음
public class FifoCache<K, V> {
    private Map<K, V> CACHE;

    public FifoCache(int maxMapSize) {
        CACHE = new LinkedHashMap<K, V>() { // parameter 없음.
            @Override
            public boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                return this.size() > maxMapSize; // maxMapSize를 초과하면 제거
            }
        };
    }

    public void put(K k, V v) {
        CACHE.put(k, v);
    }
}
```


```java
public class Application {
    public static void main(String[] args) {
        FifoCache<String, String> FIFO_CACHE = new FifoCache<>(3);

        FIFO_CACHE.put("Apple", "AppleValue"); // [ Apple ]
        FIFO_CACHE.put("Amazon", "AmazonValue"); // [ Amazon, Apple ]
        FIFO_CACHE.put("Samsung", "SamsungValue"); // [ Samsung, Amazon, Apple ]

        FIFO_CACHE.get("Apple"); // [ Samsung, Amazon, Apple ]
        FIFO_CACHE.put("Facebook", "FacebookValue"); // [ Facebook, Samsung, Amazon ]

        System.out.println(FIFO_CACHE.get("Apple"));  // null
    }
}
```

&nbsp;

## LRU CACHE

```java
public class LRUCache<K, V> {
    private Map<K, V> CACHE;

    public LRUCache(int maxMapSize) {
        CACHE = new LinkedHashMap<K, V>(maxMapSize * 4 / 3 + 1, 0.75f, true) { // parameter 없으면 FIFO
            @Override
            public boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                return this.size() > maxMapSize;
            }
        };
    }

    public void put(K k, V v) {
        CACHE.put(k, v);
    }

    public V get(K k) {
        return CACHE.get(k);
    }
}
```

- [0.75f 를 붙이는 이유](https://darksilber.tistory.com/entry/LinkedHashmap-LRU-Caching)

```java
public class Application {
    public static void main(String[] args) {
        LRUCache<String, String> LRU_CACHE = new LRUCache(3);
        LRU_CACHE.put("Apple", "AppleValue"); // [ Apple ]
        LRU_CACHE.put("Amazon", "AmazonValue"); // [ Amazon, Apple ]
        LRU_CACHE.put("Samsung", "SamsungValue"); // [ Samsung, Amazon, Apple ]

        LRU_CACHE.get("Apple"); // [ Apple, Samsung, Amazon ]
        LRU_CACHE.put("Facebook", "FacebookValue"); // [ Facebook, Apple, Samsung ]

        System.out.println(LRU_CACHE.get("Apple")); // "AppleValue"
    }
}
```
