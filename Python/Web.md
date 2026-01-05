# 🌐 클라이언트 - 서버 모델

클라이언트-서버 모델이란 네트워크 상에서 역할을 분리하는 컴퓨팅 아키텍처다.

## 핵심 용어

- **클라이언트 (Client)**: 서비스를 **요청(Request)**하는 손님 (예: 웹 브라우저)
- **서버 (Server)**: 요청을 받아 서비스를 **응답(Response)**하는 주인 (예: 웹 서버)
- **프로토콜 (Protocol)**: 클라이언트와 서버가 서로 대화하기 위한 **공통 언어/규칙** (예: HTTP, TCP/IP)
- **IP 주소 (IP Address)**: 네트워크 상에서 서버가 있는 **위치/주소**
- **포트 (Port)**: 서버 내에서 특정 프로세스가 대기하고 있는 **문(Door) 번호**

---

## DNS (Domain Name System)

DNS는 **사람이 읽을 수 있는 도메인 이름을 실제 IP 주소로 변환**해주는 시스템이다.

예시 도메인:  
`www.mins-coding.com`

컴퓨터는 **오른쪽(뒤)에서부터** 도메인을 해석한다.

1. **.(루트 도메인)**  
   주소의 맨 끝에 항상 숨어 있는 뿌리 (보통 생략됨)
2. **.com (최상위 도메인, TLD)**  
   `.kr`, `.com`, `.org` 등 국가나 용도를 나타냄
3. **mins-coding (도메인 이름)**  
   서비스나 회사의 고유 이름
4. **www (서브 도메인)**  
   서비스 내 용도 구분용 꼬리표

### DNS 레코드

| 종류 | 이름 | 설명 |
| --- | --- | --- |
| A | Address | 도메인 → IPv4 주소 |
| AAAA | Address | 도메인 → IPv6 주소 |
| CNAME | Canonical Name | 도메인의 별명 |
| MX | Mail Exchanger | 이메일 서버 위치 |
| TXT | Text | 도메인 소유 인증 |
| NS | Name Server | 도메인 관리자 |

> Cloudflare 프록시 사용 시  
> 실제 서버 IP를 숨기고 보안과 성능을 향상시킬 수 있다.

---

## IP (Internet Protocol)

IP는 인터넷에서 통신 대상을 식별하는 주소다.

- 일반적으로 **IPv4** 사용
- 32비트 구성
- 8비트씩 나누어 4개의 숫자로 표현
- 각 숫자 범위: 0~255

형식: x.x.x.x


---

## Port

IP 주소가 컴퓨터의 위치라면, 포트 번호는 **프로그램의 입구 번호**다.

### 포트 번호 분류

| 범위 | 이름 | 설명 |
| --- | --- | --- |
| 0 ~ 1023 | Well-known | HTTP(80), HTTPS(443) |
| 1024 ~ 49151 | Registered | MySQL(3306) |
| 49152 ~ 65535 | Dynamic | 클라이언트 임시 포트 |

### 자주 사용하는 포트

- 22 : SSH
- 80 : HTTP
- 443 : HTTPS
- 3306 : MySQL
- 5432 : PostgreSQL

---

## HTTP Protocol

HTTP는 **HyperText Transfer Protocol**의 약자다.

### 택배 시스템으로 HTTP Protocol 살펴보기

| 택배 시스템 | 웹 | 설명 |
|---|---|---|
| 보내는 사람 (jeff) | 클라이언트 (브라우저) | 택배(요청)를 보내는 쪽 |
| 받는 사람 (친구) | 서버 | 택배를 받고 회신을 보내는 쪽 |
| 택배 규정 (배송 절차) | HTTP | 모든 택배사가 따르는 표준 방식 |
| 운송장 (송장) | HTTP 헤더 | 보내는 사람, 받는 사람, 무게 등 정보 |
| 택배 상자 속 물건 | HTTP 본문 (Body) | 실제 전송되는 데이터 |
| 배송 주소 (시/구/동/번지) | URL | 목적지를 특정하는 주소 체계 |
| 배송 상태 (배송중, 배송완료) | 상태 코드 | 처리 결과를 알려주는 코드 |
| 물류센터 | 라우터 | 택배를 중간에서 전달해주는 곳 |


### HTTP 특징

#### 1. 비연결성 (Connectionless)

- 요청-응답 후 연결 종료
- 서버 자원 절약

#### 2. 무상태성 (Stateless)

