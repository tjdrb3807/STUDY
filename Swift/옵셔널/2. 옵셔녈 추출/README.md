# 옵셔널 추출
단 하나의 옵셔널을 switch 구문을 통해 매번 값이 있는지 확인하는 것은 매우 불편하다. 그래서 옵셔널 타입에서 값을 조금 더 안전하고 편리하게 추출하는 방법에 대해 알아보고자 한다. 

열거형의 some 케이스로 숩어있는 `옵셔널의 값을 옵셔널이 아닌 값으로 추출`하는 옵서널 추축(Optional Unwrapping) 방법에 대해 알아본다. 

## 강제 추출
옵셔널 강제 추출(Forced Unwrapping) 방식은 옵셔널의 값을 추출하는 가장 간당하지만 `가장 위험한 방법`이다. 런타입 오류가 일어날 가능성이 가장 높기 때문이다. 또, 옵셔널을 만든 의미가 무색해지는 방법이기도 하다. 

옵셔널의 값을 강제 추출하려면 옵셔널 값의 뒤에 느낌표(`!`)를 붙여주면 값을 강제로 추출하여 반환해준다. 만약 강제 추출 시 옵셔널에 값이 없다면, 즉 nil이라면 런타임 오류가 발생한다.

>옵셔널 값의 강제 추출
```Swift
var myName: String? = "yagom"

// 옵셔널이 아닌 변수에는 옵셔널 값이 들어갈 수 없다. 추출해서 할당해주어야 한다.
var yagom: String = myName!

myName = nil
yagom = myName! // 런타임 오류

// if 구문 등 조건문을 이용해서 조금 더 안전하게 처리해볼 수 있다.
if myName != nil {
    print("My name is \(myName!)")
} else {
    print("myName == nil")
}
// myName == nil
```

런타임 오류의 가능성을 항상 내포하기 때문에 옵셔널 강제 추출 방식은 사용하는 것을 지양한다.

## 옵셔널 바인딩
스위프트는 조금 더 안전하고 세련된 방법으로 `옵셔널 바인딩`(Optional Binding)을 제공한다. 옵셔널 바인딩은 옵셔널에 값이 있는지 확인할 때 사용한다. 만약 옵셔널에 값이 있다면 옵셔널에서 추출한 값을 일정 블록 안에서 사용할 수 있는 상수나 변수로 할당해서 옵셔널이 아닌 형태로 사용할 수 있도록 해준다. 

>옵셔널 바인딩을 사용한 옵셔널 값의 추출
```Swift
var myName: String? = "yagom"

// 옵셔널 바인딩을 통한 임시 상수 할당
if let name = myName {
    print("My name is \(name)")
} else {
    print("myName == nil")
}
// My name is yagom

// 옵셔널 바인딩을 통한 임시 변수 할당
if var name = myName {
    name = "wizplan"    // 변수이므로 내부에서 변경이 가능하다.
    print("My name is \(name)")
} else {
    print("myName == nil")
}
// My name is wizplan
```

위 코드에서는 if 구문을 실행하는 블록 안쪽에서만 name이라는 임시 상수를 사용할 수 있다. 즉, if 블록 밖에서는 사용할 수 없고 else 블록에서도 사용할 수 없다. 그렇기 때문에 위와 아래에서 모두 별도로 name을 사용했지만 충돌이 일어나지 않았다. 또, 상수로 사용하지 않고 변수로 사용하고 십다면 if var를 통해 임시 변수로 할당할 수도 있다. 코드에서는 if 와 else 블록만을 사용했지만, else if 블록도 추가할 수 있다. 

옵셔널 바인딩을 통해 한 번에 여러 옵셔널의 값을 추출할 수도 있다. 쉼표(`,`)를 사용해 바인딩 할 옵셔널을 나열하면 된다. 단, 바인딩하려는 옵셔널 중 하나라도 값이 없다면 블록 내부의 명령문은 실행되지 않는다. 

>옵셔널 바인딩을 사용한 여러 개의 옵셔널 값의 추출
```Swift
var myName: String? = "yagom"
var yourNema: String? = nil

// friend에 바인딩 되지 않으므로 실행되지 않는다.
if let name = myName, let friend = yourNema {
    print("We are friend! \(name) & \(friend)")
}

yourNema = "eric"

if let name = myName, let friend = yourNema {
    print("We are friend! \(name) & \(friend)")
}
// We are friend! yagom & eric
```

## 암시적 추출 옵셔널
때때로 nil을 할당하고 싶지만, 옵셔널 바인딩으로 매번 값을 추출하기 귀찮거나 로직상 nil때문에 런타임 오류가 발생하지 않을 것 같다는 확신이 들 때 nil을 할당해줄 수 있는 옵셔널이 아닌 변수나 상수가 있으면 좋을 것이다. 이때 사용하는 것이 바로 `암시적 추출 옵셔널`(Implicitly Unwrapped Optionals)이다. 

옵셔널을 표시하고자 타입 뒤에 물음표(?)를 사용했지만, 암시적 추출 옵셔널로 사용하려면 타입 뒤에는 느낌표(`!`)를 사용해주면 된다.

암시적 추출 옵셔널로 지정된 타입은 일반 값처럼 사용할 수 있으나, 여전히 옵셔널이기 때문에 nil도 할당해줄 수 있다. 그러나 nil이 할당되어 있을 때 접근을 시도하면 런타임 오류가 발생한다. 

>암시적 추출 옵셔널의 사용
```Swift
var myName: String! = "yagom"
print(myName)   // yagom
myName = nil

// 암시적 추출 옵셔널도 옵셔널이므로 당연히 바인딩을 사용할 수 있다.
if let name = myName {
    print("My name is \(name)")
} else {
    print("myName == nil")
}
// myName == nil

myName.isEmpty // 오류!!
```

옵셔널을 사용할 때는 강제 추출 또는 암시적 추출 옵셔널을 사용하기보다는 옵셔널 바인딩, nil 병합 연산자를 비소해 옵셔널 체이닝 등의 방법을 사용하는 편이 훨씬 안전하다. 또한 이렇게 하는 편이 스위프트의 지향점에 부합하다.