#Java Collection API
1.일반적으로 배열의 성능은 java collections라이브러리의 성능에 미치지 못한다.
예를들어, 배열의 내용을 문자열로 저장하려면 배열을 반복하면서 내용을 하나의 string 으로 연결해야 하는 반면 모든 collections 구현에는 실용적인 String()구현이 있다.

> 빨리 배열을 collection 으로 변환하는 것이 좋다 ?!

2.반복은 비효율 적이다.
배열기반으로 작성된 한 collection의 내용을 다른 collection으로 이동하거나 큰 collection에서 작은 오브젝트를 제거하는 경우는 드물지 않게 발생한다.

####단점
1. 각가의 추가 또는 제거후 collection크기를 조절해야하므로 비효율적이다,
2. 잠금을 획득하고, 작업 수행후 잠금해제시마다 동시성 문제가 발생한다.
3. 추가또는 삭제 작업시 다른 스레드의 경쟁조건이 발생 가능하다.

3.For루프로 Iterable반복하기.
예전에는 Iterator를 가져온후 next()를 사용하여 Iterator에 과정을 수동으로 수행해야 했지만 java5이후 For루프를 자유롭게 사용한다,

##What happens at 'compile time'?
>자바 파일은 자바 컴파일러에 의해 자바코드에서 바이트코드로 convert되어 컴파일링된다.

##What happens at 'Runtime'?
```text
class file ---(java)--> class loader

----> Bytecode verifier // 문법에 맞지않는 코드를 체크한다.

----> interpreter // 다음 실행되는 지침 때, 바이트 코드 스트림을 읽는다. 

----> Runtime ---->HardWare
```