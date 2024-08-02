# 함수의 정의와 호출
함수와 메서드는 정의하는 위치와 호출되는 범위만 다를 뿐, 정의하는 키워드와 구현하는 방법은 같다. 조건문이나 반복문 같은 스위프트의 다른 문법과 달리 함수에서 소괄호(`()`)를 생략할 수 없다. 스위프트의 함수는 재정의(오버라이드)와 중복(오버로드)를 모두 지원한다. 따라서 매겨변수의 타입이 다르면 같은 이름의 함수를 여러 개 만들 수 있고, 매개변수의 개수가 달리도 같은 이름의 함수를 만들 수 있다. 

## 기본적인 함수의 정의와 호출
스위프트의 함수는 자유도가 굉장히 높은 문법 중 하나다. 기본으로 함수의 이름과 매개변수(Parameter), 반환타입(Return Type)등을 사용하여 함수를 정의한다.

함수를 정의하는 키워드는 `func`이다. 함수 이름을 지정해준 후 매개변수는 소괄호(())로 감싸준다. 반환 타입을 명시하기 전에는 `->`를 사용하여 어떤 타입이 반환될 것인지 명시해준다. 반환을 위한 키워드는 다른 언어처럼 `return`이다. 

>기본 형태의 함수 정의와 사용
```Swift
func hello(name: String) -> String {
    return "Hello \(name)!"
}

let helloJenny: String = hello(name: "Jenny")
print(helloJenny)   // Hello Jenny!


func introduce(name: String) -> String {
    // [return "제 이름은 " + name + "입니다"] 와 같은 동작
    "제 이름은 " + name + "입니다"
}

let introduceJenny: String = introduce(name: "Jenny")
print(introduceJenny)   // "제 이름은 Jenny 입니다
```

함수 내부의 코드가 단 한 줄의 표현이고, 그 표현의 결과값의 타입이 함수의 반환 타입과 일치하면 `retrun` 키워드를 생략해도 그 표현의 결과값이 함수의 반환값이 된다.

## 매개변수
스위프트의 함수는 매개변수를 어떻게 정의하냐에 따라서도 모습이 크게 달라질 수 있다. 

### 매개변수가 없는 함수와 매개변수가 여러 개인 함수
함수에 매개변수가 필요 없다면 매개변수 위치를 공란으로 비워둔다.

>매개변수가 없는 함수의 정의와 사용
```Swift
func helloWorld() -> String {
    return "Hello, world!"
}

print(helloWorld()) // Hello, world!
```

매개변수가 여러 개 필요한 함수를 저의할 때는 쉼표(`,`)로 매개변수를 구분한다. 주의할 점은 함수를 호출할 때, 매개변수 이름을 붙여주고 콜론(`:`)을 적어준 후 전달인자를 보내준다는 점이다. 이렇게 호출 시에 매개변수에 붙이는 이름을 `매개변수 이름(Parameter Name)`이라고 한다. 

>매개변수가 여러 개인 함수의 정의와 사용
```Swift
func sayHello(myName: String, yourName: String) -> String {
    return "Hello \(yourName)! I'm \(myName)"
}

print(sayHello(myName: "yagom", yourName: "Jenny")) // Hello Jenny! I'm yagom
```

### 매개변수 이름과 전달인자 레이블
보통 함수를 정의할 때 매개변수를 정의하면 매개변수 이름과 전달인자 레이블을 같은 이름으로 사용할 수 있지만 전달인자 레이블을 별도로 지정하면 함수 외부에서 매개변수의 역할을 좀 더 명확히 할 수 있다. 전달인자 레이블을 사용하려면 함수 정의에서 매개변수 이름 핲에 한 칸을 띄운 후 전달인자 레이블을 지정한다. 

>매개변수 이름과 전달인자 레이블을 가지는 함수 정의와 사용
```Swift
// from과 to라는 전달인자 레이블이 있으며
// myName과 name이라는 매개변수 이름이 있는 sayHello 함수
func sayHello(from myName: String, to name: String) -> String {
    return "Hello \(name)! I'm \(myName)"
}

print(sayHello(from: "yagom", to: "Jenny")) // Hello Jenny! I'm yagom
```

함수 내부에서 전달인자 레이블을 사용할 수 없으며, 함수를 호출할 때는 매개변수 이름을 사용할 수 없다. C 언어나 Java 같은 기존 언어처럼 전달인자 레이블을 사용하고 싶지 않다면 `와일드카드 식별자`를 사용한다. 

