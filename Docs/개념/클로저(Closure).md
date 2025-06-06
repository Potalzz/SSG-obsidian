
#### 클로저란 ?

함수처럼 사용하지만 이름이 없는 **익명 함수**

하지만 사실 `func 키워드`를 이용해 이름을 붙여주는 함수들도 클로저이다 !
	클로저는 **Named Closure**와 **Unnamed Closure** 총 두 가지 종류가 있다. 이름을 붙여 선언하는 함수는 **Named Closure**이지만 편하게 **함수**라고 부르는 것 뿐이다.`(그래도 클로저란 사실은 변함 없다)`

이름을 붙이지 않고 사용하는 함수를 **익명 함수** 즉, **클로저**라고 부른다!

변수나 상수에 할당할 수 있으며, 함수의 매개변수로 전달 가능

클로저는 **1급 객체**이므로 함수의 파라미터로 클로저를 전달할 수 있다.


#### 클로저 표현식

```
{ (Parameters) -> Ruturn Type in
	실행 구문
}
```

또한, 클로저는 **클로저 헤드**와 **클로저 바디**로 이루어져 있다.
![[Pasted image 20250325110415.png]]

이 둘을 구분지어주는 것이 `in` 이라는 **키워드** 이다 !

#### 그래서 클로저를 왜 사용할까 ?
클로저는 Swift에서 아주 중요하고 좋은 기능 중 하나이지만, 처음 배우는 사람들에게 혼란스럽게 하는 기능이다. 클로저를 사용하는 가장 일반적인 이유 중 하나는 **기능을 저장**하는 것이다.

>"언젠가는 해야 할 작업이지만 지금은 필요하지 않다."

**다음과 같은 상황에서 사용할 수 있다.**
1. 딜레이 후 일부 코드를 실행한다.
2. 애니메이션이 완료된 후 일부 코드를 실행한다.
3. 다운로드가 완료되면 일부 코드를 실행한다.
4. 사용자가 메뉴에서 옵션을 선택한 경우 일부 코드를 실행한다.

클로저를 통해 일부 기능을 단일 변수로 마무리하고 어딘가에 저장할 수 있다. 또한 기능에서 `return` 하고 클로저를 다른 곳에 `보관` 할 수도 있다.

##### 먼저 인자와 반환값에 대해 알아보자.

스위프트에서 `( ) -> Void`는 함수의 형식을 나타내는 표시로써, 인자값으로 사용될 함수의 형식을 나타내는 데에 주로 사용된다.

보통 화살표 `->`를 기준으로 양쪽에 작성된 기호나 문자들은 함수의 매개변수와 반환값의 형식을 의미한다.


![[Pasted image 20250326232015.png]]
**왼쪽이 함수의 매개변수**를, **오른쪽이 반환값**을 뜻한다.

**매개변수**에 해당하는 **왼쪽**이 `()`인 것은 매개변수가 없는 함수라는 것을 의미하고,
**반환값**에 해당하는 **오른쪽**이 `Void`인 것은 아무 값도 반환하지 않는 함수를 의미한다.
즉, 매개변수와 반환값이 **모두 없는** 함수 타입을 의미한다.
```
// 매개변수와 반환값이 없는 함수
func exec() {
	tabBar?.alpha = ( tabBar?.alpha == 0 ? 1 : 0)
}
```


만약 `Int` 타입 하나를 입력받아 `String`을 반환한다면 이 함수의 타입 표시는 다음과 같다.
`(Int) -> String`

이렇게 함수가 완성되면, 이를 인자값으로 전달해 줄 차례이다.
함수를 인자값으로 전달할 때에는 단순히 함수의 이름만 사용한다.
함수를 실행하기 위한 `( )` 같은 것을 제외하고.
```
UIView.animate(withDuration: TimeInterval(0.15), animations: exec)
```

그런데 이런 방식으로 **함수를 인자값으로** 사용하다 보면 **두 가지 불편한 점**이 드러난다.
> 1. 함수를 인자값으로 넘기기 전에 **먼저 정의**해야 한다.
> 2. 재사용성이 거의 없는 인자값을 위해 **굳이 함수를 정의**해야 한다.

이 같은 문제를 해결하기 위해 ***클로저(Closure)*** 를 사용한다.
(자바스크립트의 익명 함수 같은 개념)

##### 클로저 사용법
클로저를 사용하면 앞의 **두 가지 문제**를 한꺼번에 해결할 수 있다.
먼저 미리 정의해 둘 필요 없이 인자값으로 전달하는 시점에 함수를 정의할 수 있으며,
모든 함수를 클로저 형식으로 변환할 수 있다.
```
// 앞의 exec()함수를 클로저로 정의
{
	tabBar?.alpha = ( tabBar?.alpha == 0 ? 1 : 0)
}
```

중괄호의 시작 `{`에서 중괄호의 종료 `}`까지가 모두 클로저 범위에 해당한다.
**클로저**는 따로 이름을 가지지 않기 때문에 코드 블록으로 시작하여 코드 블록으로 마감된다.
**클로저**를 인자값으로 사용할 때에는 다음과 같이 통째로 넣어주면 된다.
```
UIView.animate(withDuration: TimeInterval(0.15), animations: {
	tabBar?.alpha = ( tabBar?.alpha == 0 ? 1 : 0)
})
```

#### 트레일링 클로저

>함수의 **마지막 매개변수**가 클로저를 입력받는 경우,
>인자값의 위치에 클로저 구문을 넣지 않고, 대신 **메소드 뒤쪽에 실행 블록처럼** 붙일 수 있도록 하는 예외 문법이다.

```
// 마지막 매개변수 animations를 생략하고 클로저 블록을 맨 뒤에 붙임.
UIView.animate(withDuration: TimeInterval(0.15) ) {
	tabBar?.alpha = ( tabBar?.alpha == 0 ? 1 : 0)
}
```

>추가적인 정보 https://babbab2.tistory.com/82 참고


함수와 달리 주변 컨텍스트(변수 등)를 **캡쳐**하여 사용 가능 *(변수를 캡쳐한다는 것은 함수 내부에 선언된 변수는 함수가 다시 실행될 때마다 값이 초기화되지만 캡쳐하게 되면 독립적으로 값을 계속해서 기억하고 있기 때문에 함수가 실행될 때마다 이전에 기억한 값을 토대로 업데이트가 진행된다.)*

`Int`형태의 `value`타입은 클로저 시작 범위에서 변수를 캡쳐하여 대입하지만, `Stirng`형태의 `Reference`타입은 `Reference Capture`  참조 한다고 생각하면 된다.

클로져 내부에서 변수나 함수를 사용하려면 self키워드를 추가해야 한다.

일반적인 함수는 함수가 종료되면 메모리에서 삭제되기 때문에, var 사용이나 메모리 부분에서 걱정할 부분이 적지만 클로저는 외부 변수를 캡처하기 때문에 함수 종료 후에도 메모리에 남아 있어 메모리 누수가 발생할 수 있다. 이를 방지하려면 `[weak self]` 또는 `[unowned self]`를 사용하여 메모리에서 해제될 수 있도록 관리해야 한다.