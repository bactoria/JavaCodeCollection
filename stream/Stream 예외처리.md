# Stream 예외처리

&nbsp;

```java
// Bad. Using a try/catch block
public static void main(String[] args) {
    List<String> clonedUsers = users.stream()
        .map(user -> {
            try {
                return user.clone();
            } catch (CloneNotSupportedException e) {
                e.printStackTrace();
            }
            return null;
        })
        .filter(Objects::nonNull)
        .map(User::getName)
        .collect(Collectors.toList());

// Better. 예외 발생시, 예외 처리할 것이 있을 경우 사용
public static void main(String[] args) {
    List<String> clonedUsers = users.stream()
        .map(Main::cloneUser)
        .filter(Objects::nonNull)
        .map(User::getName)
        .collect(Collectors.toList());
}

private static User cloneUser(User user) {
    try {
        return user.clone();
    } catch (CloneNotSupportedException e) {
        e.printStackTrace();
    }
    return null;
}


// Good. 예외 발생시, 예외 처리할 것이 없을 경우 사용 (checked exceptions를 unchecked exceptions 로 처리함)
public static void main(String[] args) {
    List<String> clonedUsers = users.stream()
            .map(wrapper(User::clone))
            .map(User::getName)
            .collect(Collectors.toList());
}

@FunctionalInterface
public interface FunctionWithException<T, R, E extends Exception> {
    R apply(T t) throws E;
}

private static <T, R, E extends Exception> 
    Function<T, R> wrapper(FunctionWithException<T, R, E> fe) {
        return arg -> {
            try {
                return fe.apply(arg);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        };
}
```

- [Handling checked exceptions in Java streams](https://www.oreilly.com/ideas/handling-checked-exceptions-in-java-streams)