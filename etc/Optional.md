# Optional

### Init
```java
public static void main(String[] args) {
    Optional<String> strNotNull;
    strNotNull = "ee"; // compile error!
    strNotNull = Optional.ofNullable("ee"); // bad
    strNotNull = Optional.of("ee"); // best

    Optional<String> strNull;
    strNull = Optional.of(null); // bad
    strNull = Optional.ofNullable(null); // better
    strNull = Optional.empty(); // best
}
```

&nbsp;

### Use
```java
@ToString
@Getter
@AllArgsConstructor
class Car {
    private String name;
}

public class Application {
    public static void main(String[] args) {
        Optional<Car> carNotNull = Optional.of(new Car("sonata"));

        System.out.println(carNotNull); // Optional[Car(name=sonata)]
        System.out.println(carNotNull.isPresent()); // true
        carNotNull.ifPresent(System.out::println); // Car(name=sonata) (null이 아닐 경우, consumer)
        System.out.println(carNotNull.orElse(new Car(""))); // Car(name=sonata) (null 일 경우 이름이 없는 Car 객체 생성)
        System.out.println(carNotNull.filter(c -> c.getName().equals("genesis"))); // Optional.empty (조건을 만족시키지 않으면 empty로 만들어버림)
        System.out.println(carNotNull.filter(c -> c.getName().equals("sonata"))); // Optional[Car(name=sonata)]
        carNotNull.orElseThrow(IllegalStateException::new); // null 일 경우 예외처리


        Optional<Car> carNull = Optional.empty();

        System.out.println(carNull); // Optional.empty
        System.out.println(carNull.isPresent()); // false
        carNull.ifPresent(System.out::println); //
        System.out.println(carNull.orElse(new Car(""))); // Car(name=) (null 일 경우 이름이 없는 Car 객체 생성)
        carNull.orElseThrow(IllegalStateException::new); // java.lang.IllegalStateException
    }
}

```

&nbsp;

### Collection
```java
public class Application {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, null, 2, 3, 4, 5, null);

        Optional.ofNullable(numbers) // Optional<List<Integer>>
                .map(Collection::stream) // Optional<Stream<Integer>>
                .orElseGet(Stream::empty) // Stream<Integer> (1, null, 2, 3, 4, 5, null)
                .filter(Objects::nonNull) // Steram<Integer> (1, 2, 3, 4, 5)
                .forEach(System.out::print); // 12345
    }
 }
```