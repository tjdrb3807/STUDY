# 메서드
`메서드`(method)는 특정 타입에 관련된 함수를 뜻한다. 클래스, 구조체, 열거형 등은 실행하는 기능을 캡슐화한 인스턴스를 정의할 수 있다. 또한, 타입 자체와 관련된 기능을 실핼하는 타입 메서드를 정의할 수도 있다. 타입 메서드는 기존의 프로그래밍 언어에서의 클래스 메서드와 유사한 개념이다.

구조체와 열거형이 메서드를 가질 수 있다는 것은 기존 프로그래밍 언어와 스위프트간의 큰 차이점이다. 스위프트에서는 프로그래머가 정의하는 타입(클래스, 구조체, 열거형 등)에 자유롭게 메서드를 정의할 할 수 있다.

<br>
<br>

## 인스턴스 메서드
인스턴스 메서드는 특정 타입의 인스턴스에 속한 함수를 뜻한다. 인스턴스 내부의 프로퍼티 값을 변경하거나 특정 연산 결과를 반환하는 등 인스턴스와 관련된 기능을 실행한다. 인스턴스 메서드는 함수와 문법이 같다.

인스턴스 메서드는 함수와 달리 특정 타입 내부에서 구현한다. 따라서 인스턴스가 존재할 떄만 사용할 수 있다. 이 점이 함수와 유일한 차이점이다.

>클래스의 인스턴스 메서드
```Swift
class LevelClass {
    var level: Int = 0 {    // 현재 레벨을 저장하는 저장 프로퍼티
        didSet { print("Level \(level)") }   // 프로퍼티 값이 변경되면 호출되는 프로퍼티 감시자
    }
    
    // 레벨이 올랐을 때 호출할 메서드
    func levelUp() {
        print("Level Up!")
        level += 1
    }
    
    // 레벨이 감소했을 때 호출할 메서드
    func levelDown() {
        print("Level Down")
        level -= 1

        if level < 0 { reset() }
    }
    
    // 특정 레벨로 이동할 떄 호출할 메서드
    func jumpLevel(to: Int) {
        print("Jump to \(to)")
        level = to
    }
    
    // 레벨을 초기화할 떄 호출할 메서드
    func reset() {
        print("Reset!")
        level = 0
    }
}

var levelClassInstance: LevelClass = LevelClass()
levelClassInstance.levelUp()        // Level Up!
// Level 1

levelClassInstance.levelDown()      // Level Down
// Level 0

levelClassInstance.levelDown()      // Level Down
// Level -1
// Reset!
// Level 0

levelClassInstance.jumpLevel(to: 3) // Jump to 3
// Level 3
```

<br>

### mutation
자신의 프로퍼티 값을 수정할 때 클래스의 인스턴스 메서드는 크게 신경 쓸 필요가 없지만, 구조체나 열거형 등은 값 타입이므로 메서드 안에 `mutating` 키워드를 붙여 해당 메서드가 인스턴스 내부의 값을 벼경한다는 것을 명시해야한다.

>mutation 키워드 사용
```Swift
struct LevelStruct {
    var level: Int = 0 {
        didSet { print("Level \(level)") }
    }
    
    mutating func levelUp() {
        print("Level Up!")
        level += 1
    }
    
    mutating func levelDown() {
        print("Level Down")
        level -= 1
        
        if level < 0 { reset() }
    }
    
    mutating func jumpLevel(to: Int) {
        print("Jump to \(to)")
        level = to
    }
    
    mutating func reset() {
        print("Reset!")
        level = 0
    }
}

var levelStructInstance: LevelStruct = LevelStruct()
levelStructInstance.levelUp()           // Level Up!
// Level 1

levelStructInstance.levelDown()         // Level Down
// Level 0

levelStructInstance.levelDown()         // Level Down
// Level -1
// Reest!
// Level 0

levelStructInstance.jumpLevel(to: 3)    // Jump to 3
// Level 3
```

<br>

