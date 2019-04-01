# Sort

&nbsp;


```java
// 1. Array (unboxing)
int[] array1 = {1, 2, 3};

Arrays.sort(array1); // [1, 2, 3]
Arrays.sort(array1, Comparator.reverseOrder()); // Compile Error!!
Arrays.asList(array1).sort((o1, o2) -> o2 - o1); // Compile Error!!


// 2. Array (boxing)
Integer[] array = {4, 5, 6};

Arrays.sort(array); // [4, 5, 6]
Arrays.sort(array, Collections.reverseOrder()); // [6, 5, 4]
Arrays.sort(array, (o1, o2) -> o2 - o1); // [6, 5, 4]


// 3. List
List<Integer> list = new ArrayList<>(Arrays.asList(7,8,9));

Collections.sort(list); // [7, 8, 9]
Collections.reverse(list); // [9, 8, 7]
list.sort(Collections.reverseOrder()); // [9, 8, 7]
list.sort((o1, o2) -> o2 - o1); // [9, 8, 7]
```