>전달인자 레이블이 없는 함수 정의와 사용
```Swift
func sayHello(_ name: String, _ times: Int) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!" + " "
    }
    
    return result
}

print(sayHello("Chope", 2)) // Hello Chope! Hello Chope!
```

전달인자 레이블을 변경하면 함수의 이름 자체가 변경된다. 그렇기 때문에 전달인자 레이블만 다르게 써주더라도 함수 중복 정의(오버로드)로 동작할 수 있다. 

전달인자 레이블을 사용하는 경우 매개변수 이름은 함수의 이름에 포함되지 않으므로 매개변수 이름과 타입이 같은 함수를 매개변수 이름만 바꿔서 중복 정의할 수 없다. 

>전달인자 레이블 변경을 통한 함수 중복 적의
```Swift
func sayHello(to name: String, _ times: Int) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!" + " "
    }
    
    return result
}

func sayHello(to name: String, repeatCount times: Int) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!" + " "
    }
    
    return result
}

print(sayHello(to: "Chope", 2))
print(sayHello(to: "Chope", repeatCount: 2))
```

### 매개변수 기본값
스위프트 함수에서는 매개변수마다 기본값을 지정할 수 있다. 즉, 매개변수가 전달되지 않으면 기본값을 사용한다. 매개변수 기본값이 있는 함수는 함수를 중복 정의한 것처럼 사용할 수 있다.

기본값이 없는 매개변수를 기본값이 있는 매개변수 앞에 사용한다. 기본값이 없는 매개변수는 대체로 함수를 사용함에 있어 중요한 값을 전달할 가능성이 높다. 무엇보다 기본값이 있는지와 상관 없이 중요한 매개변수는 앞쪽에 배치하는 것이 좋다.

>매개변수 기본값이 있는 함수의 정의와 사용
```Swift
// times 매개변수가 기본값 3을 갖는다
func sayHello(_ name: String, times: Int = 3) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!" + " "
    }
    
    return result
}

// tiems 매개변수의 절달 값을 넘겨주지 않아 기본값 3을 반영해서 세 번 출력.
print(sayHello("Hana"))             // Hello Hana! Hello Hana! Hello Hana!

// times 매개변수의 전달 값을 2로 넘겨주었기 때문에 전달 값을 반영해서 두 번 출력.
print(sayHello("Joe", times: 2))    // Hello Joe! Hello Joe!
```

### 가변 매개변수와 입출력 매개변수
매개변수로 몇 개의 값이 들어올지 모를 때, 가변 매개변수를 사용할 수 있다. 가변 매개변수는 0개 이상(0개 포함)의 값을 받아올 수 있으며, 가변 매개변수로 들어온 인자 값은 배열처럼 사용할 수 있다. 함수마다 가변 매개변수는 하나만 가질 수 있다. 

>가변 매개변수를 가지는 함수의 정의와 사용
```Swift
func sayHelloToFriends(me: String, friends names: String...) -> String {
    var result: String = ""
    
    for friend in names {
        result += "Hello \(friend)!" + " "
    }
    
    result += "I'm " + me + "!"
    return result
}

print(sayHelloToFriends(me: "yagom", friends: "Johansson", "Jay", "Wizplan"))
// Hello Johansson! Hello Jay! Hello Wizplan! I'm yagom!
```

함수의 전달인자로 값을 전달할 때는 보통 값을 복사해서 전달한다. 값이 아닌 `참조`를 전달하려면 입출력 매개변수를 사용한다. 값 타입 데이터의 참조를 전달인자로 보내면 함수 내부에서 참조하여 원래 값을 변경한다. C언어의 포인터와 유사하다. 하지만 이 방법은 함수 외부의 값에 어떤 영향을 줄지 모르기 때문에 함수형 프로그래밍 패터다임에서는 지양하는 패턴이다. 물론 객체지향 프로그래밍 패러다임에서는 종종 사용된다. 애플의 프레임워크(iOS, macOS 등)에서는 객체지향 프로그래밍 패러다임을 사용하므로 유용할 수 있지만, 애플 프레임워크를 벗어난 다른 환경에서 함수형 프로그래밍 패러다임을 사용할 때는 입출력 매개변수를 사용하지 않는 것이 좋다.

입출력 매개변수의 전달 순서
1. 함수를 호출할 때, 전달인자의 값을 복사한다.
2. 해당 전달인자의 값을 변경하면 1에서 복사한 것을 함수 내부에서 변경한다. 
3. 함수를 반환하는 시점에 2에서 변경된 값을 원래의 매개변수에 할당한다. 