### self 프로퍼티
모든 인스턴스는 암시적으로 생성된 `self` 프로퍼티를 갖는다. Java의 `this`와 비슷하게 인스턴스 자기 자신을 가리키는 프로퍼티이다. `self` 프로퍼티는 인스턴스를 더 명확히 지칭하고싶을 때 사용한다. 스위프트는 자동으로 메서드 내부에 선언된 지역변수를 먼저 사용하고, 그다음 메서드 매개변수, 그다음 인스턴스의 프로퍼티를 찾아서 해당 변수가 무엇을 지칭하는지 유추한다. 위 코드의 `jumpLevel(to:)`메서드에 사용된 매개변수 'to'는 메서드 내부에서 전달인자로 사용하기에 불편한 이름을 갖고있다. 따라서 to를 전달인자 레이블로 지칭하고 매개면수명을 'level'로 지칭하는 것이 메서드 이름에 적합하다. 하지만 이처럼 메서드를 정의하면 메서드 안에서 전달인자와 프로퍼티 이름이 중복되고 스위프트의 변수 사용 순서에 따라 level 프로퍼티를 작성하면 매개변수로 인식하여 컴파일 오류가 발생한다. 이때 level을 매개변수가 아닌 인스턴스 프로퍼티인 level로 지칭하고 싶다면 변수명 앞에 `self` 키워드를 사용하면된다.

>self 프로퍼티 사용
```Swift
class LevelClass {
   var level: Int = 0

   func jumpLevel(to level: Int) {
      print("Jump to \(level)")
      self.level = level
   }
}
```

<br>

`self 프로퍼티`의 다른 용도는 값 타입 인스턴스 자체의 값을 치환할 수 있다. 클래스의 인스턴스는 참조 타입이라서 self 프로퍼티에 다른 참조를 할당할 수 없지만, 구조체나 열거형 등은 self 프로퍼티를 사용하여 자신 자체를 치환할 수도 있다. 

>self 프로퍼티와 mutating 키워드
```Swift
class LevelClass {
    var level: Int = 0
    
    func reset() {
        self = LevelClass() // 컴파일 오류 - self 프로퍼티 참조 변경 불가!
    }
}

struct LevelStruct {
    var level: Int = 0
    
    mutating func levelUp() {
        print("Level Up!")
        level += 1
    }
    
    mutating func reset() {
        print("Reset!")
        self = LevelStruct()
    }
}

var levelStructInstance: LevelStruct = LevelStruct()
levelStructInstance.levelUp()       // Level Up!
print(levelStructInstance.level)    // 1

levelStructInstance.reset()         // Reset!
print(levelStructInstance.level)    // 0

enum OnOffSwitch {
    case on, off
    
    mutating func nextState() {
        self = self == .on ? .off : .on
    }
}

var toggle: OnOffSwitch = OnOffSwitch.off
toggle.nextState()
print(toggle)   //  on
```

<br>

### 인스턴스를 함수처럼 호출하도록 하는 메서드
사용자 정의 명목 타입의 호출 가능한 값(Callable values of user-definded nominal types)을 구현하기 위해 인스턴스를 함수처럼 호출할 수 있도록 하는 메서드(call-as-function method)가 있다. 특정 타입의 인스턴스를 문법적으로 함수를 사용하는 것처럼 보이게 할 수 있다는 뜻이다. 인스턴스를 함수처럼 호출할 수 있도록 하려면 `callAsFunction`이라는 이름의 메서드를 구현하면 된다. 이 메서드는 매개변수와 반환 타입만 다르다면 개수에 제한 없이 원하는 만큼 만들 수 있다. `mutating` 키워드도 사용할 수 있고, `throws`와 `rethrows`도 함께 사용할 수 있다.

