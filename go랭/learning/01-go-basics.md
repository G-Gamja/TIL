# 01. Go 기초 — JS/TS 개발자를 위한 번역서

> **대상**: JavaScript/TypeScript 3년차 프론트엔드 개발자
>
> 모든 개념을 "JS에서는 이거, Go에서는 저거"로 1:1 대응시킵니다.
> Go를 새로 배우는 게 아니라 **이미 아는 것의 Go 버전**을 익힌다고 생각하세요.

---

## 0. JS 개발자가 Go를 처음 만나면 느끼는 것들

```
"왜 이렇게 불편해?"   → 그게 의도입니다. 명시적인 게 Go의 철학.
"타입이 왜 뒤에 와?"  → 익숙해지면 오히려 읽기 편합니다.
"try-catch가 없다고?" → 네. 그리고 이게 더 안전합니다.
"npm이 없다고?"       → go mod이 있습니다. 훨씬 단순합니다.
"세미콜론 안 써도 돼?" → 네. 컴파일러가 자동으로 넣어줍니다.
```

### Go가 JS와 근본적으로 다른 점

| 구분 | JavaScript | Go |
|------|-----------|-----|
| 실행 방식 | 인터프리터 (V8) | **컴파일** → 바이너리 실행 |
| 타입 시스템 | 동적 (TS로 보완) | **정적** (컴파일 타임에 확정) |
| 에러 처리 | try-catch-finally | **반환값으로 에러 전달** |
| 동시성 | 싱글스레드 + 이벤트루프 | **멀티스레드 (고루틴)** |
| 패키지 관리 | npm / yarn / pnpm | **go mod** (중앙 레지스트리 없음) |
| 빌드 결과물 | .js 파일 (런타임 필요) | **독립 실행 바이너리** (런타임 불필요) |
| null/undefined | 둘 다 있음 | **nil** (하나만 있음) |
| 클래스 | class 키워드 | **없음** (struct + 메서드) |
| 상속 | prototype / extends | **없음** (합성으로 대체) |

---

## 1. 프로젝트 구조: package.json → go.mod

### JS 프로젝트

```
my-app/
├── package.json       ← 의존성 정의
├── node_modules/      ← 의존성 설치 (프로젝트 로컬)
├── tsconfig.json      ← TS 설정
├── src/
│   └── index.ts       ← 진입점
└── dist/              ← 빌드 결과
```

### Go 프로젝트

```
my-app/
├── go.mod             ← package.json 역할
├── go.sum             ← package-lock.json 역할
├── main.go            ← 진입점 (src/index.ts 역할)
└── (바이너리)          ← go build 결과 (단일 실행 파일)
```

```bash
# JS                          # Go
npm init                       go mod init github.com/user/my-app
npm install axios              go get github.com/some/package
npm run build                  go build ./...
npm start                      go run main.go     # 또는 ./my-app
npm test                       go test ./...
```

### go.mod = package.json

```go
// go.mod
module github.com/skip-mev/skipper   // package.json의 "name"

go 1.23                              // "engines": { "node": ">=20" }

require (
    github.com/ethereum/go-ethereum v1.13.0   // "dependencies"
    go.uber.org/zap v1.26.0
)
```

**핵심 차이**: Go는 `node_modules` 같은 거대한 폴더가 프로젝트 안에 없습니다.
의존성은 `$GOPATH/pkg/mod/`에 전역 캐시됩니다 (pnpm의 글로벌 스토어와 비슷).

---

## 2. 기본 구조: React 컴포넌트 → Go 프로그램

### JS/TS

```typescript
// src/index.ts
import { createServer } from 'http';

const server = createServer((req, res) => {
  res.end('Hello!');
});

server.listen(3000);
```

### Go

```go
// main.go
package main              // 모든 Go 파일은 패키지에 속함

import (                  // import 구문 (중괄호로 묶음)
    "fmt"
    "net/http"
)

func main() {             // 프로그램 진입점 (node src/index.ts 와 같음)
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprint(w, "Hello!")
    })
    http.ListenAndServe(":3000", nil)
}
```

**`package main`이 뭔가요?**
- `package main` + `func main()` = **실행 가능한 프로그램** (npm scripts의 진입점)
- `package relayer` 같은 다른 이름 = **라이브러리** (npm 패키지처럼 다른 데서 import)

---

## 3. 변수와 타입

### 타입스크립트와 1:1 비교

