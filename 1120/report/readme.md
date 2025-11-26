# 11/20 강의 정리
# 1. 스택 프레임(Stack Frame)

스택 프레임은 **서브루틴(프로시저)이 실행될 때 스택에 생기는
구조**입니다.

구성 요소(위 → 아래):

1.  **인수(Arguments)** --- 프로시저에 전달된 값\
2.  **복귀 주소(Return Address)** --- 프로시저 종료 후 돌아갈 주소\
3.  **이전 EBP 저장** --- 기존 스택 프레임 보존\
4.  **지역 변수(Local Variables)**\
5.  **저장된 레지스터들(push 한 값)**

➡ 고급 언어의 함수 스택 구조와 동일하며, MASM에서도 똑같이 적용됩니다.

### 왜 필요한가?

-   함수 내부에서 지역 변수, 매개변수, 레지스터 값을 안정적으로 관리하기
    위해서\
-   여러 번 호출되거나 재귀가 실행될 때 **겹치지 않는 구조**가 중요함

------------------------------------------------------------------------

# 2. 인수 전달 방식 --- 값 / 참조

### ✔ 값(Value) 전달

값 자체를 스택에 push

``` asm
push val1
```

### ✔ 참조(Reference) 전달

주소(OFFSET)를 push

``` asm
push OFFSET val1
```

### ✔ 배열 전달

배열 이름 자체가 첫 번째 원소의 주소(포인터)

``` asm
push OFFSET array
```

➡ C/C++에서 배열·포인터 전달 방식과 완전히 동일합니다.

------------------------------------------------------------------------

# 3. 명시적 스택 매개변수

매개변수는 EBP 기준으로 접근한다:

    [EBP+8]  → 첫번째 인자
    [EBP+12] → 두번째 인자

예)

``` asm
x_param EQU [ebp+8]
y_param EQU [ebp+12]
```

➡ 코드 가독성을 위해 많이 사용됨.

------------------------------------------------------------------------

# 4. 호출 규약 (Calling Conventions)

## ✔ CDECL (C 언어 기본 규약)

Caller(호출한 쪽)가 스택 정리

``` asm
call AddTwo
add esp, 8
```

## ✔ STDCALL

Callee(프로시저 내부)가 스택 정리

``` asm
ret 8
```

➡ Windows API는 대부분 STDCALL을 사용

------------------------------------------------------------------------

# 5. 지역 변수(Local Variables)

지역 변수는 EBP 아래에 생성:

    [ebp-4], [ebp-8], ...

특징: - 함수가 끝나면 사라짐\
- 외부에서 접근 불가\
- C/C++ 지역 변수와 동일한 생명주기

------------------------------------------------------------------------

# 6. LEA 명령어

**주소 계산 후 저장** (연산 X)

``` asm
LEA esi, [ebp-8]
```

OFFSET은 컴파일 타임 계산\
LEA는 런타임 계산 가능 → 더 유연함

------------------------------------------------------------------------

# 7. ENTER / LEAVE

### ENTER n, 0

스택 프레임 자동 구성\
(push ebp → mov ebp,esp → sub esp, n)

### LEAVE

스택 프레임 자동 해제\
(mov esp,ebp → pop ebp)

➡ 초보자에게는 편하지만 성능상 비효율적일 수 있어 잘 안 씀

------------------------------------------------------------------------

# 8. LOCAL 지시어

이름을 가진 지역 변수를 선언:

``` asm
LOCAL count:DWORD, buffer:BYTE
```

➡ ENTER로 확보한 "이름 없는 공간"보다 훨씬 편리

------------------------------------------------------------------------

# 9. 재귀(Recursion)

프로시저가 **자기 자신을 호출하는 것**.

종료 조건 없으면 무한 재귀:

``` asm
call Endless
```

정상 재귀 예: CalcSum

``` asm
cmp ecx,0
jz L2
add eax,ecx
dec ecx
call CalcSum
```

➡ 재귀가 끝날 때 스택 프레임이 차례대로 해제되며 값이 전달됨

------------------------------------------------------------------------

# 10. INVOKE

여러 인자를 간단히 호출할 수 있는 MASM의 고급 문법:

### 기존 방식

``` asm
push OFFSET array
push LENGTHOF array
push TYPE array
call DumpArray
```

### INVOKE 방식

``` asm
INVOKE DumpArray, OFFSET array, LENGTHOF array, TYPE array
```

장점: - 파라미터 자동 push\
- 호출 규약에 따른 스택 정리 자동 처리\
- 코드 가독성 ↑

------------------------------------------------------------------------

# 11. ADDR 연산자

INVOKE에서 포인터 인수 전달:

``` asm
INVOKE Swap, ADDR var1, ADDR var2
```

주의: - `[ebp+8]` 같은 표현은 넣을 수 없음\
- INVOKE 전용

------------------------------------------------------------------------

# 12. PROC / PROTO --- 함수 선언과 원형

### PROC (프로시저 정의)

``` asm
AddTwo PROC val1:DWORD, val2:DWORD
  mov eax, val1
  add eax, val2
  ret 8
AddTwo ENDP
```

MASM이 자동으로: - push ebp\
- mov ebp, esp\
- ret 8

등을 생성해 줌(STDCALL 기준)

### PROTO (프로토타입)

``` asm
AddTwo PROTO val1:DWORD, val2:DWORD
```

➡ 후에 등장하는 프로시저를 미리 사용할 수 있게 함\
(C의 함수 원형과 완전히 동일)

------------------------------------------------------------------------

# 13. PRIVATE / PUBLIC / EXTERN / EXTERNDEF

### ✔ PRIVATE

해당 파일 내부에서만 사용

``` asm
mySub PROC PRIVATE
```

### ✔ PUBLIC

다른 모듈에서 사용 가능

``` asm
PUBLIC mySub
```

### ✔ EXTERN

외부 파일에 있는 함수 선언

``` asm
EXTERN AddTwo@8:PROC
```

### ✔ EXTERNDEF

PUBLIC + EXTERN 통합 역할

vars.inc:

``` asm
EXTERNDEF count:DWORD, SYM1:ABS
```

여러 파일에서 공유할 때 매우 유용함.
