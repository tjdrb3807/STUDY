# 프로퍼티
프로퍼티는 크게 `저장 프로퍼티`(Stored Properties), `연산 프로퍼티`(Computed Properties), `타입 프로퍼티`(Type Properties)로 나눌 수 있다. 

저장 프로퍼티는 인스턴스의 변수 또는 상수를 의미한다. 연산 프로퍼티는 값을 저장한 것이 아니라 특정 연산을 실행한 결과값을 의미한다. 연산 프로퍼티는 클래스, 구조체, 열거형에 쓰일 수 있다. 저장 프로퍼티는 구조체와 클래스에서만 사용할 수 있다. 저장 프로퍼티와 연산 프로퍼티는 특정 타입의 인스턴스에 사용되는 것을 뜻하지만 특정 타입에 사용되는 프로퍼티도 존재한다. 이를 타입 프로퍼티라고 한다.

정리하면 기존 프로그래밍 언어에서 사용되던 인스턴스 변수는 저장 프로퍼티로, 클래스 변수는 타입 프로퍼티로 구분지을 수 있다.

더불어, 프로퍼티의 값이 변하는 것을 감시하는 `프로퍼티 감시자`(Property Observers)도 있다. 프로퍼티 감시자는 프로퍼티의 값이 변할 때 값의 변화에 따른 특정 작업을 실행한다. 프로퍼티 감시자는 저장 프로퍼티에 적용할 수 있으며 부모클래스로부터 상속받을 수 있다.

<br>

# Category
1. 저장 프로퍼티
2. 지연 저장 프로퍼티
3. 연산 프로퍼티
4. 프로퍼티 감시자
5. 전역변수와 지역변수
6. 타입 프로퍼티
7. 키 경로

<br>

## 저장 프로퍼티
클래스 또는 구조체의 인스턴스와 연관된 값을 저장하는 가장 단순한 개념의 프로퍼티이다. 저장 프로퍼티는 `var`키워드를 사용하면 변수 저장 프로퍼티, `let` 키워드를 사용하면 상수 저장 프로퍼티가 된다.

저장 프로퍼티를 정의할 때 프로퍼티 기본값과 초깃값을 지정해줄 수 있다.

>저장 프로퍼티의 선언 및 인스턴스 생성
```Swift
// 좌표
struct CoordinatePoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
}

// 구조체에는 기본적으로 저장 프로퍼티를 매개변수로 갖는 이니셜라이저가 있다.
let yagomPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 5)

// 사람의 위치 정보
class Position {
    var point: CoordinatePoint
    // 저장 프로퍼티(변수) - 위치(point)는 변경될 수 있음을 뜻한다.
    let name: String    // 저장 프로퍼티(상수)
    
    // 프로퍼티 기본값을 지정해주지 않는다면 이니셜라이저를 따로 정의해주어야 한다.
    init(name: String, currentPoint: CoordinatePoint) {
        self.name = name
        self.point = currentPoint
    }
}

// 사용자 정의 이니셜라이저를 호출해야한 한다.
// 그렇지 않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스 생성이 불가능하다.
let yagomPosition: Position = Position(name: "yagom", currentPoint: yagomPoint)
```

구조체는 프로퍼티에 맞는 이니셜라이저를 자동으로 제공하지만 클래스는 그렇지 않아서 클래스 인스턴스의 저장 프로퍼티를 사용하는 일이 번거롭다. 하지만 클래스의 저장 프로퍼티에 초깃값을 지정해주면 따로 사용자 정의 이니셜라이저를 구현해줄 필요가 없다.

>저장 프로퍼티의 초깃값 지정
```Swift
// 좌표
struct CoordinatePoint {
    var x: Int = 0  // 저장 프로퍼티
    var y: Int = 0  // 저장 프로퍼티
}

// 프로퍼티의 초깃값을 할당했다면 굳이 전달인자로 초깃값을 넘길 필요가 없다.
let yagomPoint: CoordinatePoint = CoordinatePoint()

// 물론 기존에 초깃값을 할당할 수 있는 이니셜라이저도 사용 가능하다.
let wizplanPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 5)

print("yagom's point: \(yagomPoint.x), \(yagomPoint.y)")
// yagom's point: 0, 0

print("wizplan's point: \(wizplanPoint.x), \(wizplanPoint.y)")
// wizplan's point: 10, 5

// 사람의 위치 정보
class Position {
    var point: CoordinatePoint = CoordinatePoint()  // 저장 프로퍼티
    var name: String = "Unknown"                     // 저장 프로퍼티
    
}

// 초깃값을 지정해줬다면 사용자 정의 이니셜라이저를 사용하지 않아도 된다.
let yagomPosition: Position = Position()

yagomPosition.point = yagomPoint
yagomPosition.name = "yagom"
```