연산 프로퍼티 또는 감시자가 있는 프로퍼티가 입출력 매개변수로 전달된다면, 함수 호출 시점에 그 프로퍼티의 접근자가 호출되고 함수의 반환 시점에 프로퍼티의 설정자가 호출된다.

참조는 `inout` 매개변수로 전달될 변수 또는 상수 앞에 앰퍼샌드(`&`)를 붙여서 표현한다. 

>inout 매개변수의 활용
```Swift
var numbers: [Int] = [1, 2, 3]

func nonReferenceParameter(_ arr: [Int]) {
    var copiedArr: [Int] = arr
    
    copiedArr[1] = 1
}

func referenceParameter(_ arr: inout [Int]) {
    arr[1] = 1
}

nonReferenceParameter(numbers)
print(numbers[1])   // 2

referenceParameter(&numbers)    // 참조를 표현하기 위해 &를 붙여준다.
print(numbers[1])   // 1
```

입출력 매개변수는 매개변수 기본값을 가질 수 없으며, 가변 매개변수도 사용될 수 없다. 또한 상수는 변경될 수 없으므로 입출력 매개변수의 전달인자로 사용될 수 없다. 

입출력 매개변수는 잘 사용하면 문제 없지만 잘못 사용하면 `메모리 안전`을 위협하기도 한다. 따라서 사용에 몇몇 제약이 있다. 

## 반환이 없는 함수
함수는 특정 연산을 실행한 후 결과값을 반환한다. 그러나 값의 반환이 굳이 필요하지 않은 함수도 있다. 그럴 때는 반환 값이 없는 함수를 만들어 줄 수 있다. 만약 반환 값이 없는 함수라면 반환 타입을 '없음'을 의미하는 `Void`로 표기하거나 아예 반환 타입 표현을 생략해도 된다. 

>반환 값이 없는 함수의 정의와 사용
```Swift
func sayHelloWorld() {
    print("Hello, world!")
}

sayHelloWorld() // Hello, world!

func sayHello(from myName: String, to name: String) {
    print("Hello \(name)! I'm \(myName)")
}

sayHello(from: "yagom", to: "Mijeong")  // Hello Mijeong! I'm yagom

func sayGoodbye() -> Void { // Void를 명시해주어도 상관없다.
    print("Gooy bye")
}

sayGoodbye()    // Gooy bye
```

## 데이터 타입으로서의 함수
스위프트의 함수는 일급 객체이므로 하나의 데이터 타입으로 사용할 수 있다. 즉, 각 함수는 매개변수 타입과 반환 타입으로 구성된 하나의 타입으로 사용(정의)할 수 있다는 뜻이다. 
```
(매개변수 타입의 나열) -> 반환 타입
```

예를 들어 다음과 같은 함수가 있다고 하면
```Swift
func sayHello(name: String, times: Int) -> String {
    // ...
}
```
sayHello 함수의 타입은 `(String, Int) -> String` 이다. 만약 매개변수나 반환 값이 없다면 `Void` 키워드를 사용하여 없음을 나타낸다
```Swift
func sayHelloWorld() {
    // ...
}
```
sayHelloWorld 함수의 타입은 `(Void) -> Void`이다. 참고로 Void 키워드를 빈 소괄호의 묶음으로 표현할 수도 있다. 
* (Void) -> Void
* ( ) -> Void
* ( ) -> ( )

함수를 데이터 타입으로 사용할 수 있다는 것은 함수를 전달인자로 받을 수도, 반환 값으로 돌려줄 수도 있다는 의미다. 상황에 맞는 함수를 전달인자로 넘겨 적절히 처리할 수도 있으며 상황에 맞는 함수를 반환해주는 것도 가능하다. 이는 스위프트의 함수가 일급 객체이기 때문에 가능한 일이다. 

>함수 타입의 사용
```Swift
typealias CalculateTwoInts = (Int, Int) -> Int

func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}

var mathFunctions: CalculateTwoInts = addTwoInts

print(mathFunctions(2, 5))  // 2 + 5 = 7

mathFunctions = multiplyTwoInts
print(mathFunctions(2, 5))  // 2 * 5 = 10
```

>전달인자로 함수를 전달받는 함수
```Swift
func printMathResult(_ mathFunction: CalculateTwoInts, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}

printMathResult(addTwoInts, 3, 5)
```

>특정 조건에 따라 적절한 함수를 반환해주는 함수
```Swift
func chooseMathFunction(_ toAdd: Bool) -> CalculateTwoInts {
    return toAdd ?  addTwoInts : multiplyTwoInts
}

printMathResult(chooseMathFunction(true), 3, 5) // Result: 8
```