# 반환 값을 무시할 수 있는 함수
가끔 함수의 반환 값이 꼭 필요하지 않은 경우도 있다. 프로그래머가 의도적으로 함수의 반환 값을 사용하지 않을 경우 컴파일러가 함수의 결과 값을 사용하지 않았다는 경고를 보낼 때도 있다. 이런 경우 함수의 반환 값을 무시해도 된다는 `@discardableResult` 선언 속성을 사용하면 된다. 

>@discardableResult 선언 속성 사용
```Swift
func say(_ something: String) -> String {
    print(something)
    
    return something
}

@discardableResult func discadableResultSay(_ something: String) -> String {
    print(something)
    
    return something
}

// 반환 값을 사용하지 않았으므로 컴파일러가 경고를 표시할 수 있다.
say("hello")    // hello

// 반환 값을 사용하지 않을 수 있다고 미리 알렸기 때문에
// 반환 값을 사용하지 않아도 컴파일러가 경고하지 않는다.
discadableResultSay("hello")    // hello
```