초깃값을 미리 지정했더니 인스턴스를 만드는 과정이 훨씬 간편해졌다. 그러나 의도와 맞지 않게 인스턴스가 사용될 가능성이 남아있고, 인스턴스를 생성한 후에 원하는 값을 일일이 할당해야 해서 불편하다. 또, Position의 name 프로퍼티는 한 번 값을 할당해준 후에 변경하지 못하도록 상수로 정의해주고 싶은데, 인스턴스를 생성한 후에 값을 할당해주어야 하기때문에 그렇게 할 수도 없다. 

인스턴스를 생성할 때 이니셜라이저를 통해 초깃값으로 보내야 하는 이유는 프로퍼티가 옵셔널이 아닌 값으로 선언되어 있기 때문이다. 그러므로 인스턴는 생성할 때 프로퍼티에 값이 꼭 이있는 상태여야 한다. 그런데 저장 프로퍼티의 값이 있어도 그만, 없어도 그만인 옵셔널이라면 굳이 초깃값을 넣어주지 않아도 된다. 즉, 이니셜라이저에서 옵셔널 프로퍼티에 꼭 값을 할당해주지 않아도 된다.

>옵셔널 저장 프로퍼티
```Swift
// 좌표
struct CoordinatorPoint {
    // 위치는 x, y 값이 모두 있어야 하므로 옵셔널이면 안된다.
    var x: Int
    var y: Int
}

// 사람의 위치 정보
class Position {
    // 현재 사람의 위치는 모를 수도 있다. - 옵셔널
    var point: CoordinatorPoint?
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

// 이름은 필수지만 위치는 모를 수 있다.
let yagomPosition: Position = Position(name: "yagom")

// 위치를 알게되면 그 때 위치 값을 할당해준다.
yagomPosition.point = CoordinatorPoint(x: 20, y: 10)
```

이렇게 옵셔널과 이니셜라이저를 적절히 사용하면 다른 프로그래머가 사용할 때, 내가 처음 의도했던 대로 구조체와 클래스를 사용할 수 있도록 유도할 수 있다.

## 지연 저장 프로퍼티
인스턴스를 생성할 때 프로퍼티에 값이 필요 없다면 프로퍼티를 옵셔널로 선언해줄 수 있다. 그런데 그것과는 조금 다른 용도로 필요할 때 값이 할당되는 `지연 저장 프로퍼티`(Lazy Stored Properties)가 있다. 지연 저장 프로퍼티는 호출이 있어야 값을 초기화하며, 이때 `lazy` 키워드를 사용한다.

상수는 인스턴스가 완전히 생성되기 전에 초기화해야 하므로 필요할 때 값을 할당하는 지연 저장 프로퍼티와는 맞지 않는다. 따라서 지연 저장 프로퍼티는 var 키워드를 사용하여 변수로 정의한다. 

지연 저장 프로퍼티는 주로 복잡한 클래스나 구조체를 구현할 때 많이 사용된다. 클래스 인스턴스의 저장 프로퍼티로 다른 클래스 인스턴스나 구조체 인스턴스를 할당해야 할 때가 있다. 이럴 때 인스턴스를 초기화하면서 저장 프로퍼티로 쓰이는 인스턴스들이 한 번에 생성되어야 한다면? 도, 굳이 모든 저장 프로퍼티를 사용할 필요가 없다면? 이 질문의 답이 지연 저장 프로퍼티 사용이다. 지연 저장 프로퍼티를 잘 사용하면 불필요한 성능저하나 공간 낭비를 줄일 수 있다. 

>지연 저장 프로퍼티
```Swift
struct CoordinatePoint {
    var x: Int = 0
    var y: Int = 0
}

class Position {
    lazy var point: CoordinatePoint = CoordinatePoint()
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let yagomPosition: Position = Position(name: "yagom")

// 이 코드를 통해 point 프로퍼티로 처음 접근할 때
// point 프로퍼티의 CoordinatePoint가 생성된다.
print(yagomPosition.point)  // x: 0, y: 0
```

