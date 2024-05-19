#### 1. Go 설치
- Windows에 설치 : msi 파일 다운받아 실행
- msi가 System Path 환경변수를 추가함

#### 2. Go 실행 테스트
- go 프로그램 실행
    ```
    go run test.go
    ```

- 실행파일.exe 생성
    ```
    go build test.go
    ```

#### 3. GO 환경변수 (go env)
- GOROOT : Go 가 설치된 디렉토리를 가리킴.
    - Go 실행파일은 GOROOT/bin 포더에 있음
    - Go의 표준 패키지들은 GOROOT/src 폴더에 설치됨
- GOPATH : Go 프로젝트의 홈 디렉토리 역할을 하는 곳
    - Default : %USERPROFILE%\go
    - GOPATH/pkg : 3rd Party 패키지 및 사용자 정의 패키지 등을 저장하는 곳
    - GOPATH/bin : 3rd Party 실행파일 및 사용자 프로젝트의 go 실행파일을 저장하는 곳
    - 사용자 프로젝트에서 go install을 실행하면 GOPATH/bin 폴더에 저장됨