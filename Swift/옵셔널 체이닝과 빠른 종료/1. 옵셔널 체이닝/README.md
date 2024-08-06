# 옵셔널 체이닝
옵셔널 체이닝은 옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정이다. 옵셔널 값이 있다면 프로퍼티, 메서드, 서브스크립트 등을 호출할 수 있고, 옵셔널이 nil이라면 프로퍼티, 메서드, 서브스크립트 등은 nil을 반환하다. 즉, 옵셔널을 반복 사용하여 옵셔널이 자전거 체인처럼 서로 꼬리를 물고 있는 모양이기 때문에 옵셔널 체이닝이라고 부른다. 자전거 체인에서 한 칸이라도 없거나 고장 나면 체인 전체가 동작하지 않듯이 `중첩된 옵셔널 중 하나라도 값이 존재하지 않는다면 결과적으로 nil을 반환한다.`

옵셔널 체이닝은 프로퍼티나 메서드 또는 서브스크립트를 호출하고 싶은 옵셔널 변수나 상수 뒤에 물을표(`?`)를 붙여 표현한다. 옵셔널이 nil이 아니라면 정상적으로 호출될 것이고, nil이라면 결과값으로 nil을 반환할 것이다. 결과적으로 nil이 반환될 가능성이 있으므로 `옵셔널 체이닝의 반환된 값은 항상 옵셔널이다.`

<br>

#### NOTE 느낌표(!)
물음표 대신에 느낌표(!)를 사용할 수도 있는데 이는 옵셔널에 값을 강제 추출하는 효과가 있다. 물음표를 사용하는 것과 가장 큰 차이점은 값을 강제 추출하기 때문에 옵셔널에 값이 없다면 런타임 오류가 발생한다는 점이다. 또 다른 점은 옵셔널에서 값을 강제 추출해 반환하기 때문에 반환 값이 옵셔널이 아니라는 점이다. 하지만 100% nil이 아니라는 확인을 하더라도 사용을 지양하는 편이 좋다.

<br>

>사람의 주소 정보 표현 설계
```Swift
// 호실
class Room {
    var number: Int
    
    init(number: Int) { self.number = number }
}

// 건물
class Building {
    var name: String    // 건물 이름
    var room: Room?     // 호실 정보
    
    init(name: String) { self.name = name }
}

// 주소
struct Address {
    var province: String        // 광역시/도
    var city: String            // 시/군/구
    var street: String          // 도로명
    var building: Building?     // 건물
    var detailAddress: String?  // 건물 외 상세 주소
}

//사람
class Person {
    var name: String        // 이름
    var address: Address?   // 주소
    
    init(name: String) { self.name = name }
}
```

>사람 인스턴스 생성
```Swift
let yagom: Person = Person(name: "yagom")
```

yagom이 사는 호실 번호를 알고 싶다. 옵셔널 체이닝과 강제 추출을 사용해서 프로퍼티에 접근해본다.

>옵셔널 체이닝 문법
```Swift
let yagomRoomViaOptionalChaining: Int? = yagom.address?.building?.room?.number  // nil
let yagomRoomViaOptionalUnwraping: Int? = yagom.address!.building!.room!.number // 런타임 오류
```

yagom 인스턴스에는 아직 주소, 건물, 호실 정보가 없다. yagomRoomViaOptionalChaining 상수에 호실 번호를 할당하려고 옵셔널 체이닝을 사용하면 yagom.address 프로퍼티가 nil 이므로 옵셔널 체이닝 도중에 nil이 반환된다. 그러나 yagomRoomViaOptionalUnwraping 상수에 호실 번호를 할당할 떄는 강제 추출을 시도했기 떄문에 nil이 address 프로퍼티에 접근하려 한 떄 런타입 오류가 발생한다.

<br>

>옵셔널 바인딩의 사용
```Swift
let yagom: Person = Person(name: "yagom")

var roomNumber: Int? = nil

if let yagomAddress: Address = yagom.address {
    if let yagomBuilding: Building = yagomAddress.building {
        if let yagomRoom: Room = yagomBuilding.room {
            roomNumber = yagomRoom.number
        }
    }
}

if let number: Int = roomNumber {
    print(number)
} else {
    print("Can not find room number")
}
```

<br>

>옵셔널 체이닝의 사용
```Swift
let yagom: Person = Person(name: "yagom")

if let roomNumber: Int = yagom.address?.building?.room?.number {
    print(roomNumber)
} else {
    print("Can not find room nmber")
}
```

위 두 코드는 완전히 똑같은 결과를 내놓지만, 코드의 간결함과 분량은 꽤 차이가 크다. 옵셔널 체이닝의 결과값은 옵셔널 값이기 떄문에 옵셔널 바인딩과 결합할 수 있다. 

옵셔널 체이닝을 통해 한 단계뿐만 아니라 여러 단계로 복잡하게 중첩된 옵셔널 프로퍼티나 메서드 등에 매번 nil 체크를 하지 않아도 손쉽게 접근할 수 있다. 또 옵셔널 체이닝을 통해 값을 받아오기만 하는 것이 아니라 반대로 값을 할당해줄 수도 있다.

>옵셔널 체이닝을 통한 값 할당 시도
```Swift
yagom.address?.building?.room?.number = 505
print(yagom.address?.building?.room?.number)    // nil
```

위 코드는 아직 yagom의 address 프로퍼티가 없으며 그 하위 building 프로퍼티도 room 프로퍼티도 없다. 그렇기 떄문에 옵셔널 체이닝은 동작 도중에 중지된다. number 프로퍼티는 존재조차 하지 않으므로 505가 할당되지 않는다.

>옵셔널 체이닝을 통한 값 할당
```Swift
yagom.address = Address(province: "충청북도", city: "청주시 청원구", street: "충청대로", building: nil, detailAddress: nil)
yagom.address?.building = Building(name: "곰굴")
yagom.address?.building?.room = Room(number: 0)
yagom.address?.building?.room?.number = 505

print(yagom.address?.building?.room?.number)    // Optional(505)
```

<br>

옵셔널 체이닝을 통해 메서드와 서브스크립트 호출도 가능하다. 서브스크립트는 인덱스를 통해 값을 넣고 빼올 수 있는 기능이다. 

먼저, 옵셔널 체이닝을 통한 메서드 호출이다. 호출 방법은 프로퍼티 호출과 동일하다. 만약 메서드의 반환 타입이 옵셔널이라면 이 또한 옵셔널 체인에서 사용 가능하다.

>옵셔널 체이닝을 통한 메서드 호출
```Swift

```