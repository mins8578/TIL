# [2025-12-24] Git 본질 이해

## ☀️ 오늘의 목표 (Scrum)

> Git과 Github 정확한 개념을 짚고 넘어가자.

<br>

### 1️⃣ 인증(Authentication) vs 인가(Authorization)

- **인증(Authentication)**  
   *“당신은 누구입니까?”*  
  사용자의 **신원을 확인하는 과정**  
  (예: 아이디/비밀번호 로그인, 토큰 검증)

- **인가(Authorization)**  
   *“당신은 무엇을 할 수 있습니까?”*  
  인증된 사용자가 **어떤 권한을 가지는지 결정**  
  (예: 관리자만 접근 가능한 API)

 **핵심 정리**
- 인증 → 신원 확인
- 인가 → 권한 확인
- 보통 **인증 → 인가 순서**로 처리됨

<br>


### 2️⃣ PAT(Personal Access Token)란?

- **PAT 토큰**은  
   **비밀번호 대신 사용하는 긴 문자열 형태의 인증 수단**

- 사람이 직접 로그인할 때는 ID/PW를 쓰지만,
- **프로그램, 스크립트, CI/CD, 자동화 도구**는  
  PAT를 사용해 “이 요청은 내가 보낸 것이다”라고 증명함

 **왜 필요한가?**
- 비밀번호 노출 위험 감소
- 권한 범위 제한 가능 (read/write 등)
- GitHub, GitLab API 호출 시 필수

<br>

### 3️⃣ Git 브랜치의 실체: 포인터(Pointer)

>  **브랜치는 파일 복사가 아니라, 커밋을 가리키는 포인터다**

- **브랜치 = 포인터**: 특정 커밋을 가리킴
- **새 브랜치 생성**: 같은 커밋을 가리키는 포인터 하나 추가 (매우 가벼움)
- **커밋하면**: 현재 브랜치 포인터만 새 커밋으로 이동

- **바로가기 아이콘 비유**
  - 원본 파일은 그대로 두고
  - 위치만 가리키는 역할

 Git의 브랜치도 동일하게  **특정 커밋을 가리키는 표지판** 역할만 한다.

- 브랜치를 만들어도 커밋은 복사되지 않음
- 브랜치를 삭제해도 커밋이 바로 사라지지 않음
- 그래서 브랜치는 **가볍고 빠르다**
  
<br>

### 4️⃣ HEAD, main, feature의 관계

<img width="641" height="232" alt="Image" src="https://github.com/user-attachments/assets/293de74e-7e00-4126-a450-f968ab530802" />

- `HEAD` → **현재 내가 보고 있는 브랜치**
- `main`, `feature` → 각각 특정 커밋을 가리키는 포인터
- 브랜치를 이동한다는 것은  
   **HEAD가 가리키는 포인터를 바꾸는 것**

<br>

### 5️⃣ 브랜치 이동: `git switch`

- 브랜치를 이동할 때 사용하는 명령어
- 과거의 `git checkout`보다 **의도가 명확함**

```bash
git switch main
git switch feature
```

<br>

### 6️⃣ 브랜치 핵심 플로우

<img width="1239" height="315" alt="Image" src="https://github.com/user-attachments/assets/c06e4d7c-d36f-4cc2-afd7-3cf6a33d34bc" />

### 핵심 요약
- **브랜치 = 포인터**: 커밋을 가리키는 가벼운 포인터
- **HEAD**: 현재 내가 서 있는 위치
- **switch vs checkout**: 명확한 역할 분리 (`switch` 권장)
- **브랜치 전략**: feature/, bugfix/ 등 의미 있는 이름 사용

<br>

### 한줄로 보는 키워드 학습
<img width="695" height="418" alt="Image" src="https://github.com/user-attachments/assets/d5c8a698-cd65-4967-9ffd-04c057cc5871" />

<br>

---

<br>

### 🤔 오늘의 회고

> 상황 : git push -u origin main 을 통해 로컬 작업 내용을 원격 저장소로 이동 중 403 에러 발생
>
> 원인 분석 : 인증 실패로 인한 403 에러 발생 -> push 실패
>
> 해결 : 브라우저 기반 인증 (gh auth login)

<br>

### ⬇️해결 과정


1단계: 터미널에 gh auth login 명령어를 입력합니다.

2단계 (설정):

Account: GitHub.com 선택

Protocol: 설정이 간편하고 에러가 적은 HTTPS 선택

Method: 가장 쉽고 안전한 Login with a web browser 선택

3단계 (인증): 터미널에 뜬 일회용 코드(One-time code)를 복사한 뒤 브라우저에 붙여넣고 Authorize github 버튼을 클릭하여 승인했습니다.

<br>

### *️⃣결과 확인

- gh auth status 명령어를 통해 로그인이 정상적으로 완료되었음을 확인했습니다.

- 상태: ✓ Logged in to github.com as [아이디] 메시지와 함께 HTTPS 프로토콜이 성공적으로 설정되었습니다.

- 이후 다시 git push를 시도하여 정상적으로 코드가 업로드되는 것을 확인했습니다.
