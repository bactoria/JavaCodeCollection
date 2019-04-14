# groupingBy

```java
public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3, 1, 4);

    Map<Object, List<Integer>> a = list.stream().collect(Collectors.groupingBy(x -> x));
    System.out.println(a.get(1)); // [1, 1]
    System.out.println(a.get(3)); // [3]   
}
```

```java
public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3, 1, 4);

    Map<Integer, Long> groupCount = list.stream().collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    System.out.println(groupCount.get(1)); // 2
    System.out.println(groupCount.get(2)); // 1
}
```

```java
public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3, 1, 4);

    Map<Object, List<Integer>> groupIsOdd = list.stream().collect(Collectors.groupingBy(GroupingBy::isOdd));
    System.out.println(groupIsOdd.get(true)); // [1, 3, 1]
    System.out.println(groupIsOdd.get(false)); // [2, 4]
}

private static boolean isOdd(int x) {
    return x % 2 == 1;
}
```

```java
public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3, 1, 4);

//  Map<Integer, Long> groupIsOdd = list.stream().collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    Map<Boolean, Long> groupIsOdd = list.stream().map(Application::isOdd).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    System.out.println(groupIsOdd.get(true)); // 3
    System.out.println(groupIsOdd.get(false)); // 2
}

private static boolean isOdd(int x) {
    return x % 2 == 1;
}
```