# 기본 클로저
기본 클로저 내용을 포함하여 `sorted(by:)` 메서드를 이용해 동일한 기능을 하는 코드를 어떻게 간결하게 표현하는지 알아본다.

스위프트 표준 라이브러리에는 배열의 값을 정렬하기 위해 구현한 sorted(by:) 메서드가 있다. 이 메서드는 클로저를 통해 어떻게 정렬할 것인가에 대한 정보를 받아 처리하고 결괏값을 배열로 돌려준다. 단순히 정렬만 하기 때문에 입력받은 배열의 타입과 크기가 동일하다. 또한 기존의 배열은 변경하지 않고 정렬된 배열을 새로 생성하여 반환해준다.

>스위프트 라이브러리의 sroted(by:) 메서드 정의
```Swift
public func sorted(by areInIncreasingOrder: (Element, Element) -> Bool) -> [Element]
```

String 타입의 배열에 이름을 넣어 영문 알파벳을 내림차순으로 정렬하려고 한다.

>정렬에 사용될 이름 배열
```Swift
let names: [String] = ["wizplan", "eric", "yagom", "jenny"]
```

sorted(by:) 메서드는 (배열의 타입과 같은 두 개의 Parameter를 가지며 Bool 타입을 반환하는) 클로저를 전달인자로 받을 수 있다. 반환하는 Bool 값은 첫 번째 전달인자 값이 새로 생성되는 배열에서 두 번째 전달인자 값보다 먼저 배치되어야 하는지에 대한 결괏값이다. true를 반환하면 첫 번째 전달인자가 두 번째 전달인자보다 앞에 온다.

>정렬을 위한 함수 전달
```Swift
func backwards(first: String, second: String) -> Bool {
    print("\(first) \(second) 비교중")
    return first > second
}

let reversed: [String] = names.sorted(by: backwards(first:second:))
print(reversed) // ["yagom", "wizplan", "jenny", "eric"]
```

위 코드에서 first > second 라는 반환값을 받기 위해 함수 이름부터 매개변수 표현까지 부가적인 표현이 너무 많다. 이를 클로저 표현을 사용해서 조금 더 간결하게 표현한다.

클로저의 표현은 통상 아래 형식을 따른다
```Swift
{ (매개변수들) -> 반환 타입 in'
    실행 코드
}
```

클로저도 함수와 마찬가지로 입출력 매개변수를 사용할 수 있다. 매개변수 이름을 지정한다면 가변 매개변수 또한 사용 가능하다. 다만 클로저는 매개변수 기본값을 사용할 수 없다. 

>sroted(by:) 메서드에 클로저 전달
```Swift
// backwards(frist:second:) 함수 대신에 sorted(by:) 메서드의 Argument에 클로저를 직접 전달한다.
let reversed: [String] = names.sorted(by: { (first: String, second: String) -> Bool in
    return first > second
})

print(reversed) // ["yagom", "wizplan", "jenny", "eric"]
```

이렇게 프로그래밍하면 sorted(by:) 메서드로 전달되는 backward(first:second:) 함수가 어디에 있는지, 어떻게 구현되어 있는지 찾아다니지 않아도 된다. 물론, 반복해서 같은 기능을 사용하려면 함수로 구현해두는 것도 나쁘지 않다. 이는 개발자의 선택이다.