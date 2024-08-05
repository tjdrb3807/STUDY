# 탈출 클로저
함수의 전달인자로 전달한 클로저가 함수 종료 후에 호출될 때 클로저가 함수를 `탈출`(Escape)한다고 표현한다. 클로저를 매개변수로 갖는 함수를 선언할 때 매개변수 이름의 콜론(:) 뒤에 `@escaping` 키워드를 사용하여 클로저가 탈출하는 것을 허용한다고 명시해줄 수 있다.

예를 들어 비동기 작업을 실행하는 함수들은 클로저를 `컴플리션 핸들러`(Completion handler) 전달인자로 받아온다. 비동기 작업으로 함수가 종료되고 난 후 호출할 필요가 있는 클로저를 사용해야 할 때 `탈출 클로저`(Escaping Closure)가 필요하다.

@escaping 키워드를 따로 명시하지 않는다면 매개변수로 사용되는 클로저는 기본으로 `비탈출 클로저`(Nonescaping Closure)이다. 함수로 전달된 클로저가 함수의 동작이 끝난 후 사용할 필요가 없을 때 비탈출 클로저를 사용한다. 

클로저가 함수를 탈출할 수 있는 경우 중 하나는 함수 외부에 정의된 변수나 상수에 저장되어 함수가 종료된 후에 사용할 경우이다. 예를 들어 비동기로 작업을 하기 위해서 컴플리션 핸들러를 전달인자로 이용해 클로저 형태로 받는 함수들이 많다. 함수가 작업을 종료하고 난 이후(즉, 함수의 return 후)에 컴플리션 핸들러, 즉 클로저를 호출하기 때문에 클로저는 함수를 탈훌해 있어야만 한다. 함수의 전달인자로 전달받은 클로저를 다시 반환(Return)할 때도 마찬가지다. 

>탈출 클로저를 매개변수로 갖는 함수
```Swift
var completionHandlers: [() -> Void] = []

func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```

>함수를 탈출하는 클로저의 예
```Swift
typealias VoidVoidClosure = () -> Void

let firstClosure: VoidVoidClosure = { print("Closure A") }
let secondClosure: VoidVoidClosure = { print("Closure B") }

// first와 second 매개변수 클로저는 함수의 반환 값으로 사용될 수 있으므로 탈출 클로저이다.
func returnOneClosure(
    first: @escaping VoidVoidClosure,
    second: @escaping VoidVoidClosure,
    shouldRetuenFirstClosure: Bool) -> VoidVoidClosure {
        // 전달인자로 전달받은 클로저를 함수 외부로 다시 반환하기 때문에 함수를 탈출하는 클로저이다.
        shouldRetuenFirstClosure ? first : second
}

// 함수에서 반환한 클로저가 함수 외부의 상수에 저장된다.
let returnedClosure: VoidVoidClosure = returnOneClosure(
    first: firstClosure,
    second: secondClosure,
    shouldRetuenFirstClosure: true)

returnedClosure()   // Closure A

var closures: [VoidVoidClosure] = []

// closure 매개변수 클로저는 함수 외부의 변수에 저장될 수 있으므로 탈출 클로저이다.
func appendClosure(closure: @escaping VoidVoidClosure) { closures.append(closure) }
```

위 코드에서 두 함수의 전달인자로 전달하는 클로저 앞에 @escaping 키워드를 사용하여 탈출 클로저임을 명시해주었다. 이 코드는 클로저 모두가 탈출한 수 있는 조건이 명확하기 때문에 @escaping 키워드를 사용하여 탈출 클로저임을 명시하지 않으면 컴파일 오류가 발생한다. 위 코드는 함수 외부로 다시 전달되어 외부에서 사용이 가능하다든가, 외부 변수에 저장되는 등 클로저의 탈출 조건을 모두 갖추고 있다. 

<br>

타입 내부 매서드의 매개변수 클로저에 @escaping 키워드를 사용하여 탈출 클로저임을 명시한 경우, 클로저 내부에서 해당 타입의 프로퍼티나 메서드, 서브스크립트 등에 접근하려면 `self` 키워드를 명시적으로 사용해야 한다. 비탈출 클로저는 클로저 내부에서 타입 내부의 프로퍼티나 메서드, 서브스크립트 등에 접근할 때 `self` 키워드를 꼭 써주지 않아도 된다. 즉, 비 탈출 클로저 내부에서 self 키워드는 선택 사항이다. 

