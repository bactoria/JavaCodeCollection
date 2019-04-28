# flatmap은 lazy?

&nbsp;

```java
Stream.iterate(0, i -> i + 1)
    .flatMap(i -> Stream.of(i, i, i))
    .map(i -> i + 1)
    .peek(i -> System.out.println("Map: " + i))
    .limit(4)
    .forEach(i -> {});
```

&nbsp;

###result

**java8**
```java
// flatMap은 lazy하지 않다.
Map: 1
Map: 1
Map: 1
Map: 2
Map: 2
Map: 2
```

**java10**
```java
// flatMap은 lazy하다.
Map: 1
Map: 1
Map: 1
Map: 2
```

- [Are Java 8 streams truly lazy? Not completely!](https://jaxenter.com/java-8-streams-lazy-136183.html)