```typescript
// TypeScript
const name: string = "Skip";
let age: number = 3;
const active: boolean = true;
const data: number[] = [1, 2, 3];
const meta: Record<string, string> = { key: "value" };
```

```go
// Go
name := "Skip"                           // 타입 추론 (:=)
var age int = 3                          // 명시적 선언
active := true
data := []int{1, 2, 3}                  // 슬라이스 (동적 배열)
meta := map[string]string{"key": "value"} // 맵
```

### 타입 선언 위치가 다르다

```typescript
// TS: 타입이 앞 (왼쪽)
function greet(name: string): string { ... }
```

```go
// Go: 타입이 뒤 (오른쪽)
func greet(name string) string { ... }
```

처음엔 어색하지만, 복잡한 시그니처에서는 Go 방식이 더 읽기 쉽습니다:
```go
func process(ctx context.Context, items []Item, opts ...Option) (Result, error)
// "context와 Item 슬라이스와 옵션들을 받아서 Result와 error를 반환하는 함수"
// 왼쪽→오른쪽으로 자연스럽게 읽힘
```

### 변수 선언 3가지 방식

```go
// 1. 짧은 선언 (:=) — 실무에서 90% 이상 사용
name := "Skip"              // 타입 추론 (TS의 const name = "Skip"과 비슷)

// 2. var 키워드 — 타입을 명시하고 싶을 때, 또는 패키지 레벨에서
var count int64 = 42        // 구체적 타입 지정
var name string             // 제로값 초기화 (빈 문자열 "")

// 3. 패키지 레벨 변수 — 함수 바깥에서 (:= 사용 불가)
var defaultTimeout = 30 * time.Second
```

**주의: `:=`는 함수 안에서만 사용 가능!** 파일 최상위에서는 `var`를 써야 합니다.

### 제로값 (Zero Value) — undefined/null 대신

JS에서 초기화 안 하면 `undefined`이죠? Go에서는 **타입별 제로값**이 들어갑니다.

```go
var i int        // 0        (JS: undefined)
var s string     // ""       (JS: undefined)
var b bool       // false    (JS: undefined)
var sl []int     // nil      (JS: undefined)
var m map[string]int  // nil (JS: undefined)

// nil ≈ null + undefined를 합친 개념
// 하지만! nil은 포인터, 슬라이스, 맵, 인터페이스, 채널에만 사용 가능
// string이나 int는 nil이 될 수 없음 (이건 TS의 strict mode와 비슷)
```

**이게 왜 좋은가요?**
```typescript
// TS에서 이런 버그 겪어보셨죠?
const user = getUser();
console.log(user.name);  // 💥 TypeError: Cannot read property 'name' of undefined
```

```go
// Go에서는 컴파일 타임에 잡힘
var user User
fmt.Println(user.Name)  // 안전: ""가 출력됨 (crash 아님)
```

### JS의 number vs Go의 숫자 타입

JS는 `number` 하나로 모든 숫자를 처리합니다. Go는 **구체적으로 지정**합니다.

```go
var i int       // 플랫폼에 따라 32 or 64비트 (일반 용도)
var i32 int32   // 정확히 32비트 (-2B ~ +2B)
var i64 int64   // 정확히 64비트
var u uint64    // 부호 없는 64비트 (블록 높이 등에 사용)
var f float64   // JS의 number와 동일 (IEEE 754)

// ⚠️ Go는 타입이 다르면 연산 불가!
var a int32 = 10
var b int64 = 20
// c := a + b  // 컴파일 에러!
c := int64(a) + b  // 명시적 변환 필요
```

**이 프로젝트에서 자주 보이는 숫자 타입:**
```go
var blockHeight uint64       // 블록 높이 (항상 양수)
var amount *big.Int          // 토큰 금액 (JS의 BigInt와 같은 개념)
var gasPrice int64           // 가스 가격
var decimals int32           // 소수점 자릿수
```

---

## 4. 문자열

### 템플릿 리터럴 → fmt.Sprintf

```typescript
// TS
const msg = `Hello ${name}, you have ${count} items`;
console.log(msg);
```

