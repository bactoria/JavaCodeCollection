# Clone

&nbsp;

**clone**
```java
@Setter
@Getter
@ToString
@AllArgsConstructor
public final class User implements Cloneable{
    private String name;
    private int value;

    @Override
    public User clone() {
        try {
            return (User) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException();
        }
    }
}

public class Application {
    public static void main(String[] args) {
        User user = new User("준오", 1);
        User clonedUser = user.clone();

        user.setValue(3);

        System.out.println(user); // User(name=준오, value=3)
        System.out.println(clonedUser); // User(name=준오, value=1)
    }
}
```

&nbsp;

**복사 팩터리**
```java
@Setter
@Getter
@ToString
@AllArgsConstructor
class User {
    private String name;
    private int value;

    static User newInstance(User user) {
        return new User(user.name, user.value);
    }
}

public class Clone {
    public static void main(String[] args) {
        User user = new User("준오", 1);
        User clonedUser = User.newInstance(user);

        user.setValue(3);

        System.out.println(user); // User(name=준오, value=3)
        System.out.println(clonedUser); // User(name=준오, value=1)
    }
}
```

&nbsp;

- **이펙티브자바 3/E** Item13. clone 재정의는 주의해서 진행하라 