- 이전 요청 기억 ❌
- 모든 요청은 독립적

---

## curl로 HTTP 요청 보내기

```bash
# GET 요청: 데이터 조회
# -X GET: HTTP 메서드 지정 (GET은 기본값이라 생략 가능)
# -v: verbose 모드, 요청/응답 헤더를 상세히 출력
curl -v https://jsonplaceholder.typicode.com/users/1

# POST 요청: 데이터 생성
# -X POST: POST 메서드 사용
# -H: 헤더 추가 (Content-Type으로 JSON 형식임을 명시)
# -d: 전송할 데이터 (request body)
curl -X POST https://jsonplaceholder.typicode.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "min", "email": "[min@example.com]"}'

# PUT 요청: 데이터 전체 수정
# 기존 사용자 정보를 새 정보로 완전히 대체
curl -X PUT https://jsonplaceholder.typicode.com/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name": "김민석", "email": "[minseok@example.com]"}'

# DELETE 요청: 데이터 삭제
curl -X DELETE https://jsonplaceholder.typicode.com/users/1
```

**curl 옵션 정리**

- `-X` : HTTP 메서드 지정
- `-H` : 헤더 추가
- `-d` : 요청 본문 데이터
- `-v` : 상세 출력 (디버깅용)
- `-i` : 응답 헤더 포함 출력

---

## HTTP Request / Response
### HTTP Request 구조
```bash
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "username": "min",
  "name": "김민석",
  "email": "mins@example.com"
}
```
> GET 요청은 Body 사용을 권장하지 않는다.

### HTTP Method
| Method | 의미    | CRUD   |
| ------ | ----- | ------ |
| GET    | 조회    | Read   |
| POST   | 생성    | Create |
| PUT    | 전체 수정 | Update |
| PATCH  | 부분 수정 | Update |
| DELETE | 삭제    | Delete |

### 각 HTTP Method 상세 설명
**GET - 데이터 조회**
> GET은 서버에서 데이터를 읽어오는 요청이다.

```bash
# GET 요청 예시
# /users/1 경로에서 ID가 1인 사용자 정보를 조회
GET /users/1 HTTP/1.1
Host: [api.example.com](http://api.example.com)
```
- 데이터를 변경하지 않음
- 같은 요청을 여러 번 반복 해도 결과가 동일함(멱등성)
- URL에 파라미터가 노출됨

**POST - 데이터 생성**
> POST는 서버에 새로운 데에터를 생성하는 요청이다.
```bash
# POST 요청 예시
# /users 경로에 새로운 사용자 "임태종"을 생성
POST /users HTTP/1.1
Host: [api.example.com](http://api.example.com)
Content-Type: application/json

{
"name": "김민석",      # 사용자 이름
"email": "[min@example.com]"  # 이메일 주소
}
```

**PUT - 데이터 전체 수정**
> PUT은 해당 위치에 데이터를 완전히 대체한다.
```bash
# PUT 요청 예시
# ID가 1인 사용자의 모든 정보를 새로운 값으로 교체
PUT /users/1 HTTP/1.1
Host: [api.example.com](http://api.example.com)
Content-Type: application/json

{
    "name": "min",       # 이름을 min로 변경
    "email": "newemail@example.com",  # 이메일도 변경
    "age": 28             # 나이 정보 추가
}
```

**PATCH - 데이터 부분 수정**
> PATCH는 데이터의 일부분만 수정한다.
```bash
# PATCH 요청 예시
# ID가 1인 사용자의 이메일만 부분 수정
PATCH /users/1 HTTP/1.1
Host: [api.example.com](http://api.example.com)
Content-Type: application/json

{
    "email": "[updated@example.com]"  # 이메일만 변경, 나머지는 유지
}
```

**DELETE - 데이터 삭제**
> DELETE는 서버의 데이터를 제거한다.
```bash
# DELETE 요청 예시
# ID가 1인 사용자 정보를 삭제
DELETE /users/1 HTTP/1.1
Host: [api.example.com](http://api.example.com)
```


### HTTP Response 구조
```bash
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 1,
  "username": "min",
  "createdAt": "2026-01-04T12:00:00Z"
}
```

### HTTP Status Code
| 범위  | 의미       |
| --- | -------- |
| 1xx | 정보       |
| 2xx | 성공       |
| 3xx | 리다이렉션    |
| 4xx | 클라이언트 오류 |
| 5xx | 서버 오류    |