```go
// Go — 백틱은 "raw string" (줄바꿈 포함 가능, 변수 보간 없음)
msg := fmt.Sprintf("Hello %s, you have %d items", name, count)
fmt.Println(msg)

// 포맷 동사 (format verbs):
fmt.Sprintf("%s", str)       // string
fmt.Sprintf("%d", num)       // 정수 (decimal)
fmt.Sprintf("%f", float)     // 부동소수점
fmt.Sprintf("%v", anything)  // 아무 타입이나 기본 형태로
fmt.Sprintf("%+v", struct)   // 구조체를 필드명과 함께 출력 (디버깅용!)
fmt.Sprintf("%T", value)     // typeof와 같은 역할 (타입 이름 출력)
```

```go
// 실전에서 자주 쓰는 패턴
log.Printf("릴레이 완료: chain=%s tx=%s 소요시간=%v", chainID, txHash, elapsed)
return fmt.Errorf("체인 %s에서 블록 %d 조회 실패: %w", chainID, height, err)
```

---

## 5. 함수

### 기본 비교

```typescript
// TS
function add(a: number, b: number): number {
  return a + b;
}

const add = (a: number, b: number): number => a + b;
```

```go
// Go
func add(a, b int) int {    // 같은 타입이면 마지막에만 타입 표기
    return a + b
}

// Go에도 익명 함수(화살표 함수 비슷)가 있음
add := func(a, b int) int {
    return a + b
}
```

### 다중 반환값 — Go의 가장 큰 특징

JS에서는 여러 값을 반환하려면 객체나 배열을 씁니다:

```typescript
// TS
function divide(a: number, b: number): { result: number; error: string | null } {
  if (b === 0) return { result: 0, error: "division by zero" };
  return { result: a / b, error: null };
}

const { result, error } = divide(10, 0);
```

```go
// Go — 다중 반환값이 언어 차원에서 지원됨
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

result, err := divide(10, 0)
```

**이 프로젝트의 거의 모든 함수가 `(결과, error)`를 반환합니다.**
이것이 Go의 에러 처리 방식의 근간입니다 (다음 섹션에서 자세히).

### 가변 인자 (Variadic)

```typescript
// TS
function log(...args: string[]) { console.log(args); }
```

```go
// Go
func log(args ...string) { fmt.Println(args) }

// 슬라이스를 펼칠 때 (JS의 spread operator ...와 같음)
items := []string{"a", "b", "c"}
log(items...)   // JS: log(...items)
```

### 함수는 일급 객체

JS처럼 Go에서도 함수를 변수에 담고, 인자로 넘기고, 반환할 수 있습니다.

```go
// 콜백 패턴 (TS의 callback과 동일한 개념)
func retry(attempts int, fn func() error) error {
    for i := 0; i < attempts; i++ {
        if err := fn(); err == nil {
            return nil
        }
    }
    return fmt.Errorf("all %d attempts failed", attempts)
}

// 사용 (JS의 화살표 함수 느낌)
retry(3, func() error {
    return sendTransaction()
})
```

---

## 6. 에러 처리 — try-catch에서 벗어나기

### JS 방식 (익숙한 것)

```typescript
try {
  const result = await fetchData();
  const parsed = JSON.parse(result);
  return parsed;
} catch (error) {
  console.error("Failed:", error);
  throw new Error(`fetchData failed: ${error}`);
}
```

### Go 방식 (적응해야 할 것)

```go
result, err := fetchData()
if err != nil {
    return fmt.Errorf("fetchData 실패: %w", err)
}

parsed, err := parseJSON(result)
if err != nil {
    return fmt.Errorf("JSON 파싱 실패: %w", err)
}

return parsed, nil
```

**"이거 엄청 장황한데요?"** — 맞습니다. 하지만:

1. **에러를 무시할 수가 없음**: Go 컴파일러는 사용하지 않은 변수가 있으면 컴파일 에러
2. **에러 발생 지점이 100% 명확**: try 블록 안 어디서 터졌는지 추적할 필요 없음
3. **서버가 예상치 못하게 죽지 않음**: panic(=crash)이 아니라 에러를 반환

### if err != nil 패턴 (하루에 100번 쓰게 됨)

```go
// 패턴 1: 에러를 감싸서 위로 전달 (가장 흔함)
proof, err := client.GetProof(ctx, chainID)
if err != nil {
    return nil, fmt.Errorf("체인 %s 증명 조회 실패: %w", chainID, err)
}

// 패턴 2: if 안에서 바로 선언 (err의 스코프를 제한)
if err := db.Save(record); err != nil {
    return fmt.Errorf("DB 저장 실패: %w", err)
}

// 패턴 3: 에러를 무시 (확실히 에러가 안 날 때만! 거의 안 씀)
_ , _ = fmt.Fprintf(w, "OK")  // 반환값을 _로 버림
```