>구조체에서 callAsFunction 메서드 구현
```Swift
struct Puppy {
    var name: String = "멍멍이"
    
    func callAsFunction() { print("멍멍") }
    
    func callAsFunction(destination: String) { print("\(destination)(으)로 달려갑니다.") }
    
    func callAsFunction(something: String, times: Int) { print("\(something)(을)를 \(times)번 반복합니다.") }
    
    func callAsFunction(color: String) -> String { "\(color) 응가" }
    
    mutating func callAsFunction(name: String) { self.name = name }
}

var doggy: Puppy = Puppy()
doggy.callAsFunction()                               // 멍멍
doggy()                                              // 멍멍

doggy.callAsFunction(destination: "집")               // 집(으)로 달려갑니다.
doggy(destination: "집")                              // 집(으)로 달려갑니다.

doggy.callAsFunction(something: "재주넘기", times: 3)   // 재주넘기(을)를 3번 반복합니다.
doggy(something: "재주넘기", times: 3)                  // 재주넘기(을)를 3번 반복합니다.

print(doggy(color: "무지개색"))                         // 무지개색 응가

doggy(name: "댕댕이")
print(doggy.name)                                     // 댕댕이
```

<br>
<br>

## 타입 메서드
인스턴스 프로퍼티와 타입 프로퍼티가 있듯이 메서드에도 인스턴스 메서드와 타입 메서드가 있다. 타입 자체에 호출이 가능한 메서드를 타입 메서드(흔히 객체지향 프로그래밍에서 지칭하는 클래스 메서드와 유사)라고 부른다. 메서드 앞에 `static` 키워드를 사용하여 타입 메서드임을 나타내준다. 클래스의 타입 메서드는 `static` 키워드와 `class` 키워드를 사용할 수 있는데 `static`으로 정의하면 상속 후 메서드 재정의가 불가능하고 `class`로 정의하면 상속 후 메서드 재정의가 가능하다.

>클래스의 타입 메서드
```Swift
class AClass {
    static func staticTypeMethod() { print("AClass staticTypeMethod") }
    
    class func classTypeMethod() { print("AClass classTypeMethod") }
}

class BClass: AClass {
    /*
     // 컴파일 오류 발생!! - 재정의 불가!
     override static func staticTypeMethod() { }
     */
    
    override class func classTypeMethod() { print("BClass classType Method") }
}

AClass.staticTypeMethod()   // AClass staticTypeMethod
AClass.classTypeMethod()    // AClass classTypeMethod
BClass.classTypeMethod()    // BClass classType Method
```

<br>

타입 메서드는 인스턴스 메서드와는 달리 `self 프로퍼티가`가 타입 그 자체를 가리킨다. 인스턴스 메서드에서는 `self`가 인스턴스를 가리킨다면 타입 메서드의 `self`는 타입을 가리킨다. 그래서 타입 메서드 내부에서 타입 이름과 `self`는 같은 뜻이라고 볼 수 있다. 그래서 타입 메서드에서 `self 프로퍼티`를 사용하면 타입 프로퍼티 및 타입 메서드를 호출할 수 있다. 

>타입 프로퍼티와 타입 메서드의 사용
```Swift
// 시스템 음량은 한 기기에서 유일한 값이어야 한다.
struct SystemVolume {
    static var volume: Int = 5              // 타입 프로퍼티를 사용하면 언제나 유일한 값이 된다.
    
    static func mute() { self.volume = 0 }  // 타입 프로퍼티를 제어하기 위해 타입 메서드 사용
    // SystemVolume.volume = 0과 같은 표현
    // Self.volume = 0과 같은 표현
}

// 내비게이션 역할은 여러 인스턴스가 수행할 수 있다.
class Navigation {
    var volume: Int = 5                     // 내비게이션 인스턴스마다 음량을 따로 설정할 수 있다.
    
    // 길 안내 음성 재생
    func guideWay() { SystemVolume.mute() } // 내비게이션 의 다른 재생원 음소거
    
    // 길 안내 음성 종료
    func finishGuideWay() { SystemVolume.volume = self.volume } // 기존 재생원 음량 복구
}

SystemVolume.volume = 10

let myNavi: Navigation = Navigation()

myNavi.guideWay()
print(SystemVolume.volume)  // 0

myNavi.finishGuideWay()
print(SystemVolume.volume)  // 5
```