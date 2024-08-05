# 값 획득
클로저는 자신이 정의된 위치(블록)의 주변 문맥을 통해 상수나 변수를 `획득`(Capture) 할 수 있다. 값 획득을 통해 클로저는 주변에 정의한 상수나 변수가 더 이상 존재하지 않더라도 해당 상수나 변수의 값을 자신 내부에서 `참조`하거나 수정할 수 있다. 클로저를 통해 비동기 콜백(Call-back)을 작성하는 경우, 현재 상태를 미리 획득해두지 않으면, 실제로 클로저의 기능을 실행하는 순간에는 주변의 상수나 변수가 이미 메모리에 존재하지 않는 경우가 발생한다. 

중첩 함수도 하나의 클로저 형태이며, 중첩 함수 주변의 변수나 상수를 획득해 놓을 수도 있다. 즉, 자신을 포함하는 함수의 지역변수나 지역 상수를 획득할 수 있다. 

>makeIncrementer(forIncrement:) 함수
```Swift
func makeIncrementer(forIncrement amount: Int) -> (() -> Int) {
    print("CALL: makeIncrementer(forIncrement:)")
    var runningTotal: Int = 0
    
    func incrementer() -> Int {
        print("CALL: incrementer()")
        runningTotal += amount
        
        return runningTotal
    }
    
    return incrementer
}
```

incrementer()함수는 makeIncrementer(forIncrement:) 함수 영역에 포함된 amount와 runningTotal 변수의 참조를 획득할 수 있다. 참조를 획득하면 runningTotal과 amount는 makeIncrementer(forIncrement:) 함수의 실행이 끝나도 사라지지 않는다. 게다라 incrementer() 함수가 호출될 때마다 계속해서 두 변수를 사용할 수 있다. 

>incrementByTwo 상수에 함수 할당
```Swift
let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2) // CALL: makeIncrementer(forIncrement:)

let first: Int = incrementByTwo()   // CALL: incrementer(), runningTotle = 2
let second: Int = incrementByTwo()  // CALL: incrementer(), runningTotle = 4
let third: Int = incrementByTwo()   // CALL: incrementer(), runningTotle = 6
```

incrementByTwo라는 이름의 상수에 incrementer 함수를 할당했다. incrementByTwo를 호출할 때마다 runningTotal은 값이 2씩 증가한다. 위 코드 아래에 incrementer를 하나 더 생성해주면, incrementByTwo와는 별개의 다른 참조를 갖는 runningTotal 변수값을 확인할 수 있다.

>각각의 incrementer의 동작
```Swift
let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)
let incrementByTwo2: (() -> Int) = makeIncrementer(forIncrement: 2)
let incrementByTen: (() -> Int) = makeIncrementer(forIncrement: 10)

let first: Int = incrementByTwo()   // 2
let second: Int = incrementByTwo()  // 4
let third: Int = incrementByTwo()   // 6

let first2: Int = incrementByTwo2()  // 2
let second2: Int = incrementByTwo2() // 4
let third2: Int = incrementByTwo2()  // 6

let ten: Int = incrementByTen()     // 10
let twenty: Int = incrementByTen()  // 20
let thirty: Int = incrementByTen()  // 30
```

각각의 incrementer 함수는 언제 호출이 되더라도 자신만의 runningTotal 변수를 갖고 카운트하게 되며, 다른 함수의 영향도 전혀 받지 않는다. 이는 각각 자신만의 runningTotal 변수의 참조를 미리 획득했기 때문이다. 

<br>

### NOTE 클래스 인스턴스 프로퍼티로서의 클로저
클래스 인스턴스의 프로퍼티로 클로저를 할당하면 클로저는 해당 인스턴스 또는 인스턴스의 멤버의 참조를 획득할 수 있으나, 클로저와 인스턴스 사이에 `강한참조 순환`문제가 발생할 수 있다. 강한참조 순환 문제는 `획득목록을 통해 없앨 수 있다.`