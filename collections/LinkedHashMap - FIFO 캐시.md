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

```java
// 주의: Thread-Safe 하지 않음
public class FifoCache<K, V> {
    private Map<K, V> CACHE;

    public FifoCache(int maxMapSize) {
        CACHE = new LinkedHashMap<K, V>() {
            @Override
            public boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                return this.size() > maxMapSize; // maxMapSize를 초과하면 제거
            }
        };
    }

    public void put(K k, V v) {
        CACHE.put(k, v);
    }

    public List<V> getAll() {
        return new ArrayList<>((Collection<? extends V>) CACHE.entrySet());
    }
}
```


```java
public class Application {
    public static void main(String[] args) {
        FifoCache<String, String> CACHE = new FifoCache<>(3);

        CACHE.put("Apple", "AppleValue");
        CACHE.put("Amazon", "AmazonValue");
        CACHE.put("Samsung", "SamsungValue");
        CACHE.put("Facebook", "FacebookValue");
        CACHE.put("Google", "GoogleValue");

        System.out.println(CACHE.getAll()); // [SamsungValue, FacebookValue, GoogleValue]
    }
}
```