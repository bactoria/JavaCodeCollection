# Optional

Java Application에서 런타임에 NullPointerException(NPE) 예외가 발생하는 경우가 더럿 있다.

null인 객체의 메서드를 호출하면 NPE가 일어난다.

&nbsp;

**해결법**

위의 런타임에러를 해결하기 위해 아래와 같은 방어로직을 짠다.  

```
if (service != null) {
	service.save(dto);
}
```

여기서의 null 체크 로직은 비즈니스 로직의 가독성을 해친다.

그냥 두면 장애(NPE)가 발생하고.. 해결했더니 코드 가독성이 떨어지고.. 하... 더 좋은 방법 없나?

&nbsp;

**더 나은 해결법**

해답은 함수형 언어에 있었다.

자바에서 "존재하지 않는 값": null

함수형 언어에서 "존재할지 안 할지 모르는 값": Optional

자바8에서 `java.util.Optional<T>`이 추가됨.


### Example

유저이름의 길이를 구하자!

```java
// NPE
String username = getUsername();
int length = username.length(); // username이 Null일 경우 NullPointerException  


// Null
String username = getUsername();
int length;
if (username != null) {
	length = username.length();
} else {
	length = 0;
}


// Optional - Bad!! // null 로직과 마찬가지로 가독성을 해친다! 아니.. 가독성을 더 해친다.
String username = getUsername();
Optional<String> usernameOpt = Optional.ofNullable(username);
int length;
if (usernameOpt.isPresent()) {
	length = usernameOpt.get().length();
} else {
	length = 0;
}


// Optional - Good!! // 가독성 굿
int length = Optional.ofNullable(getUsername()).map(String::length).orElse(0);
```

&nbsp;

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