## 연산 프로퍼티
연산 프로퍼티는 실제 값을 저장하는 프로퍼티가 아니라, 특정 상태에 따른 값을 연산하는 프로퍼티이다. 인스턴스 내/외부의 값을 연산하여 적절한 값을 돌려주는 `접근자`(getter)의 역할이나 은닉화된 내부의 프로퍼티 값을 간접적으로 설정하는 `설정자`(setter)의 역할을 할 수도 있다. 클래스, 구조체, 열거형에 연산 프로퍼티를 정의할 수 있다. 

`'굳이 메서드를 두고 왜 연산 프로퍼티를 쓸까?'` 인스턴스 외부에서 메서드를 통해 인스턴스 내부 값에 접근하려면 메서드를 두 개(접근자, 설정자) 구현해야 한다. 또한 이를 감수하고 메서드로 구현한다 해도 두 메서드가 분산 구현되어 코드의 가독성이 나빠질 위험이 있다. 타인의 코드를 보는 프로그래머의 입장에서는 프로퍼티가 메서드 형식보다 훨씬 더 간편하고 직관적이기도 하다. 

다만 연산 프로퍼티는 접근자인 `get` 메서드만 구현해둔 것 처럼 읽기 전용 상태로 구현하기 쉽지만, 쓰기 전용 상태로 구현할 수 없다는 단점이 있다. 메서드로 설장자 메서드만 구현하여 쓰기 전용 상태로 구현할 수 있지만 연산 프로퍼티는 그것이 불가능하다. 

>메서드로 구현된 접근자와 설정자
```Swift
struct CoordinatePoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
    
    // 대칭점을 구하는 메서드 - 접근자
    // Self는 타입 자기 자신을 뜻한다.
    // Self 대신 CoordinatePoint를 사용해도 된다.
    func oppositePoint() -> Self {
        CoordinatePoint(x: -x, y: -y)
    }
    
    // 대칭점을 설정하는 메서드 - 설정자
    mutating func setOppositePoint(_ opposite: CoordinatePoint) {
        x = -opposite.x
        y = -opposite.y
    }
}

var yagomOpsition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

// 현재 좌표
print(yagomOpsition)    // 10, 20

// 대칭 좌표
print(yagomOpsition.oppositePoint())    // -10, -20

// 대칭 좌표를 (15, 10)으로 설정하면
yagomOpsition.setOppositePoint(CoordinatePoint(x: 15, y: 10))

// 현재 좌표는 -15, -10으로 설정된다.
print(yagomOpsition)    // -15, -10
```

oppositePoint() 메서드로 대칭점을 구할 수 있으며 setOppositePoint(_:) 메서드로 대칭점을 설정해줘야 한다. 위 코드에서는 접근자와 설정자 이름의 일관성을 유지하기 힘들며, 해당 포인트에 접근할 때와 설정할 때 사용되는 코드를 한번에 읽기도 쉽지 않다. 

하지만 연산 프로퍼티를 사용하면 이 두 메서드를 좀 더 간결하고 확실하게 표현할 수 있다. 

>연산 프로퍼티의 정의와 사용
```Swift
struct CoordinatePoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
    
    // 대칭 좌표
    var oppositePoint: Self {   // 연산 프로퍼티
        get {   // 접근자
            return CoordinatePoint(x: -x, y: -y)
        }
        
        // 설정자
        set(oppositePoint) {
            x = -oppositePoint.x
            y = -oppositePoint.y
        }
    }
}

var yagomPosition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

// 현재 좌표
print(yagomPosition)    // 10, 20

// 대칭 좌표
print(yagomPosition.oppositePoint)  // -10, -20

// 대칭 좌표를 (15, 10)으로 설정하면
yagomPosition.oppositePoint = CoordinatePoint(x: 15, y: 10)

// 현재 좌표는 -15, -10으로 설정된다.
print(yagomPosition)    // -15, -10
```

이런 식으로 연산 프로퍼티를 사용하면 하나의 프로퍼티에 접근자와 설정자 모두 모여있고, 해당 프로퍼티가 어떤 역할을 하는지 좀 더 명확하게 표현 가능하다. 인스턴스를 사용하는 입장에서도 마치 저장 프로퍼티인 것처럼 편하게 사용할 수 있다. 

설정자의 Parameter로 원하는 이름을 소괄호 안에 명시해주면 `set` 메서드 내부에서 전달받은 Argument를 사용할 수 있다. 관용적인 표현으로 `newValue`로 Parameter 이름을 대신할 수 있다. 그럴 경우에는 Parameter를 따로 표기하지 말아야 한다. 또, 접근자 내부의 코드가 단 한 줄이고, 그 결과값의 타입이 프로퍼티의 타입과 같다면 return 키워드를 생략해도 그 결과값이 접근자의 반환값이 된다. 

