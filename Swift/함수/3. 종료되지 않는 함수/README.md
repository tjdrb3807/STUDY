# 종료되지 않는 함수
스위프트에는 종료(return)되지 않는 함수가 있다. 

종료되지 않는다는 의미는 `정상적으로 끝나지 않는 함수`라는 뜻이다. 이를 `비반환 함수`(Nonreturning function) 또는 `비반환 메서드`(Nonreturning method)라고 한다. 비반환 함수(메서드)는 정상적으로 끝날 수 없는 함수이다. 이 함수를 실행하면 프로세스 동작은 끝났다고 볼 수 있다.

비반환 함수 안에서는 오류를 던진다든가, 중대한 시스템 오류를 보고하는 등의 일을 하고 프로세스를 종료해 버리기 때문이다. 비반환 함수는 어디서든 호출이 가능하고 `guard`구문의 else 블록에서도 호출할 수 있다. 비반환 메서드는 재정의는 할 수 있지만 비반환 타입이라는 것은 변경할 수 없다.

비반환 함수(메서드)는 반환 타입을 `Never`라고 명시해주면 된다.

>비반환 함수의 정의와 사용
```Swift
func crashAndBurn() -> Never {
    fatalError("Something very, very bad happend")
}

crashAndBurn()  // 프로세스 종료 후 오류 보고

func someFunction(isAllIsWell: Bool) {
    guard isAllIsWell else {
        print("마을에 도둑이 들었습니다.")
        crashAndBurn()
    }
    
    print("All is well")
}

someFunction(isAllIsWell: true)     // All is well
someFunction(isAllIsWell: false)    // 마을에 도둑이 들었습니다.
// 프로세스 종료 후 오류 보고
```

`Never` 타입이 스위프트 표준 라이브러리에서 사용되는 대표적인 예로는 `fatalError` 함수가 있다. 
