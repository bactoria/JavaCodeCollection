# First-class citizen

### First-class citizen 의 조건
1. 파라미터로 사용 가능해야 한다.
2. 리턴 사용 가능해야 한다.
3. 변수에 할당 가능해야 한다.

&nbsp;

### Object is First-class citizen
```java
Lotto lotto1 = new Lotto();

calculateRank(lotto1); // 가능 (1)
Lotto lotto2 = makeLotto(); // 가능 (2)
Lotto lotto3 = lotto2; // 가능 (3)

=> 가능
```

&nbsp;

### Method is not First-class citizen
```java
public Lotto getLotto() { ... }

calculateRank(getLotto); // 불가능 (1)
public getLotto doSomething() { return getLotto; } // 불가능 (2)
SomeMethod m = getLotto; // 불가능 (3)

=> 불가능
```

&nbsp;

### 자바8 - First-class Function 지원
```java
@FunctionalInterface
interface Operation<T, U, R> {
    R operate(T t, U u);
}

public class Application {
    public static void main(String[] args) {
        int result;

        Operation<Integer, Integer, Integer> plus = (x, y) -> x + y;
        Operation<Integer, Integer, Integer> plusCopy = plus;
        result = plusCopy.operate(10, 30)); // 가능 (3)
        System.out.println(result); // 40

        result = operate(20, plus, 30)); // 가능 (1)
        System.out.println(result); // 50

        result = plusMethod().operate(40, 30)); // 가능 (2)
        System.out.println(result); // 70
    }

    private static Integer operate(int x, Operation<Integer, Integer, Integer> operation, int y) {
        return operation.operate(x, y);
    }

    private static Operation<Integer, Integer, Integer> plusMethod() {
        return (o1, o2) -> o1 + o2;
    }
}
```

