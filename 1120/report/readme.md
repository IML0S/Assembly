
# 11/20 강의 정리

## 서브루틴(Subroutine) 명칭

  언어    명칭
  ------- ---------------------------
  C/C++   함수(Function)
  Java    메서드(Method)
  MASM    프로시저(Procedure, PROC)

**Arguments(인수/인자)**: 호출자가 전달하는 값\
**Parameters(매개변수)**: 서브루틴이 받는 값

------------------------------------------------------------------------

## 스택 프레임(Stack frame)

프로시저 호출 시 스택에 생성되는 영역.

구성 순서(위 → 아래):

1.  전달된 인수(arguments)\
2.  복귀 주소(return address)\
3.  이전 EBP\
4.  지역 변수(local variables)\
5.  저장된 레지스터들(push)

push/pop은 **4바이트 단위**로 동작.

------------------------------------------------------------------------

## 인수 전달 방식

### ✔ 값 인수 (value)

    push val1

값 그 자체를 push.

### ✔ 참조 인수 (reference)

    push OFFSET val1

주소를 push(C/C++의 `&val1`과 동일).

### ✔ 배열 전달

배열 이름 자체가 배열의 첫 주소 →

    OFFSET array

------------------------------------------------------------------------

## 명시적 스택 매개변수 (explicit stack parameters)

스택 배치:

    [EBP+8]  = 첫 번째 인수
    [EBP+12] = 두 번째 인수
    ...

예:

    x_param EQU [ebp+8]
    y_param EQU [ebp+12]

------------------------------------------------------------------------

## 스택 정리(Stack cleanup)

### ✔ C 언어(cdecl)

Caller가 정리:

    call AddTwo
    add esp, 8

### ✔ STDCALL

Callee가 정리:

    ret 8

------------------------------------------------------------------------

## 지역 변수(Local variables)

EBP 아래에 생성됨:

    [ebp-4], [ebp-8], ...

해당 프로시저에서만 사용 가능.

------------------------------------------------------------------------

## LEA 명령어

간접 주소 계산:

    LEA esi, [ebp-8]

OFFSET은 **컴파일 시간**, LEA는 **런타임 계산**.

------------------------------------------------------------------------

## ENTER / LEAVE

### ENTER n, 0

= 자동 스택 프레임 생성

    push ebp
    mov ebp, esp
    sub esp, n

### LEAVE

= 자동 정리

    mov esp, ebp
    pop ebp

------------------------------------------------------------------------

## LOCAL 지시어

여러 지역 변수 선언:

    LOCAL var1:DWORD, arr:BYTE

ENTER는 이름 없는 공간 확보이고\
LOCAL은 **이름 있는 지역 변수** 생성.

------------------------------------------------------------------------

## 재귀(Recursion)

종료 조건 없으면 무한 재귀:

``` asm
Endless PROC
  call Endless
  ret
Endless ENDP
```

정상 재귀 예:

``` asm
CalcSum PROC
  cmp ecx, 0
  jz L2
  add eax, ecx
  dec ecx
  call CalcSum
L2: ret
CalcSum ENDP
```

------------------------------------------------------------------------

## INVOKE 지시어

### 기존 호출 방식

    push OFFSET array
    push LENGTHOF array
    push TYPE array
    call DumpArray

### INVOKE 사용

    INVOKE DumpArray, OFFSET array, LENGTHOF array, TYPE array

스택 push 순서는 .MODEL 호출 규약(STDCALL)에 따름.

------------------------------------------------------------------------

## ADDR 연산자

    INVOKE Swap, ADDR var1, ADDR var2

-   INVOKE 전용\
-   상수 주소만 가능 (`[ebp+12]` 불가)

------------------------------------------------------------------------

## PROC 지시어 (이름 있는 매개변수)

``` asm
AddTwo PROC val1:DWORD, val2:DWORD
  mov eax, val1
  add eax, val2
  ret 8
AddTwo ENDP
```

PROC + STDCALL → MASM이 자동으로:

    push ebp
    mov ebp, esp
    ...
    ret (인수 크기)

생성.

------------------------------------------------------------------------

## PROTO 지시어

``` asm
AddTwo PROTO val1:DWORD, val2:DWORD
```

INVOKE 보다 **앞에** 있어야 함.

------------------------------------------------------------------------

## 프로시저 숨김(PRIVATE)

기본: 모든 PROC는 public.

숨기기:

    mySub PROC PRIVATE

파일 전체 기본 private:

    OPTION PROC:PRIVATE
    PUBLIC mySub

------------------------------------------------------------------------

## 외부 프로시저 호출(EXTERN)

    EXTERN AddTwo@8 : PROC

@n = 매개변수가 사용하는 스택 바이트 수.

INVOKE 사용 시 PROTO로 대체 가능.

------------------------------------------------------------------------

## 변수/심볼 공유: PUBLIC, EXTERN, EXTERNDEF

### EXPORT:

    PUBLIC count, SYM1

### IMPORT:

    EXTERN count:DWORD, SYM1:ABS

### EXTERNDEF(둘 다 자동)

var.inc:

    EXTERNDEF count:DWORD, SYM1:ABS

------------------------------------------------------------------------

#  요약

-   프로시저는 스택 프레임을 만든다.\
-   인수는 `[ebp+8]`, 지역 변수는 `[ebp-4]`부터.\
-   호출 규약(cdecl, stdcall)에 따라 스택 정리 주체가 다르다.\
-   LOCAL, PROC, PROTO, INVOKE, ADDR로 고급 언어 스타일 구현.\
-   PRIVATE/PUBLIC/EXTERN/EXTERNDEF로 모듈별 캡슐화 가능.\
-   재귀는 스택이 깊어지므로 종료 조건 필수.
