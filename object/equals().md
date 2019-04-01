# equals()

&nbsp;

**논리적 동치**
```java
List<Integer> values1 = new ArrayList<>(Arrays.asList(1,2));
List<Integer> values2 = new ArrayList<>(Arrays.asList(1,2));
values1.equals(values2); // true

int[] values3 = new int[]{1,2};
int[] values4 = new int[]{1,2};
Arrays.equals(values3, values4); // true

```