### %w 에러 래핑 — JS의 cause와 같은 개념

```typescript
// TS (ES2022)
throw new Error("상위 에러", { cause: originalError });
```

```go
// Go
return fmt.Errorf("상위 에러: %w", originalErr)
// %w = wrap. 원래 에러를 감싸서 에러 체인을 만듦

// 나중에 확인할 때
errors.Is(err, ErrNotFound)           // 체인을 따라가며 특정 에러인지 확인
errors.As(err, &targetErr)            // 체인에서 특정 타입의 에러 추출
```

### 실전: 이 프로젝트에서의 에러 흐름

```go
// handler → service → client → 블록체인 순서로 에러가 전파됨
func (h *Handler) GetRoute(w http.ResponseWriter, r *http.Request) {
    route, err := h.service.FindRoute(r.Context(), req)
    if err != nil {
        // 에러 메시지: "경로 탐색 실패: 토큰 조회 실패: DB 연결 끊김"
        // 에러가 어디서 시작됐는지 한눈에 보임!
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    json.NewEncoder(w).Encode(route)
}
```

---

## 7. 구조체 — class 대신

### TS class vs Go struct

```typescript
// TypeScript
class User {
  constructor(
    public name: string,
    public email: string,
    private age: number = 0
  ) {}

  greet(): string {
    return `Hi, I'm ${this.name}`;
  }
}

const user = new User("Alice", "alice@skip.money", 30);
```

```go
// Go — class, constructor, this 전부 없음
type User struct {
    Name  string    // 대문자 = public (export)
    Email string    // 대문자 = public
    age   int       // 소문자 = private (unexported)
}

// 메서드: func 뒤에 (u *User)를 붙여서 "이 메서드는 User의 것"이라고 선언
func (u *User) Greet() string {
    return fmt.Sprintf("Hi, I'm %s", u.Name)
}

// 생성: "생성자"라는 특별한 문법 대신, 관례적으로 New~ 함수를 만듦
func NewUser(name, email string, age int) *User {
    return &User{
        Name:  name,
        Email: email,
        age:   age,
    }
}

user := NewUser("Alice", "alice@skip.money", 30)
user.Greet()
```

### public / private — 대소문자로 결정

TS의 `public`/`private` 키워드 대신, Go는 **첫 글자의 대소문자**로 결정합니다.

```go
type Relayer struct {
    ChainID string       // 대문자 → 다른 패키지에서 접근 가능 (public)
    client  RPCClient    // 소문자 → 같은 패키지에서만 접근 (private)
}

func (r *Relayer) Start() {}    // 대문자 → public 메서드
func (r *Relayer) validate() {} // 소문자 → private 메서드
```

이건 함수, 변수, 타입 전부 동일합니다:
```go
func ProcessPacket() {}  // public (다른 패키지에서 import 가능)
func validateTx() {}     // private (이 패키지 안에서만)
```

### 구조체 태그 (Struct Tags) — 데코레이터와 비슷한 역할

```go
type Chain struct {
    ChainID string `json:"chain_id" gorm:"uniqueIndex"`
    Name    string `json:"name"     gorm:"not null"`
    IsEVM   bool   `json:"is_evm"   gorm:"default:false"`
}

// json:"chain_id" → JSON 직렬화 시 키 이름을 "chain_id"로
// gorm:"uniqueIndex" → DB에서 유니크 인덱스 생성
```

이것은 TS의 데코레이터(`@Column()`, `@ApiProperty()`)와 비슷한 **메타데이터**입니다.

---

## 8. 포인터 — JS에는 없는 개념 (하지만 어렵지 않음)

### "참조" vs "복사"의 이야기

JS에서 이미 아는 개념입니다:

```typescript
// JS — 원시 타입은 복사됨
let a = 10;
let b = a;    // b는 a의 복사본
b = 20;       // a는 여전히 10

// JS — 객체는 참조됨
const obj1 = { name: "Alice" };
const obj2 = obj1;  // obj2는 obj1을 참조
obj2.name = "Bob";  // obj1.name도 "Bob"이 됨!
```

Go에서는 이 "참조"를 **명시적으로** 표현합니다:

```go
// Go — 기본적으로 모든 게 복사됨
a := 10
b := a      // 복사
b = 20      // a는 여전히 10

