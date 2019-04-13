# Function

## java.util.function

### UnaryOperator<T>
```java
//      UnaryOperator<Long> unaryOperator = x -> x + 4; // Bad (return Boxing Data)
        LongUnaryOperator longUnaryoperator = x -> x + 4; // Good (unBoxing Data)
        System.out.println(longUnaryoperator.applyAsLong(10)); // 14

//      UnaryOperator<Double> unaryOperator = x -> x + 4;
        DoubleUnaryOperator doubleUnaryOperator = x -> x + 4;
        System.out.println(doubleUnaryOperator.applyAsDouble(10.5)); // 14.5

//      UnaryOperator<Integer> unaryOperator = x -> x + 4;
        IntUnaryOperator intUnaryOperator = x -> x + 4;
        System.out.println(intUnaryOperator.applyAsInt(10)); // 14
```

### BinaryOperator<T>
```java
//      BinaryOperator<Double> doubleBinaryOperator = (x, y) -> x + y;
        DoubleBinaryOperator doubleBinaryOperator = (x, y) -> x + y;
        System.out.println(doubleBinaryOperator.applyAsDouble(3.0, 2.5)); // 5.5

//      BinaryOperator<Integer> intBinaryOperator = (x, y) -> x + y;
        IntBinaryOperator intBinaryOperator = (x, y) -> x + y;
        System.out.println(intBinaryOperator.applyAsInt(3, 2)); // 5

//      BinaryOperator<Long> longBinaryOperator = (x, y) -> x + y;
        LongBinaryOperator longBinaryOperator = (x, y) -> x + y;
        System.out.println(longBinaryOperator.applyAsLong(3, 2)); // 5
```

### Predicate<T>
```java
        BiPredicate<Integer, Double> biPredicate = (x, y) -> x > y;
        System.out.println(biPredicate.test(4, 4.2)); // false

//      Predicate<Double> doublePredicate = x -> x > 5;
        DoublePredicate doublePredicate = x -> x > 5;
        System.out.println(doublePredicate.test(4.2)); // false

//      Predicate<Integer> intPredicate = x -> x > 5;
        IntPredicate intPredicate = x -> x > 5;
        System.out.println(intPredicate.test(6)); // true

//      Predicate<Long> longPredicate = x -> x > 5;
        LongPredicate longPredicate = x -> x > 5;
        System.out.println(longPredicate.test(4)); // false
```

### Function<T, R>
```java
        BiFunction<String, Integer, String> biFunction = String::substring;
        System.out.println(biFunction.apply("abcde", 2)); // "cde"

//      Function<Double, Integer> doubleToIntFunction = x -> (int) Math.floor(x);
//      ToIntFunction<Double> doubleToIntFunction = x -> (int) Math.floor(x);
//      DoubleFunction<Integer> doubleToIntFunction1 = x -> (int) x;
        DoubleToIntFunction doubleToIntFunction = x -> (int) x;
        System.out.println(doubleToIntFunction.applyAsInt(10.7)); // 10

// ... 종류 너무 많다
```

### Supplier<T>
```java
//      Supplier<Integer> intSupplier = () -> 6;
        IntSupplier intSupplier = () -> 6;
        System.out.println(intSupplier.getAsInt()); // 6

        Supplier<String> nameSupplier = ()-> "bactoria";
        System.out.println(nameSupplier.get()); // bactoria
```
### Consumer<T>
```java
        Consumer<String> consumer = System.out::println;
        consumer.accept("안녕?"); // "안녕?"
```

&nbsp;

- [Why doesn't Java 8's Predicate<T> extend Function<T, Boolean>
](https://stackoverflow.com/questions/22682836/why-doesnt-java-8s-predicatet-extend-functiont-boolean)