# BigDecimal

```java
BigDecimal bigDecimal;

bigDecimal = new BigDecimal("0.01"); // 0.01
bigDecimal = new BigDecimal(0.01); // 0.0100000000000000002081...
bigDecimal = BigDecimal.valueOf(0.01); // 0.01
```

&nbsp;

```java
BigDecimal num1 = BigDecimal.valueOf(0.1);
BigDecimal num2 = BigDecimal.valueOf(0.10);

num1 == num2; // false - 주소
num1.equals(num2); // false - 0이 붙어서
num1.compareTo(num2); // 0
```

&nbsp;

```java
BigDecimal num1 = BigDecimal.valueOf(0.5);
BigDecimal num2 = BigDecimal.valueOf(0.3);

num1.add(num2); // 0.8

num1.subtract(num2); // 0.2

num1.multiply(num2);  //0.15

num1.divide(num2); // error - 1.66666666~
num1.divide(num2,3, RoundingMode.DOWN); // 1.666
num1.divide(num2,3, RoundingMode.HALF_EVEN); // 1.667
```