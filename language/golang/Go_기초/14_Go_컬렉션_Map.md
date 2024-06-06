#### 1. Map이란
- Map : 키(Key)에 대응하는 값(Value)을 신속히 찾는 해시테이블(Hash table)을 구현한 자료구조
- Go 언어는 Map 타입을 내장하고 있는데, "map[Key타입]Value타입"과 같이 선언할 수 있음
``` go
// 정수를 Key로, 문자열을 값으로 하는 맵 변수
var idMap map[int]string
```

- 이 때 선언된 변수 idMap은 (map은 reference 타입이므로) nil 값을 가짐
- 이를 Nil Map이라 부름
- Nil Map에는 데이터를 쓸 수 없음
- map을 초기화하기 위해 make() 함수를 사용할 수 있음
``` go
idMap = make(map[int]string)
```

- make() 함수의 첫번째 파라미터로 map 키워드와 `[키타입]값타입` 을 지정함
- 이때의 make()함수는 해시테이블 자료구조를 메모리에 생성하고, 그 메모리를 가리키는 map value를 리턴함
- map value는 내부적으로 runtime.hmap 구조체를 가리키는 포인터임
- 따라서 idMap 변수는 이 해시테이블을 가리키는 map을 가리키게 됨
- map은 make() 함수를 써서 초기화할 수도 있지만, 리터럴(literal)을 사용해 초기화할 수도 있음
- 리터럴 초기화 : `map[Key타입]Value타입 { key:value }`
- Map 타입 뒤 { } 괄호 안에 "키:값"들을 열거하면 됨
``` go
// 리터럴을 사용한 초기화
tickers := map[string]string{
    "GOOG": "Google Inc",
    "MSFT": "Microsoft",
    "FB": "FaceBook",
}
```

#### 2. Map 사용
- 처음 map이 make() 함수에 의해 초기화 되었을 때는, 아무 데이터가 없는 상태임
- 이 때 새로운 데이터를 추가하기 위해서는 `map변수[키] = 값` 과 같이 해당 키에 그 값을 할당하면 됨  
ex) 키 901에 Apple을 할당하면, 새 해시 키-값 쌍이 추가됨
- 만약 키 901의 값이 이미 존재했다면, 추가 대신 값만 갱신함
``` go
package main

func main() {
    var m map[int]string

    m = make(map[int]string)
    // 추가 혹은 갱신
    m[901] = "Apple"
    m[134] = "Grape"
    m[777] = "Tomato"

    // 키에 대한 값 읽기
    str := m[134]
    println(str)

    noData := m[999] // 값이 없으면 nil 혹은 zero 리턴
    println(noData)

    // 삭제
    delete(m, 777)
}
```
- map에서 특정 키에 대해 값을 읽을 때는 `map변수[키]`를 읽으면 됨
- 즉, 위 예제에서 m[134]는 Grape라는 값을 str 변수에 할당하게 됨
- 만약 map 안에 찾는 키가 존재하지 않는다면?  
-> reference 타입인 경우 nil을, value 타입인 경우 zero를 리턴함
- map에서 특정 키와 그 값을 삭제하기 위해서는 delete() 함수를 사용함

#### 3. Map 키 체크
- Map을 사용하는 경우, 종종 map 안에 특정 키가 존재하는지를 체크할 필요가 있음
- 이를 위해 `map변수[키]` 읽기를 수행할 때, 2개의 리턴값을 리턴함
- 첫번째 : 키에 상응하는 값
- 두번쨰 : 그 키가 존재하는지 아닌지를 나타내는 bool 값
``` go
package main

func main() {
    tickers := map[string]string{
        "GOOG": "Google Inc",
        "MSFT": "Microsoft",
        "FB": "FaceBook",
        "AMZN": "Amazon",
    }

    // map 키 체크
    val, exists := tickers["MSFT"]
    if !exists {
        println("No MSFT ticker")
    }
}
```

#### 4. for 루프를 사용한 Map 열거
``` go
package main

import "fmt"

func main() {
    myMap := map[string]string{
        "A": "Apple",
        "B": "Banana",
        "C": "Charlie",
    }

    // for range 문을 사용해 모든 맵 요소 출력
    // Map은 unordered 이므로 순서는 무작위
    for key, val := range myMap {
        fmt.Println(key, val)
    }
}
```