user1 := User{Name: "Alice"}
user2 := user1       // ⚠️ 구조체도 통째로 복사됨!
user2.Name = "Bob"   // user1.Name은 여전히 "Alice"
// (JS와 다른 점!)

// 참조를 원하면 포인터(&, *)를 사용
user1 := &User{Name: "Alice"}  // &: "주소를 줘" (포인터 생성)
user2 := user1                  // user2는 같은 User를 가리킴
user2.Name = "Bob"              // user1.Name도 "Bob"!
```

### & 와 * 정리

```go
// & = "주소를 알려줘" (포인터를 만듦)
user := User{Name: "Alice"}
ptr := &user   // ptr은 *User 타입 (User를 가리키는 포인터)

// * = "그 주소에 있는 값을 줘" (포인터를 따라가서 값에 접근)
fmt.Println(*ptr)       // {Alice}
fmt.Println(ptr.Name)   // "Alice" — 필드 접근은 자동으로 역참조됨
```

### 왜 메서드에서 *를 쓰는가?

```go
// ❌ 값 리시버: user의 복사본을 받음 → 수정해도 원본에 반영 안 됨
func (u User) SetName(name string) {
    u.Name = name  // 복사본만 수정됨, 원본은 그대로!
}

// ✅ 포인터 리시버: user의 주소를 받음 → 원본 수정 가능
func (u *User) SetName(name string) {
    u.Name = name  // 원본이 수정됨
}
```

**실전 규칙**: 대부분 `*`(포인터 리시버)를 씁니다. 값 리시버는 작은 불변 타입에서만.

---

## 9. 슬라이스와 맵 — Array와 Object

### 슬라이스 = Array

```typescript
// TS
const chains: string[] = ["cosmos", "ethereum", "solana"];
chains.push("polygon");
const first = chains[0];
const count = chains.length;
chains.forEach(c => console.log(c));
const evmChains = chains.filter(c => c === "ethereum");
const upper = chains.map(c => c.toUpperCase());
```

```go
// Go
chains := []string{"cosmos", "ethereum", "solana"}
chains = append(chains, "polygon")      // push (새 슬라이스 반환!)
first := chains[0]
count := len(chains)

// forEach → for range
for _, c := range chains {
    fmt.Println(c)
}

// filter → 직접 구현 (Go 철학: 제네릭 유틸 대신 명시적 코드)
var evmChains []string
for _, c := range chains {
    if c == "ethereum" {
        evmChains = append(evmChains, c)
    }
}

// map → 직접 구현
upper := make([]string, len(chains))
for i, c := range chains {
    upper[i] = strings.ToUpper(c)
}
```

**"filter, map, reduce 없어요?"** — 네. Go는 의도적으로 이런 고차 함수를 내장하지 않았습니다.
"명시적인 for 루프가 더 읽기 쉽다"는 철학입니다. 처음엔 답답하지만 금방 익숙해집니다.

### 슬라이스 주의사항 (JS 개발자가 실수하는 포인트)

```go
// ⚠️ append는 새 슬라이스를 반환함! 반드시 재할당 필요
chains = append(chains, "new")  // ✅
append(chains, "new")           // ❌ 결과가 사라짐 (컴파일은 됨!)

// ⚠️ 슬라이스 잘라내기 (subslice)는 원본을 공유함
original := []int{1, 2, 3, 4, 5}
sub := original[1:3]  // [2, 3] — 원본의 일부를 참조!
sub[0] = 99           // original도 [1, 99, 3, 4, 5]이 됨!
```

### 맵 = Object / Record

```typescript
// TS
const chainNames: Record<string, string> = {
  "cosmoshub-4": "Cosmos Hub",
  "osmosis-1": "Osmosis",
};

chainNames["noble-1"] = "Noble";          // 추가
const name = chainNames["cosmoshub-4"];   // 조회
delete chainNames["osmosis-1"];           // 삭제
const exists = "cosmoshub-4" in chainNames; // 존재 확인
Object.entries(chainNames).forEach(...)   // 순회
```

```go
// Go
chainNames := map[string]string{
    "cosmoshub-4": "Cosmos Hub",
    "osmosis-1":   "Osmosis",
}

chainNames["noble-1"] = "Noble"            // 추가
name := chainNames["cosmoshub-4"]          // 조회
delete(chainNames, "osmosis-1")            // 삭제

