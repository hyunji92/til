#함수형 프로그래밍 기법
##14.1 함수는 모든 곳에 존재한다.
> 함수형 프로그래밍이란 함수나 메서드가 수학의 함수처럼 부작용없이 동작함을 의미한다. 즉 함수를 마치 일반값처럼 사용해서 인수로 전달하거나, 결과로 반환하거나, 자료구조에 저장할 수 있음을 의미한다.

- 일급함수 : 일반갑서럼 취급할 수 있는 함수를 일급함수라고 한다. ( 자바 8이 이전버전과 구별되는 특징중 하나. )
- `::`   : 연산자로 메서드레퍼런스를 만들거나, 메서드를 함수값으로 사용할 수 있다.

##14.2 고차원 함수
> 함수형 프로그래밍 커뮤니티에 따릐면 Comparator.comparing처럼 다음 중 하나 이상의 동작을 수행하는 함수를 `고차원 함수` 라고 부른다 .

- 고차원 함수 : 하나이상의 함수를 인수로 받음. 함수를 결과로 반환. 
- 부작용과 고차원 함수 : 고차원 함수나 메서드를 구현할 때 어떤 인수가 전달될 지 알 수 없으므로 인수가 부작용을 포함할 가능성을 염두해 두어야한다. 함수를 인수로 받아 사용하면서 코드가 정확히 어떤 작업을 수행하고 프로그램의 상태를 어떻게 바꿀지 예측하기 어려워 진다. (디버깅도 어려워질 것이다!) 따라서 인수로 전달된 함수가 어떤 부작용ㅇ을 포함하게 될지 정확하게 문서화하는 것이 좋다. 