>Parameter 이름을 생략한 설정자
```Swift
struct CoordinatePoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
    
    // 대칭 좌표
    var oppositePoint: Self {   // 연산 프로퍼티
        get {   // 접근자
            CoordinatePoint(x: -x, y: -y)
        }
        
        set {   // 설정자
            x = -newValue.x
            y = -newValue.y
        }
    }
}
```

굳이 대칭점을 설정해줄 필요가 없으면 읽기 전용으로 연산 프로퍼티를 사용할 수도 있다. 

>읽기 전용 연산 프로퍼티
```Swift
struct CoordinatePoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
    
    // 대칭 좌표
    var oppositePoint: Self {   // 연산 프러퍼티
        get {   // 접근자
            CoordinatePoint(x: -x, y: -y)
        }
    }
}

var yagomPosition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

// 현재 좌표
print(yagomPosition)    // 10, 20

// 대칭 좌표
print(yagomPosition.oppositePoint)  // -10, -20

// 설정자를 구현하지 않았으므로 오류!!
yagomPosition.oppositePoint = CoordinatePoint(x: 15, y: 10)
```

## 프로퍼티 감시자
`프로퍼티 감시자`(Properties Observers)를 사용하면 프로퍼티의 값이 변경됨에 따라 적절한 작업을 취할 수 있다. 프로퍼티 감시자는 프로퍼티의 값이 새로 할당될 때마다 호출한다. 이때 변경되는 값이 현재의 값과 같더라도 호출한다.

프로퍼티 감시자는 저장 프로퍼티뿐만 아니라 프로퍼티를 재정의해 상속받은 저장 프로퍼티 또는 연산 프로퍼티에도 적용할 수 있다. 물론 상속받지 않은 연산 프로퍼티에는 프로퍼티 감시자를 사용할 필요가 없으며 할 수도 없다. 연산 프로퍼티의 접근자와 설정자를 통해 프로퍼티 감시자를 구현할 수 있기 때문이다. 연산 프로퍼티는 상속 받았을 때만 프로퍼티 재정의를 통해 프로퍼티 감시자를 사용한다. 

프로퍼티 감시자에는 프로퍼티의 값이 변경되기 직전에 호출하는 `willSet` 메서드와 프로퍼티의 값이 변경된 직후에 호출하는 `didSet`메서드가 있다. 

willSet 메서드와 didSet 메서드에는 Parameter가 하나씩 있다. willSet 메서드에 전달되는 Argument는 프로퍼티가 `변경될 값`이고, didSet 메서드에 전달되는 Argument는 프로퍼티가 `변경되기 전의 값`이다. 그래서 Parameter의 이름을 따로 지정하지 않으면 willSet 메서드에는 `newValue`가 didSet 메서드에는 `oldValue`라는 Parameter 이름이 자동 지정된다. 

newValue 혹인 oldValue Parameter 이름 대신에 다른 이름을 사용하고 싶다면 `willSet(newValueName)` 이나 `didSet(oldValueName)`처럼 willSet이나 didSet 다음에 소괄호로 감싼 이름을 적어주면 된다. 

참고로 didSet 감시자 코드 블록 내부에서 oldValue 값을 참조하지 않거나 Parameter 목록에 명시적으로 Parameter를 적어주지 않으면 didSet 코드 블록이 실행되지 않는다. 

>프로퍼티 감시자
```Swift
class Account {
    var credit: Int = 0 {
        willSet {
            print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
        }
        
        didSet {
            print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
        }
    }
}

let myAccount: Account = Account()
// 잔액이 0원에서 1000원으로 변경될 예정입니다.
myAccount.credit = 1000
// 잔액이 0원에서 1000원으로 변경되었습니다.
```

클래스를 상속받았다면 기존의 연산 프로퍼티를 재정의하여 프로퍼티 감시자를 구현할 수도 있다. 연산 프로퍼티를 재정의해도 기존의 연산 프로퍼티 기능(접근자와 설정자, get과 set 메서드)은 동작한다. 