// ⭐ 존재 확인 — 이 패턴 매우 자주 사용!
name, exists := chainNames["cosmoshub-4"]  // 두 번째 반환값이 존재 여부
if !exists {
    fmt.Println("없음")
}

// 순회
for chainID, name := range chainNames {
    fmt.Printf("%s: %s\n", chainID, name)
}
```

**핵심 차이: Go 맵은 순서가 보장되지 않습니다!** 순회할 때마다 순서가 바뀔 수 있습니다.
(JS의 Object는 삽입 순서를 보장하지만, Go의 map은 의도적으로 랜덤)

---

## 10. 제어 구조

### if — 거의 같지만 살짝 다름

```go
// 괄호 () 안 씀
if balance > 0 {
    fmt.Println("있음")
} else {
    fmt.Println("없음")
}

// ⭐ Go만의 패턴: if에서 변수 선언 가능 (스코프가 if 블록으로 제한됨)
if err := doSomething(); err != nil {
    return err
}
// 여기서는 err에 접근 불가

// TS로 비유하면:
// { const err = doSomething(); if (err) return err; }
// 이런 느낌이지만 훨씬 깔끔함
```

### for — 유일한 반복문 (while, do-while 없음)

```go
// 기본 for (JS와 동일)
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// while 스타일
for count < 100 {
    count++
}

// 무한 루프 (서버에서 이벤트 루프 등)
for {
    event := waitForEvent()
    process(event)
}

// for...of 스타일 (range)
for index, value := range items {
    fmt.Printf("[%d] %v\n", index, value)
}

// 인덱스 필요 없으면 _ (TS의 _unused 변수와 같은 개념)
for _, item := range items {
    process(item)
}
```

### switch — break 안 써도 됨!

```typescript
// TS — break 까먹으면 fall-through 버그
switch (chain) {
  case "evm":
    handleEVM();
    break;         // 이거 빠뜨리면 다음 case도 실행됨
  case "cosmos":
    handleCosmos();
    break;
}
```

```go
// Go — 자동으로 break됨! (실수 방지)
switch chain {
case "evm":
    handleEVM()
    // break 필요 없음, 자동으로 여기서 끝남
case "cosmos":
    handleCosmos()
case "solana":
    handleSolana()
default:
    return fmt.Errorf("지원하지 않는 체인: %s", chain)
}
```

---

## 11. 패키지와 import

### JS의 모듈 시스템과 비교

```typescript
// TS — 파일 단위 모듈, named export
// utils.ts
export function formatAmount(n: bigint): string { ... }
export const MAX_RETRIES = 3;

// main.ts
import { formatAmount, MAX_RETRIES } from './utils';
```

```go
// Go — 디렉토리(폴더) 단위 패키지, 대문자 = export
// utils/format.go
package utils

func FormatAmount(n *big.Int) string { ... }  // 대문자 = export
const MaxRetries = 3                          // 대문자 = export
func helper() {}                              // 소문자 = 패키지 내부

// main.go
import "github.com/skip-mev/skipper/utils"

utils.FormatAmount(amount)
utils.MaxRetries
```

**핵심 차이:**
- JS: **파일** 단위 모듈, `export` 키워드로 공개
- Go: **디렉토리** 단위 패키지, **대문자 첫 글자**로 공개

### import 그룹핑

```go
import (
    // 1. 표준 라이브러리
    "context"
    "fmt"
    "time"

    // 2. 외부 라이브러리 (빈 줄로 구분)
    "github.com/ethereum/go-ethereum/common"
    "go.uber.org/zap"

    // 3. 내부 패키지 (빈 줄로 구분)
    "github.com/skip-mev/skipper/relayer/cctp"
)
```

---

## 12. context.Context — AbortController의 진화형

### JS의 요청 취소

```typescript
const controller = new AbortController();
setTimeout(() => controller.abort(), 5000);

const res = await fetch(url, { signal: controller.signal });
```

### Go의 context

```go
// context = AbortController + 타임아웃 + 메타데이터를 합친 것
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()  // 함수 끝나면 자동으로 cancel

