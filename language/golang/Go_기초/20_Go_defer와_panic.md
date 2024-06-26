#### 1. 지연실행 defer
- 특정 문작 혹은 함수를 나중에 (defer를 호출하는 함수가 리턴하기 직전에) 실행하게 됨
- 일반적으로 C#, java같은 언어에서의 finally 블럭처럼 마지막에 Clean-up 작업을 위해 사용
``` go
package main

import "os"

func main() {
    f, err := os.Open("1.txt")
    if err != nil {
        panic(err)
    }

    // main 마지막에 파일 close 실행
    defer f.Close()

    // 파일 읽기
    bytes := make([]byte, 1024)
    f.Read(bytes)
    println(len(bytes))
}
```

#### 2. panic 함수
- 현재 함수를 즉시 멈추고, 현재 함수에 defer 함수들을 모두 실행한 후 즉시 리턴
- 이러한 panic 모드 실행 방식은 다시 상위함수에도 똑같이 적용되고, 계속 콜스택을 타고 올라가며 적용됨
- 마지막에는 프로그램이 에러를 내고 종료하게 됨
``` go
package main

import "os"

func main() {
    // 잘못된 파일명을 넣음
    openFile("Invalid.txt")

    // openFile() 안에서 panic이 실행되면
    // 아래 println 문장은 실행 안됨
    println("Done")
}

func openFile(fn string) {
    f, err := os.Open(fn)
    if err != nil {
        panic(err)
    }

    defer f.Close()
}
```

#### 3. recover 함수
- panic 함수에 의한 패닉상태를 다시 정상상태로 되돌리는 함수
- 위의 panic 예제에서는 main 함수에서 println()이 호출되지 못하고, 프로그램이 crash함
- recover 함수를 사용하면 panic 상태를 제거하고 openFile()의 다음 문장인 println()을 호출하게 됨
``` go
package main

import (
    "fmt"
    "os"
)

func main() {
    // 잘못된 파일명을 넣음
    openFile("Invalid.txt")

    // recover에 의해 이 문장이 실행됨
    println("Done")
}

func openFile(fn string) {
    // defer 함수. panic 호출시 실행됨
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("OPEN ERROR", r)
        }
    }()

    f, err := os.Open(fn)
    if err != nil {
        panic(err)
    }
    
    defer f.Close()
}
```