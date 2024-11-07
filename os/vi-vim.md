---
icon: ghost
---

# Vi / Vim 명령어 정리

명령모드, 입력모드, 마지막행 모드로 구성

1. 명령모드 진입
2. i로 입력모드 진입
3. 입력이 끝나면 esc로 명령모드 진입
4. :로 마지막 행 모드 진입 (저장 w, 종료 q, 취소 i, 저장 후 종료 wq)

### vi 열기

`vi test.txt` : 파일 열기&#x20;

`vi -100 test.txt` : 파일 열고 100번째 행으로 이동&#x20;

`vi -/"abc" test.txt` : 파일 열자마자 문자열 검색 후 커서 이동

&#x20;`vi -r test.txt` : 손상된 파일 복구 `view test.txt` : 읽기 전용으로 열기

### vi 이동

`h`: 왼쪽으로 커서 이동&#x20;

`j`: 아래쪽으로 커서 이동&#x20;

`k`: 위쪽으로 커서 이동&#x20;

`l`: 오른쪽으로 커서 이동



`w`: _<mark style="background-color:red;">word</mark>_ 단어의 시작부분으로 이동(오른쪽으로 이동) \
👉🏻 문장이 `"This is a test"`라고 할 때, 커서가 `"This"`의 `T`에 있을 경우 `w`를 누르면 커서가 `"is"`의 `i`로 이동&#x20;

`e`: _<mark style="background-color:red;">end</mark>_ 단어의 끝으로 이동 \
👉🏻 커서가 `"This"`의 `T`에 있을 때, `e`를 누르면 커서가 `"This"`의 `s`로 이동&#x20;

`b`: _<mark style="background-color:red;">back</mark>_ 이전 단어의 시작으로 이동&#x20;

`ge`: _<mark style="background-color:red;">go to end</mark>_ 이전 단어의 끝으로 이동



`^`: 행의 맨 왼쪽으로 커서 이동&#x20;

`&`: 행의 맨 오른쪽으로 커서 이동



`H`: 페이지 맨 위로 운동&#x20;

`M`: 페이지 중간으로 운동&#x20;

`L`: 페이지 맨 아래로 운동



`Ctrl + u`: 화면의 절반만큼 위로 이동&#x20;

`Ctrl + d`: 화면의 절반만큼 아래로 이동&#x20;

`Ctrl + b`: 한화면 위로 이동&#x20;

`Ctrl + f`: 한화면 아래로 이동