result, err := client.GetBlock(ctx, height)
// 5초 안에 응답이 없으면 자동으로 취소됨
```

**왜 거의 모든 함수의 첫 번째 인자가 `ctx context.Context`인가요?**

```go
// 이 프로젝트에서 보게 될 전형적인 함수 시그니처
func (s *Service) FindRoute(ctx context.Context, req RouteRequest) (*Route, error)
func (r *Relayer) Relay(ctx context.Context, packet Packet) error
func (c *Client) GetBalance(ctx context.Context, addr string) (*big.Int, error)
```

HTTP 요청이 들어오면 ctx가 생기고, 그 요청이 처리되는 동안 관련된 모든 함수에 전달됩니다.
클라이언트가 연결을 끊으면 ctx가 취소되고, 진행 중인 DB 쿼리나 RPC 호출도 같이 취소됩니다.

**Express의 `req` 객체가 미들웨어를 타고 내려가는 것과 비슷합니다.**

---

## 13. defer — finally의 Go 버전

```typescript
// TS
let file: FileHandle | null = null;
try {
  file = await open('data.txt');
  // ... 작업 ...
} finally {
  file?.close();   // 에러가 나든 안 나든 반드시 실행
}
```

```go
// Go — defer는 함수가 끝날 때 실행됨 (정상 종료든 에러 반환이든)
func readFile() error {
    file, err := os.Open("data.txt")
    if err != nil {
        return err
    }
    defer file.Close()  // 이 함수가 끝날 때 자동으로 실행

    // 아래에서 뭐가 에러나든 Close()는 반드시 호출됨
    data, err := io.ReadAll(file)
    if err != nil {
        return err  // 여기서 리턴되어도 Close()는 실행됨
    }

    return process(data)  // 여기서 리턴되어도 Close()는 실행됨
}
```

**자주 쓰는 defer 패턴:**
```go
mu.Lock()
defer mu.Unlock()       // 잠금 해제

tx := db.Begin()
defer tx.Rollback()     // 트랜잭션 롤백 (Commit 하면 Rollback은 no-op)

timer := time.Now()
defer func() {
    log.Printf("소요시간: %v", time.Since(timer))
}()
```

---

## 실습 과제

모두 직접 코드를 작성하고 `go run main.go`로 실행해보세요.

### 과제 1: 환경 세팅과 Hello World

```bash
mkdir -p ~/go-practice/01-hello && cd ~/go-practice/01-hello
go mod init hello
```

`main.go`를 만들어서 이름과 나이를 변수로 선언하고 출력하세요.

### 과제 2: TS 코드를 Go로 변환

아래 TypeScript 코드를 Go로 변환하세요:

```typescript
interface Chain {
  chainId: string;
  name: string;
  isEVM: boolean;
}

function getChain(chains: Chain[], id: string): Chain | undefined {
  return chains.find(c => c.chainId === id);
}

const chains: Chain[] = [
  { chainId: "cosmoshub-4", name: "Cosmos Hub", isEVM: false },
  { chainId: "ethereum-1", name: "Ethereum", isEVM: true },
];

const chain = getChain(chains, "cosmoshub-4");
if (chain) {
  console.log(`Found: ${chain.name}`);
} else {
  console.log("Not found");
}
```

**힌트**: Go에서는 `undefined` 대신 `(Chain, bool)` 또는 `(Chain, error)`를 반환합니다.

### 과제 3: 에러 처리 체인

세 단계로 이루어진 작업을 만들어보세요:
1. `fetchData(url string) (string, error)` — URL이 빈 문자열이면 에러
2. `parseData(raw string) (Data, error)` — 데이터가 너무 짧으면 에러
3. `processData(data Data) error` — 처리 후 결과 출력

각 에러를 `%w`로 감싸서 전파하고, main에서 최종 에러 메시지가 어떻게 보이는지 확인하세요.

### 과제 4: 맵과 슬라이스 활용

체인 목록(`[]Chain`)을 받아서 `map[string]Chain` (ChainID → Chain 매핑)으로 변환하는
함수를 작성하세요. TS의 `Array.reduce()`로 하던 작업의 Go 버전입니다.

---

## 다음 단계

Go의 기본 문법이 JS와 어떻게 대응되는지 이해했으니,
[02-go-advanced.md](02-go-advanced.md)에서 Go만의 강력한 기능을 배웁니다:
- 인터페이스 (TS의 interface와 비슷하지만 근본적으로 다름)
- 고루틴과 채널 (async/await를 완전히 대체하는 동시성 모델)
- 제네릭 (TS 제네릭의 Go 버전)
- 테스트 (Jest 없이 내장 테스트)
