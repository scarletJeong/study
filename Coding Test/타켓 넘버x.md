`2024-12-13`

##### 깊이/너비 우선 탐색(DFS/BFS)

1. 배열[ ] 정렬
		Arrays.sort(numbers, Collections.reverseOrder());
		Arrays.sort(numbers);
		

```
/Solution.java:6: error: no suitable method found for sort(int[],Comparator<Object>)  
Arrays.sort(numbers, Collections.reverseOrder());  
^  
method Arrays.<T#1>sort(T#1[],Comparator<? super T#1>) is not applicable  
(inference variable T#1 has incompatible bounds  
equality constraints: int  
lower bounds: Object)

```

오류는 Java의 `int[]` 배열이 기본 타입 배열이므로, `Comparator`를 사용할 수 없기 때문에 발생합니다. `Arrays.sort()`와 `Collections.reverseOrder()`는 객체 배열에만 적용됩니다. 이를 해결하려면 기본 타입 배열을 **래퍼 클래스 배열(`Integer[]`)로 변환**해야 합니다.


