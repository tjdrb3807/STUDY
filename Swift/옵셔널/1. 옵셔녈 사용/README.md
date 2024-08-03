# 옵셔널 사용
옵셔널 변수 또는 상수가 아니면 `nil`을 할달할 수 없다. 변수 또는 상수에 정말 값이 없을 때만 `nil`로 표현한다. 

함수형 프로그래밍 패러다임에서 자주 등장하는 `모나드` 개념과 일맥상통하다. 

그래서 옵셔널의 사용은 많은 의미를 축약하여 표현하는 것과 같다. 옵셔널을 읽을 때 `'해당 변수 또는 상수에는 값이 없을 수 있다. 즉, 변수 또는 상수가 nil일 수도 있으므로 사용에 주의하라'`는 뜻으로 직관저긍로 받아들일 수 있다. 값이 없는 옵셔널 변수 또는 상수에 (강제로) 접근하면 런타임 오류가 발생한다. 그렇게 되면 OS가 프로그램을 강제 종료시킬 확률이 매우 높다.

>오류가 발생하는 nil 할당
```Swift
var myName: String = "yagom"
myName = nil    // 오류!
```

nil은 옵셔널로 선언된 곳에서만 사용될 수 있다. 옵셔널 변수 또는 상수 등은 데이터 타입 뒤에 물음표(`?`)를 붙여 표현해준다. 

>옵셔널 변수의 선언 및 nil 할당
```Swift
var myName: String? = "yagom"
print(myName)
// 옵셔널 타입의 값을 print 함수를 통해 출력하면
// Optional("yagom") 이라고 출력된다.
// 또, 옵셔널 타입의 값을 print 함수의 Parameter로 전달하면
// 컴파일러 경고가 발생할 수 있다. 
```

사실 `var myName: Optional<String>` 처럼 옵셔널을 조금 더 명확하게 써줄 수도 있다. 그러나 물음표를 붙여주는 것이 조금 더 편하고 읽기도 쉽기 때문에 굳이 긴 표현을 사용하지는 않는다.

옵셔널은 어떤 상황에 사용하는 것인가? 굳이 변수에 nil이 있음을 가정해야 하는가? 이 질문에 답할 수 있는 예로 함수에 전달되는 Argument의 값이 잘못된 값일 경우 제대로 처리하지 못했음을 nil을 반환하여 표현하는 것을 들 수 있다. 물론 기능상 심각한 오류라면 별도로 처리해야겠지만, 간단히 nil을 반환해서 오류가 있음을 알릴 수도 있다. 

스위프트 프로그래밍을 하면서 매개변수가 옵셔널일 때는 'Parameter에는 값이 없어도 되는구나'라는 것을 API 문서를 보지 않고도 알아야 한다. 

옵셔널은 열어형으로 구현되어 있다. 

>옵셔널 열거형의 정의
```Swift
public enum Optional<Wrapped> : ExpressibleBtnilLiteral {
    case none
    case some(Wrapped)
    
    public init(_ some: Wrapped)
    /// 중략...
}
```

옵셔널은 제네릭이 적용된 열거형이다. `ExpressibleByNilLiteral` 프로토콜을 따른다는 것도 확인할 수 있다. 여기서 알아야 할 것은 옵셔널이 값을 갖는 케이스와 그렇지 못한 케이스 두 가지로 정의되어 있다는 것이다. 즉, nil일 때는 none 케이스가 될 것이고, 값이 있을 경우는 some 케이스가 되는데, 연관 값으로 Wrapped가 있다. 따라서 옵셔널에 값이 있으면 some의 연관 값인 Wrapped에 값이 할당된다. 즉, 값이 옵셔널이라는 열거형의 방패막에 보호되어 래핑되어 있는 모습이라는 것이다. 

옵셔널 자체가 열거형이기 때문에 옵셔널 변수는 switch 구문을 통해 값이 있고 없을을 확인할 수 있다. 

>switch를 통한 옵셔널 값의 확인
```Swift
func checkOptionalValue(valeu optionalValue: Any?) {
    switch optionalValue {
    case .none:
        print("This Optional variable is nil")
    case .some(let value):
        print("Value is \(value)")
    }
}

var myName: String? = "yagom"
checkOptionalValue(valeu: myName)   // Value is yagom

myName = nil
checkOptionalValue(valeu: myName)   // This Optional variable is nil
```

여러 케이스의 조건을 통해 검사하고자 한다면 더욱 유용하게 쓰일 수도 있다. 그럴 땐 `where` 절과 병함해서 쓰면 더운 좋다. 

>switch를 통한 옵셔널 값의 확인
```Swift
let numbers: [Int?] = [2, nil, -4, nil, 100]

for number in numbers {
    switch number {
    case .some(let value) where value < 0:
        print("Negative value!! \(value)")
    case .some(let value) where value > 10:
        print("Large value!! \(value)")
    case .some(let value):
        print("Value \(value)")
    case .none:
        print("nil")
    }
}

//Value 2
//nil
//Negative value!! -4
//nil
//Large value!! 100
```