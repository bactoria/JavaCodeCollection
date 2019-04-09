# Enum

### Basic 

**non-enum**
```java
class State {
    public static final State READY = new STATE();
    public static final State PLAY = new STATE();
    public static final State EXIT = new STATE();
}
```

**Enum**
```java
enum State {
    READY, PLAY, EXIT
}
```

&nbsp;

### Adv

**non-enum**
```java
public class State {
    public static final State READY = new State(2, "준비중");
    public static final State PLAY = new State(1, "플레이~");
    public static final State EXIT = new State(0, "종료!!");

    private final int value;
    private final String message;

    private State(int value, String message) {
        this.value = value;
        this.message = message;
    }

    public static State[] values() {
        return new State[]{READY, PLAY, EXIT};
    }

    public static State findState(int i) {
        return Arrays.stream(State.values())
                .filter(state -> state.value == i)
                .findAny()
                .orElseThrow(() -> new IllegalArgumentException("잘못된 입력 값 :: " + i));
    }

    public String getMessage() {
        return message;
    }
}
```
```java
public class Application {
    public static void main(String[] args) {
        int input = 2;
        State curState = State.findState(input);
        System.out.println(curState == State.READY); // true
        System.out.println(curState.getMessage()); // "준비 중"

        Arrays.stream(State.values()).forEach(System.out::println); // READY PLAY EXIT

        // valueOf(String name) 어떻게 만들지?
    }
}
```

&nbsp;

**enum**
```java
public enum State {
    READY(2, "준비 중"),
    PLAY(1, "플레이~"),
    EXIT(0, "종료!!");

    private final int value;
    private final String message;

    private State(int value, String message) {
        this.value = value;
        this.message = message;
    }

    public static State findState(int i) {
        return Arrays.stream(State.values())
                .filter(state -> state.value == i)
                .findAny()
                .orElseThrow(() -> new IllegalArgumentException("잘못된 입력 값 :: " + i));
    }

    public String getMessage() {
        return message;
    }
}
```
```java
public class Application {
    public static void main(String[] args) {
        int input = 2;
        State curState = State.findState(input);
        System.out.println(curState == State.READY); // true
        System.out.println(curState.getMessage()); // "준비 중"

        Arrays.stream(State.values()).forEach(System.out::println); // READY PLAY EXIT
        System.out.println(State.valueOf("READY")); // READY
    }
}
```
