
## 1. 프로토콜 (Protocol)이란 ?
>특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진을 정의한다.
>
>구조체, 클래스, 열거형은 프로토콜을 채택(Adopted)해서 특정 기능을 실행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있다.
>
>타입에서 **프로토콜의 요구사항을 충족시키려면** 프로토콜이 제시하는 청사진(Blueprint)의 기능을 **모두 구현해야 한다**.
>
>즉, **프로토콜은 정의를 하고 제시를 할 뿐 스스로 기능을 구현하지는 않는다.**

간단하게 말하면 프로토콜은 일종의 **계약서**와 같다.

## 2. 프로토콜의 특징은 어떤 것이 있을까 ?

### 추상화 도구
>기능만 명시하고, 구현은 하지 않는다.

#### 예시
```
protocol Vehicle {
	func drive()
}
```
>Vehicle이라는 프로토콜을 채택하는 각 타입이 drive를 구체화한다.

"운전"이라는 개념만 정해두고 `Vehicle`이라는 프로토콜을 채택하면, 자전거든 자동차든 자유롭게 구현한다.

### 다중 채택 가능
>여러 프로토콜을 한 타입에 동시에 적용 가능하다.

#### 예시
```
protocol Flyable {
	...
}

protocol Shootable {
	...
}

class Jet: Flyable, Shootable {
	...
}
```
>제트기가 "날 수 있고", "쏠 수 있는" 능력을 동시에 가진다.


### 구조체/클래스/열거형에 모두 적용 가능
>프로토콜은 모든 타입에서 채택 가능하다.

```
enum Direction: Descriable {
	...
}
```
>계약서(프로토콜)를 사람(struct), 로봇(class), 버튼(enum)등 누구든 따를 수 있다.


### 프로토콜 확장 (Protocol Extension)
>프로토콜에 기본 구현을 제공할 수 있게 해준다.

```
protocol Drawable {
	func draw()
	func prepare()
}

// 프로토콜 확장으로 기본 구현 제공
extension Drawable {
	func draw() {
		print("기본 그리기 구현")
	}

	func prepare() {
		print("그리기 준비 중...")
	}

	// 프로토콜에 없는 추가 메서드 제공 가능
	func clean() {
		print("그리기 도구 정리 중...")
	}
}
```
>프로토콜 확장은 프로토콜에 정의된 요구사항에 대한 기본 구현을 제공한다.

프로토콜 확장을 사용함으로써 다음과 같은 효과를 기대할 수 있다.

**선택적 구현 가능**
	기본 구현이 제공된 메서드를 재정의하지 않아도 된다.

**필요에 따른 재정의**
	기본 구현이 있더라도 특정 타입에 맞게 메서드를 재정의할 수 있다.

**프로토콜에 없는 메서드 추가 가능**
	 프로토콜 확장을 통해 프로토콜에 정의되지 않은 추가 메서드도 제공할 수 있다.

**조건부 확장 (Conditional Extensions)**
	특정 조건을 만족하는 타입에만 적용되는 확장을 만들 수 있다.


### 컴파일 타임 검증
>채택한 타입이 규칙을 따르지 않으면 컴파일 오류

프로토콜을 채택한 타입이 해당 프로토콜에 구현된 메서드를 구현하지 않는 경우, 즉시 걸리는 정적 검사기처럼 컴파일 오류를 발생시킨다.


### 연관 타입 지원(associatedtype)
>제네릭처럼 타입 유연성 제공

```
protocol Container {
	associatedtype Item
}
```

"어떤 박스는 사과, 어떤 박스는 책"처럼 내용물 타입이 다른 박스 설계



## 3. 프로토콜은 왜 사용할까 ?
### 공통 인터페이스 제공
> 여러 타입이 동일한 동작을 하도록 설계할 수 있다.

#### 예시
```
protocol Playable {
	func play()
}

struct Song: Playable {
	func play() {
		print("노래 재생 중")
	}
}

struct Video: Playable {
	func play() {
		print("비디오 재생 중")
	}
}
```
>Song과 Video 모두 Playable을 채택하여 공통 동작 `play()`를 구현한다.

"재생 가능한 미디어"라는 공통 리모컨을 통해 다양한 기기를 같은 방식으로 제어할 수 있다.

---

### 유연한 설계 (추상화)
> 타입이 아닌 역할(행동) 중심으로 설계할 수 있다.

#### 예시
```
protocol APIService {
	func fetchData()
}

class ViewModel {
	let service: APIService

	init(service: APIService) {
		self.service = service
	}
}

```

> 실제 서비스이든 테스트 서비스이든 ‘APIService 역할’만 하면 된다.

진짜 택배 기사든 로봇 택배 기사든, "배송만 잘하면 된다"는 관점의 설계다.

---

### 테스트 용이성 (Mock 가능)
> 실제 구현체 없이도 테스트 가능