>상속받은 연산 프로퍼티의 프로퍼티 감시자 구현
```Swift
class Account {
    var credit: Int = 0 {
        willSet {
            print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
        }
        
        didSet {
            print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
        }
    }
    
    var dollorValue: Double {   // 연산 프로퍼티
        get {
            Double(credit) / 1000.0
        }
        
        set {
            credit = Int(newValue * 1000)
            print("잔액을 \(newValue)달러로 변경 중입니다.")
        }
    }
}

class ForeignAccount: Account {
    override var dollorValue: Double {
        willSet {
            print("잔액이 \(dollorValue)달러에서 \(newValue)달러로 변경될 예정입니다.")
        }
        
        didSet {
            print("잔액이 \(oldValue)달러에서 \(dollorValue)달러로 변경되었습니다.")
        }
    }
}

let myAccount: ForeignAccount = ForeignAccount()
// 잔액이 0원에서 1000원으로 변결될 예정입니다.
myAccount.credit = 1000
// 잔액이 0원에서 1000원을 변경되었습니다.

// 잔액이 1.0달러에서 2.0달러로 변경될 예정입니다.
// 잔액이 1000원에서 2000원으로 변경될 예정입니다.
// 잔액이 1000원에서 2000원으로 변경되었습니다.
myAccount.dollorValue = 2   // 잔액을 2.0달러로 변경 중입니다.
// 잔액이 1.0달러에서 2.0달러로 변경되었습니다. 
```

만약 프로퍼티 감시자가 있는 프로퍼티를 함수의 입출력 매개변수의 전달인자로 전달한다면 항상 willSet과 didSet 감시자를 호출한다. 함수 내부에서 값이 변경되든 되지 않든 간에 함수가 종료되는 시점에 값을 다시 쓰기 때문이다.

## 전역변수와 지역변수
연산 프로퍼티와 프로퍼티 감시자는 전역변수와 지역변수 모두에 사용할 수 있다. 따라서 프로퍼티에 한정하지 않고, 전역에서 쓰일 수 있는 변수와 상수에도 두 기능을 사용할 수 있다. 함수나 메서드, 클로저, 클래스, 구조체, 열거형 등의 범위 안에 포함되지 않았던 변수나 상수, 즉 프로퍼티를 다루기 전에 계속해서 사용했던 변수와 상수는 모두 전역변수 또는 전역 상수에 해당된다.

이제까지 변수라고 통칭했던 전역변수 또는 지역변수는 `저장변수`라고 할 수 있다. 저장변수는 마치 저장 프로퍼티처럼 값을 저장하는 역할을 한다. 그런제 전역변수나 지역변수를 연산변수로 구현할 수도 있으며, 프로퍼티 감시자를 구현할 수도 있다. 

참고로 전역변수 또는 전역상수는 지연 저장 프로퍼티처럼 처음 접근할 때 최초로 연산이 이루어진다. `lazy` 키워드를 사용하여 연산을 늦출 필요가 없다. 반대로 지역변수 및 지역상수는 절대로 지연 연산되지 않는다. 

>저장변수의 감시자와 연산변수
```Swift
var wonInPocket: Int = 2000 {
    willSet {
        print("주머니의 돈이 \(wonInPocket)원에서 \(newValue)원으로 변경될 예정입니다.")
    }
    
    didSet {
        print("주머니의 돈이 \(oldValue)원에서 \(wonInPocket)원으로 변경되었습니다.")
    }
}

var dollarInPoket: Double {
    get {
        Double(wonInPocket) / 1000.0
    }
    
    set {
        wonInPocket = Int(newValue * 1000.0)
        print("주머니의 달러를 \(newValue)달러로 변경 중입니다.")
    }
}

// 주머니의 돈이 2000원에서 3500원으로 변경될 예정입니다.
// 주머이늬 돈이 2000원에서 3500원으로 변경되었습니다.
dollarInPoket = 3.5    // 주미너의 달러를 3500달러로 변경 중입니다.
```

## 타입 프로퍼티
이제까지 알아본 프로퍼티 개념은 모두 타입을 정의하고 해당 타입의 인스턴스가 생성되었을 때 사용할 수 있는 인스턴스 프로퍼티이다. 인스턴스 프로퍼티는 인스턴스를 새로 생성할 때마다 초깃값에 해당하는 값이 프로퍼티의 값이 되고, 인스턴스마다 다른 값을 지닐 수 있다. 

각각의 인스턴스가 아닌 타입 자체에 속하는 프로퍼티를 `타입 프로퍼티`라고 한다. 타입 프로퍼티는 타입 자체에 영향을 미치는 프로퍼티이다. 인스턴스의 생성 여부와 상관없이 타입 프로퍼티의 값은 하나며, 그 타입의 모든 인스턴스가 공통으로 사용하는 값(C언어의 static constant와 유사), 모든 인스턴스에서 공용으로 접근하고 값을 변경할 수 있는 번수(C 언어의 static 변수와 유사) 등을 정의할 떄 유용하다. 