>클래스 인스턴스 메서드에 사용되는 탈출, 비탈출 클로저
```Swift
typealias VoidVoidClosure = () -> Void

func functionWithNoescapeClosure(closure: VoidVoidClosure) { closure() }

func functionWithEscapingClosure(completionHandler: @escaping VoidVoidClosure) -> VoidVoidClosure {
    completionHandler
}

class SomeClass {
    var x: Int = 10
    
    // 비탈출 클로저에서 self 키워드 사용은 선택 사항이다.
    func runNoescapeClosure() { functionWithNoescapeClosure { x = 200 } }
    
    // 탈출 클로저에서는 명시적으로 self를 사용해야 한다.
    func runEscapingClosure() -> VoidVoidClosure { functionWithEscapingClosure { self.x = 100 } }
}

let instance: SomeClass = SomeClass()
instance.runNoescapeClosure()
print(instance.x)   // 200

let returnedClosure: VoidVoidClosure = instance.runEscapingClosure()
returnedClosure()
print(instance.x)   // 100
```

비탈출 클로저에서는 인스턴스의 프로퍼티인 x를 사용하기 위해 self 키워드를 생략해도 무관했지만, 탈출하는 클로저에서는 값 획득을 하기 위해 self 키워드를 사용하여 프로퍼티에 접근해야한다.

<br>

## withoutActuallyEscaping
비탈출 클로저나 탈출 클로저와 관련한 여러 가지 상황 중 한 가지 애매한 경우가 있다. 비 탈출 클로저로 전달한 클로저가 탈출 클로저인 척 해야 하는 경우이다. 실제로는 탈출하지 않는데 다른 함수에서 탈출 클로저를 요구하는 상황에 해당한다. 

>오류가 발생하는 hasElements 함수
```Swift
func hasElements(in arry: [Int], match predicate: (Int) -> Bool) -> Bool {
    (arry.lazy.filler { predicate($0) }.isEmpty == false)
}
```

hasElement(in:match:) 함수는 @escaping 키워드가 없으므로 비탈출 클로저를 전달받게 된다. 그런데 내부에서 lazy 컬렉션은 비동기 작업을 할 때 사용하기 때문에 filler 메서드가 요구하는 클로저는 탈출 클로저이다. 그래서 탈출 클로저 자리에 비탈출 클로저를 전달할 수 없다는 오류와 마주하게 된다.

그런데 함수 전체를 보면, match 클로저가 탈출할 필요가 없다. 이떄 해당 클로저를 탈출 클로저인양 사용할 수 있게 돕는 `withoutActuallyEscaping(_:do:)` 함수가 있다.

>withoutActuallyEscaping(_:do:) 함수의 활용
```Swift
let numbers: [Int] = [2, 4, 6, 8]

let evenNumberPredicate = { (number: Int) -> Bool in number % 2 == 0 }
let oddNumberPredicate = { (number: Int) -> Bool in number % 2 == 1 }

func hasElements(in arry: [Int], match predicate: (Int) -> Bool) -> Bool {
    withoutActuallyEscaping(
        predicate,
        do: { escapablePredicate in
            (arry.lazy.filter { escapablePredicate($0) }.isEmpty == false)
        })
}

let hasEvenNumber = hasElements(in: numbers, match: evenNumberPredicate)
let hasOddNumber = hasElements(in: numbers, match: oddNumberPredicate)

print(hasEvenNumber)    // true
print(hasOddNumber)     // false
```

`withoutActuallyEscaping(_:do:)` 함수의 첫 번째 전달인자로 탈출 클로저인 척해야 하는 클로저가 전달된다. do 전달 인자는 이 비탈출 클로저를 또 매개변수로 전달받아 실제로 작업을 실행할 탈출 클로저를 전달한다. 이런게 withoutActuallyEscaping(_:do:) 함수를 활용하여 비탈출 클로저를 탈출 클로저처럼 사용할 수 있다.