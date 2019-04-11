# Enum

## Basic 

**non-enum**
```java
class State {
    public static final State READY = new STATE();
    public static final State PLAY = new STATE();
    public static final State EXIT = new STATE();
}
```

&nbsp;

**enum**
```java
enum State {
    READY, PLAY, EXIT
}
```

&nbsp;
&nbsp;

## Adv

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

&nbsp;
&nbsp;

## abstract Method

```java
// 이펙티브자바 <Item 34>
enum Operation {
    PLUS("+") {
        public double apply(double x, double y) { return x + y; }
    },
    MINUS("-") {
        public double apply(double x, double y) { return x - y; }
    },
    TIMES("*") {
        public double apply(double x, double y) { return x * y; }
    },
    DIVIDE("/") {
        public double apply(double x, double y) { return x / y; }
    };

    private final String symbol;

    Operation(String symbol) { this.symbol = symbol; }

    @Override public String toString() { return symbol; }

    public abstract double apply(double x, double y);
}

public class AbstractMethod {
    public static void main(String[] args) {
        double x = 1.1;
        double y = 3.3;
        for (Operation op : Operation.values())
            System.out.printf("%f %s %f = %f%n",
                    x, op, y, op.apply(x, y));

        // 1.100000 + 3.300000 = 4.400000
        // 1.100000 - 3.300000 = -2.200000
        // 1.100000 * 3.300000 = 3.630000
        // 1.100000 / 3.300000 = 0.333333
    }
}
```

&nbsp;

**`Functional Interface`를 이용한 리팩토링**
```java
// 이펙티브자바 <Item 42>
enum Operation {
    PLUS("+", (x, y) -> x + y),
    MINUS("-", (x, y) -> x - y),
    TIMES("*", (x, y) -> x * y),
    DIVIDE("/", (x, y) -> x / y);

    private final String symbol;
    private final DoubleBinaryOperator op;

    Operation(String symbol, DoubleBinaryOperator op) {
        this.symbol = symbol;
        this.op = op;
    }

    public double apply(double x, double y) {
        return op.applyAsDouble(x, y);
    }

    @Override
    public String toString() { return symbol; }
}
```
