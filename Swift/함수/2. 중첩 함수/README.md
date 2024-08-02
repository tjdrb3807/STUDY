# 중첩 함수
스위프트는 데이터 타입의 중첩이 자유롭다. 예를 들어 열거형 안에 또 하나의 열거형이 들어갈 수 있고 클래스 안에 또 다른 클래스가 들어올 수 있는 등 다른 프로그래밍 언어에서 생각하지 못했던 패턴을 자유롭게 만들어볼 수 있다. 

함수 안의 함수로 구현된 중첩 함수는 상위 함수의 몸통 블록 내부에서만 함수를 사용할 수 있다. 물론 중첩 함수의 사용 범위가 해당 함수의 안쪽이라고 해서 아예 외부에서 사용할 수 없는 것은 아니다. 함수가 하나의 반환 값으로 사용될 수 있으므로 중첩 함수를 담은 함수가 중첩 함수를 반환하면 밖에서도 사용할 수 있다. 

>원점으로 이동하기 위한 함수
```Swift
typealias MoveFunc = (Int) -> Int

func goRight(_ currentPosition: Int) -> Int {
    return currentPosition + 1
}

func goLeft(_ currentPosition: Int) -> Int {
    return currentPosition - 1
}

func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc {
    return shouldGoLeft ?  goLeft(_:) : goRight(_:)
}

var position: Int = 3   // 현 위치

// 현 위치가 0보다 크므로 전달되는 인자 값은 true가 된다.
// 그러므로 goLeft(_:) 함수가 할당될 것이다.
let moveToZero: MoveFunc = functionForMove(position > 0)

// 원점에 도착하면(현 위치가 0이면) 반복문이 종료된다.

while position != 0 {
    print("\(position)...")
    position = moveToZero(position)
}

print("원점 도착!")
```

위 코드는 지금까지 함수를 구현하던 방식이다. 그런데 왼쪽으로 이동하는 함수와 오른쪽으로 이동하는 함수는 아주 사소한 기능의 차이일 뿐 원점을 찾아가는 목적은 같다. 따라서 굳이 모듈 전역에서 사용할 필요가 없다. 그래서 사용 범위를 한정하고자 함수를 하나의 함수 안쪽으로 배치하여 중첩 함수로 구현하고, 필요할 때만 외부에서 사용할 수 있도록 구현한다. 

>중첩 함수의 사용
```Swift
typealias MoveFunc = (Int) -> Int

func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc {
    func goRight(_ currentPosition: Int) -> Int {
        return currentPosition + 1
    }
    
    func goLeft(_ currentPosition: Int) -> Int {
        return currentPosition - 1
    }
    
    return shouldGoLeft ? goLeft(_:) : goRight(_:)
}

var position: Int = -4  // 현 위치

// 현 위치가 0보다 작으므로 전달되는 인자 값은 false가 된다.
// 그러므로 goRight(_:) 함수가 할당될 것이다.

let moveToZero: MoveFunc = functionForMove(position > 0)

// 원점에 도착하면(현 위치가 0이면) 반복문이 종료된다.

while position != 0 {
    print("\(position)...")
    position = moveToZero(position)
}

print("원점 도착!")
```

전역함수가 많은 큰 프로젝트에서는 전역으로 사용이 불필요한 goRight(_:) 함수와 goLeft(_:) 함수의 사용 범위를 조금 더 명확하고 깔끔하게 표현해줄 수 있다. 