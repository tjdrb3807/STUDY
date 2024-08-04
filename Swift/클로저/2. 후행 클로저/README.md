# 후행 클로저
`함수나 메서드의 마지막 전달인자로 위치하는 클로저는 함수나 메서드의 소괄호를 닫은 후 작성해도 된다.` 클로저가 조금 길어지거나 가독성이 조금 떨어진다 싶으면 `후행 클로저`(Trailing Closure) 기능을 사용하면 좋다. XCode에서도 자동완성 기능을 사용하면 자동으로 후행 클로저로 유도한다. 

단, 후행 클로저는 맨 마지막 전달인자로 전달되는 클로저에만 해당되므로 전달인자로 클로저 여러 개를 전달할 때는 맨 마지막 클로저만 후행 클로저로 사용할 수 있다. 또한 sroted(by:) 메서드처럼 `단 하나의 클로저만 전달인자로 전달하는 경우에는 소괄호를 생략할 수 있다.`

또, 매개변수에 클로저가 여러 개 있는 경우, `다중 후행 클로저` 문법을 사용할 수 있다. 다중 후행 클로저를 사용하는 경우, 중괄호를 열고 닫음으로써 클로저를 표현하며, 첫 번째 클로저의 전달인자 레이블은 생략한다.

>후행 클로저 표현
```Swift
// 후행 클로저의 사용
let reversed: [String] = names.sorted() { (first: String, second: String) -> Bool in
    return first > second
}

// sorted(by:) 메서드의 소괄호까지 생략 가능하다.
let reversed: [String] = names.sorted { (first: String, second: String) -> Bool in
    return first > second
}

func doSomething(do: (String) -> Void,
                 onSuccess: (Any) -> Void,
                 onFailed: (Error) -> Void) {
    // do something...
}

// 다중 후행 클로저의 사용
doSomething { (someString: String) in
    // do closure
} onSuccess: { (result: Any) in
    // success closure
} onFailed: { (error: Error) in
    // failure closure
}
```