#### 예시
```
class MockService: APIService {
	func fetchData() {
		print("테스트용 데이터 반환")
	}
}

```
> 테스트 환경에서는 실제 네트워크 요청 없이도 동작 검증 가능

배우가 리허설할 때 대역 배우로 연습하는 것과 유사하다.

---

### 다형성 (Polymorphism)

> 서로 다른 타입을 하나의 인터페이스로 처리할 수 있다.

#### 예시
```
let mediaList: [Playable] = [Song(), Video()]
mediaList.forEach { $0.play() }
```
> Song이든 Video든 공통 프로토콜 타입으로 배열에 담아 재생한다.

브랜드가 다른 TV라도 하나의 리모컨으로 작동시키는 것처럼, 일관된 사용 가능.



## 4. 프로토콜은 어떠한 상황에서 사용하면 좋을까 ?

### 공통 동작이 필요한 경우
> 여러 타입에 동일한 메서드나 속성이 필요할 때

#### 예시
```
protocol Identifiable {
	var id: UUID { get }
}

struct User: Identifiable {
	let id = UUID()
}
```
> 각 객체를 식별하기 위한 id 속성을 제공한다.

학생 명단에서 각 학생이 고유 학번을 가지듯이, 객체에도 고유 식별자를 부여하는 느낌.

---

### 네트워크/의존성 추상화
> 서비스를 추상화하여 유연한 설계를 가능하게 한다.

#### 예시
```
protocol NetworkService {
	func request()
}

class RealService: NetworkService { ... }
class MockService: NetworkService { ... }
```
> 진짜든 가짜든 ‘NetworkService’ 역할을 수행하면 된다.

실제 택배 기사와 가짜 기사(모형)가 모두 ‘배송 서비스’를 제공할 수 있도록 설계한 셈.

---

### 다양한 ViewModel을 공통 규격으로 묶을 때

> SwiftUI 또는 MVVM에서 각 ViewModel이 공통 동작을 따르게 할 수 있다.

#### 예시
```
protocol ViewModelType {
	func bind()
}

class LoginViewModel: ViewModelType {
	func bind() {
		print("로그인 바인딩")
	}
}
```
> 여러 ViewModel이 동일한 형식으로 View와 바인딩될 수 있다.

"무대에 오르려면 모두 리허설을 해야 한다"는 규칙을 세우는 것과 같다.

---

### 제네릭에 제약을 줄 때

> 특정 역할(프로토콜)을 따르는 타입만 허용

#### 예시
```
func render<T: Drawable>(_ item: T) {
	item.draw()
}
```
> Drawable을 채택한 타입만 render 함수의 인자로 받을 수 있다.

"그림을 그릴 수 있는 것만 캔버스에 올려라"는 규칙.

---

### SwiftUI에서 View 설계 시

> View 프로토콜을 채택하여 SwiftUI 뷰를 구성

#### 예시
```
struct ContentView: View {
	var body: some View {
		Text("Hello, world!")
	}
}
```
> SwiftUI에서는 View 프로토콜을 반드시 채택해야 화면 구성 가능

무대에 오르기 위해서는 반드시 ‘연기 능력(View 프로토콜)’이 있어야 한다는 조건이다.


## 5. 프로토콜은 어떻게 사용할까 ?

### 1. 정의하기

> 먼저 역할(기능)의 인터페이스를 선언한다.

#### 예시
```
protocol Greetable {
	var name: String { get }
	func greet()
}
```
---

### 2. 채택 및 구현하기

> 프로토콜을 따르는 타입이 실제로 기능을 구현한다.

#### 예시
```
struct Person: Greetable {
	var name: String
	func greet() {
		print("안녕하세요, \(name)입니다.")
	}
}
```
---

### 3. 프로토콜을 매개변수 타입으로 사용하기

> 다형성을 활용해 다양한 타입을 다룬다.

#### 예시
```
func greetSomeone(_ someone: Greetable) {
	someone.greet()
}
```
---

### 4. 프로토콜 확장 사용하기

> 기본 동작을 정의하거나 공통 기능 제공

#### 예시
```
extension Greetable {
	func greet() {
		print("기본 인사입니다.")
	}
}
```
> 기본 동작을 제공하며, 필요하면 재정의할 수도 있다.

---

### 5. 제네릭에서 제약으로 사용하기

> 특정 프로토콜을 채택한 타입만 제네릭 함수에 전달 가능

#### 예시
```
func greetAll<T: Greetable>(_ people: [T]) {
	for person in people {
		person.greet()
	}
}
```
---

### 6. 테스트용 Mock 객체 만들기

> 프로토콜을 활용해 테스트 대체 객체를 구현

#### 예시
```
class MockGreetService: Greetable {
	var name: String = "테스트유저"
	func greet() {
		print("👻 테스트 인사")
	}
}
```
> Mock 객체를 통해 실제 구현 없이도 테스트 수행 가능