타입 프로퍼티는 두 가지인데 저장 타입 프로퍼티는 변수 또는 상수로 선언할 수 있으며, 연산 타입 프로퍼티는 변수로만 선언할 수 있다. 저장 타입 프로퍼티는 반드시 초깃값을 설정해야 하며 지연 연산된다. 지연 저장 프로퍼티와는 조금 다르게 다중 스레드 환경이라고 하더라도 단 한 번만 초기화된다는 보장을 받는다. 지연 연산 된다고 해서 lazy 키워드로 표시해주지는 않는다. 

>타입 프로퍼티와 인스턴스 프로퍼티
```Swift
class AClass {
    // 저장 타입 프로퍼티
    static var typeProperty: Int = 0
    
    // 저장 인스턴스 프로퍼티
    var instanceProperty: Int = 0 {
        didSet {
            // Self.typeProperty는
            // AClass.typeProperty와 같은 표현이다.
            Self.typeProperty = instanceProperty + 100
        }
    }
    
    // 연산 타입 프로퍼티
    static var typeComputedProperty: Int {
        get {
            typeProperty
        }
        
        set {
            typeProperty = newValue
        }
    }
}

AClass.typeProperty = 123

let classInstance: AClass = AClass()
classInstance.instanceProperty = 100

print(AClass.typeProperty)  // 200
print(AClass.typeComputedProperty)  // 200
```

타입 프로퍼티는 인스턴스를 생성하지 않고도 사용할 수 있으며 타입에 대항하는 값이다. 그래서 인스턴스에 접근할 필요 없이 타입 이름만으로도 프로퍼티를 사용할 수 있다. 

>타입 프로퍼티의 사용
```Swift
class Account {
    static let dollarExchangeRate: Double = 1000.0  // 타입 상수
    
    var credit: Int = 0 // 저장 인스턴스 프로퍼티
    
    var dollarValue: Double {   // 연산 인스턴스 프로퍼티
        get {
            Double(credit) / Self.dollarExchangeRate
        }
        
        set {
            credit = Int(newValue * Self.dollarExchangeRate)
            print("잔액을 \(newValue)달러로 변경 중입니다.")
        }
    }
}
```

## 키 경로
함수는 일급 객체로서 상수나 변수에 참조를 할당할 수 있다.
```Swift
func someFunction(paramA: Any, paramB: Any) {
    print("someFunction called...")
}

var functionReference = someFunction(paramA:paramB:)
```

또 이렇게 함수를 참조해두고 나중에 원할 때 호출할 수 있고, 다른 함수를 참조하도록 할 수도 있다.
```Swift
functionReference("A", "B") // someFunction called...
functionReference = anohterFunciton(paramA:paramB:)
```

프로퍼티도 이처럼 값을 바로 꺼내오는 것이 아니라 어떤 `프로퍼티 위치`만 참조하도록 할 수 있다. 바로 `키 경로`(KeyPath)를 활용하는 방법이다. 키 경롤르 사용하면 간접적으로 특정 타입의 어떤 프로퍼티 값을 가리켜야 할지 미리 지정해두고 사오ㅛㅇ할 수 있다.

키 경로 타입은 `AnyKeyPath`라는 클래스로부터 파생된다. 제인 말이 확장된 키 경로 타입은 `WritableKeyPath<Root, Value>`와 `ReferenceWritableKeyPaht<Root, Value>`타입이다. 

WritableKeyPath<Root, Value> 타입은 값 타입에 키 경로 타입으로 읽고 쓸 수 있는 경우에 적용되며, ReferenceWritableKeyPath<Root, Value> 타입은 참조 타입, 즉 클래스 타입에 키 경로 타입으로 읽고 쓸 수 있는 경우에 적용된다.

키 경오를 역슬래스(`\`)와 타입, 마침표(`.`) 결로로 구성된다. 여기서 경로는 프로퍼티 이름이라고 생각하면 된다.

>키 경로 타입의 타입 확인
```Swift
class Person {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}

struct Stuff {
    var name: String
    var owner: Person
}

print(type(of: \Person.name))   // ReferenceWritableKeyPath<Person, String>
print(type(of: \Stuff.name))    // WritableKeyPath<Stuff, String>
```