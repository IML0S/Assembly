# 5장 Procedure  

## 1. 프로시저의 개념  
프로시저(Procedure)는 **고급 언어의 함수(Function)** 와 같은 개념으로, 프로그램의 특정 기능을 독립적으로 수행하는 코드 블록이다.  
어셈블리어에서는 `PROC`과 `ENDP`로 정의하며, 호출 시 `CALL` 명령어를 사용한다.  

프로시저의 장점은 다음과 같다.  
- 코드의 재사용성 증가  
- 구조적 설계 가능  
- 유지보수 용이성 향상  

```asm
main PROC
  call MySub
  exit
main ENDP

MySub PROC
  mov eax, 10
  ret
MySub ENDP
```

---

## 2. CALL과 RET 명령어  
`CALL` 명령은 프로시저를 호출하고, 현재의 복귀 주소를 스택에 저장한다.  
`RET` 명령은 스택에서 복귀 주소를 꺼내 원래 위치로 되돌아간다.  

**작동 순서**  
1. CALL 실행 → 복귀 주소를 스택에 PUSH  
2. 프로시저 실행  
3. RET 실행 → 복귀 주소를 POP → 원래 위치로 이동  

```asm
call ExampleSub ; ExampleSub 호출
; ...
ExampleSub PROC
  ; 명령어 수행
  ret
ExampleSub ENDP
```

---

## 3. 스택(Stack)과 레지스터 보존  
스택은 LIFO(Last-In, First-Out) 구조를 가진 메모리 공간이다.  
프로시저 호출 시 스택은 **복귀 주소, 지역 변수, 인수, 레지스터 값** 등을 저장하는데 사용된다.  

### PUSH와 POP  
`PUSH`는 데이터를 스택에 저장하고, `POP`은 스택에서 꺼낸다.  

```asm
push eax    ; EAX 값 저장
push ebx
pop ebx     ; EBX 복원
pop eax     ; EAX 복원
```

스택은 **높은 주소 → 낮은 주소 방향**으로 자란다.  
따라서 `PUSH` 시 ESP는 감소하고, `POP` 시 ESP는 증가한다.

---

## 4. 레지스터 인수 전달 (Register Arguments)  
프로시저에 값을 전달하는 가장 일반적인 방식은 **레지스터를 사용하는 것**이다.  
예를 들어, EAX와 EBX의 값을 더해 반환하는 프로시저는 다음과 같다.

```asm
.data
theSum DWORD ?

.code
main PROC
  mov eax, 5
  mov ebx, 7
  call AddValues
  mov theSum, eax
  exit
main ENDP

AddValues PROC
  add eax, ebx
  ret
AddValues ENDP
```

---

## 5. USES 연산자  
`USES`는 프로시저 내에서 사용되는 레지스터를 지정하여 자동으로 PUSH/POP하도록 한다.  
이를 통해 수동으로 레지스터를 보존하는 수고를 덜 수 있다.

```asm
ArraySum PROC USES esi ecx
  mov eax, 0
L1:
  add eax, [esi]
  add esi, TYPE DWORD
  loop L1
  ret
ArraySum ENDP
```

컴파일러는 자동으로 다음과 같은 코드를 생성한다.
```asm
push esi
push ecx
; ...
pop ecx
pop esi
```

---

## 6. PUSHAD / POPAD  
모든 32비트 범용 레지스터를 한 번에 저장하거나 복원하는 명령어이다.  

| 명령어 | 기능 |
|--------|------|
| PUSHAD | EAX, ECX, EDX, EBX, ESP, EBP, ESI, EDI 순으로 스택에 저장 |
| POPAD  | 반대 순서로 복원 |

```asm
MySub PROC
  pushad
  ; 연산 수행
  popad
  ret
MySub ENDP
```

> ⚠️ EAX는 함수의 반환값으로 자주 사용되므로, 반환값을 덮어쓰지 않도록 주의해야 한다.

---

## 7. 지역 변수 (Local Variables)  
지역 변수는 프로시저 내에서만 유효하며, 스택 공간을 통해 생성된다.  

```asm
MySub PROC
  sub esp, 8         ; 지역 변수 2개 (DWORD 2개) 확보
  mov [esp], 10
  mov [esp+4], 20
  add esp, 8         ; 지역 변수 해제
  ret
MySub ENDP
```

---

## 8. 프로시저의 문서화  
프로시저를 작성할 때는 아래 정보를 주석으로 명시하는 것이 좋다.  
- 프로시저의 역할  
- 입력 (Input Parameters)  
- 출력 (Return Values)  
- 변경되는 레지스터  

```asm
;-------------------------------------------------
; SumArray
; 배열의 합계를 구하는 프로시저
; 입력 : ESI - 배열 주소, ECX - 배열 길이
; 출력 : EAX - 합계
;-------------------------------------------------
SumArray PROC USES ecx esi
  mov eax, 0
L1:
  add eax, [esi]
  add esi, TYPE DWORD
  loop L1
  ret
SumArray ENDP
```

---

## 9. 네스티드 프로시저 (Nested Procedures)  
하나의 프로시저가 다른 프로시저를 호출할 수도 있다.  
이 경우 스택을 통해 복귀 주소가 중첩 관리된다.

```asm
main PROC
  call Proc1
  exit
main ENDP

Proc1 PROC
  call Proc2
  ret
Proc1 ENDP

Proc2 PROC
  mov eax, 1234h
  ret
Proc2 ENDP
```

---

## 10. 링크 라이브러리 (Link Library)  
링크 라이브러리는 미리 작성된 프로시저 집합이다.  
Irvine32.lib는 학습용 라이브러리로, 콘솔 입출력 등 다양한 기능을 제공한다.  

예시:
```asm
INCLUDE Irvine32.inc

main PROC
  mov eax, 1234h
  call WriteHex   ; 16진수 출력
  call Crlf       ; 줄바꿈
  exit
main ENDP
END main
```

---

## 11. 프로시저 호출의 전체 과정 요약  
1. **CALL 실행** – 복귀 주소 PUSH, 프로시저로 이동  
2. **프로시저 실행** – 지역 변수 생성, 레지스터 보존  
3. **RET 실행** – 복귀 주소 POP, 원래 코드로 복귀  

---

## 핵심 정리  
| 명령어 | 기능 요약 |
|--------|------------|
| CALL | 프로시저 호출 |
| RET | 복귀 |
| PUSH/POP | 스택 저장 및 복원 |
| PUSHAD/POPAD | 8개 레지스터 저장/복원 |
| USES | 자동 PUSH/POP 생성 |
| PROC/ENDP | 프로시저 정의 |
| ESP | 스택 포인터 |

---

