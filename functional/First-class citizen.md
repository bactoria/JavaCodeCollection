# First-class citizen

### First-class citizen 의 조건
1. 파라미터로 사용 가능해야 한다.
2. 리턴 사용 가능해야 한다.
3. 변수에 할당 가능해야 한다.

&nbsp;

### Object is First-class citizen
```java
public class Application {
    public static void main(String[] args) {
        Lotto lotto = new Lotto();

        Rank rank = calculateRank(lotto); // 가능 (1)
        lotto = makeLotto(); // 가능 (2)
        Lotto lottoCopy = lotto; // 가능 (3)
    }

    private static Rank calculateRank(Lotto lotto) {
        return new Rank(lotto);
    }

    private static Lotto makeLotto() {
        return new Lotto();
    }
}
```

&nbsp;

### Method is not First-class citizen
```java
public class Application {
    public static void main(String[] args) {
        calculateRank(getLotto); // 불가능 (1)
        public getLotto doSomething() { return getLotto; } // 불가능 (2)
        SomeMethod m = getLotto; // 불가능 (3)
    }

    private static Lotto getLotto() { 
        return new Lotto() 
    }
}
```

&nbsp;

### 자바8 - First-class Function 지원
```java
import java.util.function.IntBinaryOperator;

public class Application {
    public static void main(String[] args) {
        int result;
        IntBinaryOperator plus = (x, y) -> x + y;

        IntBinaryOperator plusCopy = plus; // 가능 (3)
        result = plusCopy.applyAsInt(10, 30);
        System.out.println(result); // 40

        result = operate(20, plus, 30); // 가능 (1)
        System.out.println(result); // 50

        result = plusMethod().applyAsInt(40, 30); // 가능 (2)
        System.out.println(result); // 70
    }

    private static Integer operate(int x, IntBinaryOperator operator, int y) {
        return operator.applyAsInt(x, y);
    }

    private static IntBinaryOperator plusMethod() {
        return (o1, o2) -> o1 + o2;
    }
}
```