---

### REST API
>REST는 Representational State Transfer의 약자다.


### REST API의 정의

REST 원칙을 따르는 API

웹에서 클라이언트와 서버가 HTTP 프로토콜을 사용하여 데이터를 주고받을 때 일관된 규칙을 따르도록 설계된 인터페이스

**REST API의 동작 흐름**

<img width="1884" height="972" alt="Image" src="https://github.com/user-attachments/assets/77407f7a-4d86-4b4b-8d76-8786be4b9013" />

- URL은 **자원(Resource)**을 표현
- 행위는 HTTP Method로 표현

**URL 구조**
```bash
https://api.example.com:443/users/jeff?role=admin#profile
```
| 구성 요소    | 설명              |
| -------- | --------------- |
| Scheme   | https           |
| Host     | api.example.com |
| Port     | 443             |
| Path     | /users/jeff     |
| Query    | ?role=admin     |
| Fragment | #profile        |

---

### JSON (JavaScript Object Notation)
>JSON은 데이터를 저장하고 전송하 위한 텍스트 기반 포맷
```bash
  // JSON의 기본 구조
  // 중괄호 {} 안에 "키": 값 쌍을 작성합니다
  // 키는 반드시 큰따옴표(")로 감싸야 합니다
  // 여러 항목은 쉼표(,)로 구분합니다

{
  "name": "min",
  "age": 26,
  "isInstructor": true
}
```

### JSON에서 사용할 수 있는 데이터 타입
| 타입 | 설명 | 예시 |
|---|---|---|
| 문자열 (String) | 텍스트 데이터, 큰따옴표로 감싸야 함 | `"임태종"` |
| 숫자 (Number) | 정수 또는 소수, 따옴표 없이 작성 | `25`, `3.14` |
| 불리언 (Boolean) | 참/거짓 값 | `true`, `false` |
| null | 값이 없음을 표현 | `null` |
| 배열 (Array) | 여러 값의 순서 있는 목록 | `[1, 2, 3]` |
| 객체 (Object) | 키-값 쌍의 집합 | `{"a": 1}` |


### JSON 구조 예제

**단순 객체**
```bash
// 한 사람의 정보를 담은 JSON 객체
// 이름, 직업, 나이 정보를 키-값 쌍으로 표현합니다

{
  "name": "임태종",
  "job": "강사",
  "age": 30
}
```

**배열(Array)**
```bash
// 여러 개의 값을 순서대로 나열할 때 배열을 사용합니다
// 대괄호 [] 안에 값들을 쉼표로 구분하여 작성합니다

{
  "students": ["김철수", "이영희", "박민수"]
}
```

**중첩 구조(Nested Structure)**
```bash
// 객체 안에 또 다른 객체나 배열을 포함할 수 있습니다
// 이를 통해 복잡한 데이터 구조를 표현합니다

{
  "instructor": {
    "name": "jeff",
    "koreanName": "임태종",
    "subjects": ["Python", "Git", "AWS"]
  },
  "studentCount": 25,
  "isActive": true
}
```

핵심 함수 4가지

| 함수 | 방향 | 입력 | 출력 | 용도 |
|---|---|---|---|---|
| `json.dumps()` | 직렬화 | Python 객체 | JSON 문자열 | 네트워크 전송, 로그 출력 |
| `json.dump()` | 직렬화 | Python 객체 | 파일에 저장 | 데이터 영구 저장 |
| `json.loads()` | 역직렬화 | JSON 문자열 | Python 객체 | API 응답 처리 |
| `json.load()` | 역직렬화 | 파일 | Python 객체 | 저장된 데이터 불러오기 |


---
## 직렬화(Serialization), 역직렬화(Deserialization)
> 직렬화란 복잡하게 얽힌 데이터 구조를 일렬로 펼쳐서 저장하거나 전송할 수 있는 형태로 만드는 것이다.

<img width="1668" height="1102" alt="Image" src="https://github.com/user-attachments/assets/57196097-fa49-4775-9dc6-64cf5bb0e6ca" />

**Pydantic**

파이썬에서 “데이터 유효성 검사”와 설정 관리를 위해 사용되는 라이브러리.

쉽게 말해, 외부에서 들어오는 데이터(API 요청, 설정 파일 등)가 우리가 기대한 타입과 형식에 맞는지 확인하고 이를 파이썬 객체로 변환해주는 도구이다.
