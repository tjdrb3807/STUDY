# 흐름 제어

프로그램을 작성하다 보면 특정 조건에서 코드를 실행해야 하거나 실행하지 말아야 하는 상황이 생기기 마련이다. 또, 특정 명령어를 반복해서 실행해야 하는 일도 종종 발생한다. 이럴때 사용하는 것이 `조건문`과 `반복문`이다. 

스위프트의 흐름 제어 구문에서는 소괄호(`()`)를 대부분 생햑할 수 있다. 물론 사용해도 무관하지만 중괄호(`{}`)는 생략할 수 없다.

## 조건문

조건문 에서는 `if` 구문과 `switch` 구문을 소개한다. 그러나 스위프트의 조건문에서는 `guard` 구문도 있다. 

### if 구문
`if`구문은 대표적으로 `if`, `else` 등의 키워드를 사용하여 구현할 수 있다. `스위프트의 if 구문은 조건의 값이 꼭 Bool 타입이어야 한다.`

> if 구문 기본 구현
```Swift
let first: Int = 5
let second: Int = 5
var biggerValue: Int = 0

if first > second { // 조건 수식을 소괄호로 묶어주는 것은 선택 사항
    biggerValue = first
} else if first == second {
    biggerValue = first
} else if first < second {
    biggerValue = second
} else if first == 5 {
    // 조건을 축족하더라도 이미 first == second라는 조건을 축족해 위에서 실행됨
    // 따라서 실행되지 않음
    biggerValue = 100
}
// 마지막 else는 생략 가능, 물론 else if도 생략 가능
// 즉, else나 else if 없이 if만 단독으로 사용 가능

print(biggerValue) // 5
```

else if는 몇 개가 이어져도 상관없으며 else 블록은 없어도 상관없다. 당연히 위 else if 조건을 충족해 블록 내부의 명령문이 실행되면 다음에 이어지는 else if의 조건을 충족하더라도 싱행되지 않고 조건문을 빠져나온다. if 키워드 뒤에 따라오는 조건수식을 소괄호로 감싸주는 것은 선택 사항이다. 

### swift 구문
`swift`구문도 소괄호(`()`)를 생략할 수 있다. 단 `break` 키워드 사용은 선택 사항이다. 즉 case 내부의 코드를 모두 실행하면 break 없이도 switch 구문이 종료된다는 뜻이다. 이 방식은 예상치 못한 실수를 줄이는 데도 큰 도움이 된다. 따라서 `break`를 쓰지 않고 case를 연속 실행하던 트릭을 더 이상 사용하지 못한다. 스위프트에서 switch 구문의 case를 연속 실행하려면 `fallthrough` 키워드를 사용해야한다. 

```Swift
switch 입력 값 {
case 비교 값 1:
    실행 구분
case 비교 값 2:
    실행 구문
    fallthrough // 이번 case를 마치고 switch 구문을 탈출하지 않고 아래 case로 넘어간다
case 비교 값 3, 비교 값 4, 비교 값 5:   // 한 번에 여러 값과 비교할 수 잇다.
    실행 구문
    break   // break 키워드를 통한 종료는 선택 사항이다.
default:    // 한정된 범위가 명확하지 않다면 default는 필수다.
    실행 구문
}
```

스위프트의 switch 구문의 조건에 다양한 값이 들어갈 수 있다. 다만 각 case에 들어갈 비교 값은 입력 값과 데이터 타입이 같아야 한다. 또, 비교될 값이 명확히 한정적인 값(열거형 값 등)이 아닐 때는 `default`를 꼭 작성해줘야 한다. 또한 각 case에는 범위 연산자를 사용할 수도, `where` 절을 사용하여 조건을 확장할 수도 있다. 

>swifth 구문 기본 구현
```Swift
let integerValue: Int = 5

switch integerValue {
case 0:
    print("Value == zero")
case 1...10:
    print("Value == 1~10")
    fallthrough
case Int.min..<0, 101..<Int.max:
    print("Value < 0 or Value > 100")
    break
default:
    print("10 < Value <= 100")
}

// 결과
// Value == 1~10
// Value < 0 or Valur > 100
```

switch 구문의 입력 값으로 숫자 표현이 아닌 문자, 문자열, 열거형, 튜플, 범위, 패턴이 적용된 타입 등 다양한 타입의 값도 사용 가능하다.

>문자열 switch case 구성
```Swift
let stringValue: String = "Liam Nessson"

switch stringValue {
case "yagom":
    print("He it yagom")
case "Jay":
    print("He is Jay")
case "Jenny", "Joker", "Nova":  // 여러 개의 항목을 한 번에 case로 지정해주는 것도 가능하다.
    print("He or She if \(stringValue)")
default:
    print("\(stringValue) said 'I don't know who you are")
}
```