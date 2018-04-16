##codility01

https://codility.com/c/run/demoSXECZZ-GEB#final-form

This is a demo task.

Write a function:

> `class Solution { public int solution(int[] A); }`

that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = [1, 3, 6, 4, 1, 2], the function should return 5.

Given A = [1, 2, 3], the function should return 4.

Given A = [−1, −3], the function should return 1.

Assume that:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [−1,000,000..1,000,000].

Complexity:

> - expected worst-case time complexity is O(N);
> - expected worst-case space complexity is O(N), beyond input storage (not counting the storage required for input arguments).



## Point `HashSet`

![javaCollection](/Users/jeonghyeonji/Desktop/javaCollection.jpg)



| **인터페이스** | **구현 클래스 **                              | **특징 **                                  |
| --------- | ---------------------------------------- | ---------------------------------------- |
| **List**  | `LinkedList` `Stack` `Vector` `ArrayList` | 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.           |
| **Set**   | `HashSet` `TreeSet`                      | 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다.  |
| **Map **  | `HashMap` `TreeMap` `HashTable` `Properties` | 키(key)와 값(value)의 쌍으로 이루어진 데이터의 집합이다. 순서는 유지되지 않고, 키는 중복을 허용하지 않으며 값의 중복을 허용한다.출처: |

### => `HashSet` 중복을 허용하지 않는다. 순서를 유지하지 않는다.



```java
import java.util.HashSet;

class Solution {
    public int solution(int[] A) {
        int num = 1;
        HashSet<Integer> hashSet = new HashSet<Integer>();

        for (int i = 0 ; i < A.length; i++) {
            hashSet.add(A[i]);                     
        }

         while (hashSet.contains(num)) {
                num++;
            }

        return num;
    }
}
```

