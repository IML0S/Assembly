# Assembly Practice Answers

## 1. Which instruction pushes all of the 32-bit general-purpose registers on the stack?

32비트 범용 레지스터를 모두 스택에 푸시하는 명령어는 무엇인가?
이 명령은 EAX, ECX, EDX, EBX, ESP, EBP, ESI, EDI 등 모든 32비트 범용 레지스터를 스택에 차례로 저장한다.

정답: PUSHAD

## 2. Which instruction pushes the 32-bit EFLAGS register on the stack?

32비트 EFLAGS 레지스터를 스택에 푸시하는 명령어는 무엇인가?
이 명령은 CPU 상태 플래그가 저장된 EFLAGS 값을 스택에 푸시한다.

정답: PUSHFD

## 3. Which instruction pops the stack into the EFLAGS register?

스택의 값을 꺼내 EFLAGS 레지스터로 복원하는 명령어는 무엇인가?
스택의 맨 위 값을 꺼내서 CPU의 플래그 상태를 복원한다.

정답: POPFD

## 4. Why might NASM’s ability to push multiple specific registers be better than MASM’s PUSHAD instruction?

NASM에서 여러 특정 레지스터를 한 번에 푸시할 수 있는 기능이 MASM의 PUSHAD보다 왜 더 나을까?
NASM은 PUSH EAX EBX ECX처럼 필요한 레지스터만 선택적으로 저장할 수 있다.
따라서 불필요한 레지스터를 스택에 넣지 않아 메모리 사용과 실행 시간이 절약된다.

정답: 필요한 레지스터만 푸시할 수 있어 더 효율적이다.

## 5. If the PUSH instruction didn’t exist, how could you perform the same action as “push eax”?

PUSH 명령이 없다면, push eax와 동일한 동작을 어떻게 구현할 수 있을까?
PUSH는 스택 포인터를 줄이고(ESP 감소) 그 위치에 값을 저장하는 두 단계로 구성된다.
따라서 두 개의 명령으로 대체할 수 있다.

정답:
```asm
sub esp, 4
mov [esp], eax
```

## 6. Does the RET instruction pop the top of the stack into the instruction pointer?

RET 명령이 스택의 맨 위 값을 꺼내 명령 포인터로 복원하는가?
RET은 스택에 저장된 복귀 주소를 EIP(명령 포인터)로 가져와 함수 호출 전 위치로 돌아간다.

정답: True

## 7. Are nested procedure calls disallowed in MASM unless the NESTED operator is used?

MASM에서는 NESTED 연산자를 사용하지 않으면 중첩된 프로시저 호출이 허용되지 않는가?
MASM은 기본적으로 중첩 호출을 허용하므로, NESTED 연산자가 없어도 중첩 호출이 가능하다.

정답: False

## 8. In protected mode, does each procedure call use at least 4 bytes of stack space?

보호 모드에서 각 프로시저 호출은 최소 4바이트의 스택 공간을 사용하는가?
프로시저 호출 시 리턴 주소(32비트)가 스택에 저장되므로 최소 4바이트가 사용된다.

정답: True

## 9. Can the ESI and EDI registers not be used when passing 32-bit parameters to procedures?

ESI와 EDI 레지스터는 32비트 매개변수를 전달할 때 사용할 수 없는가?
ESI와 EDI 역시 일반 범용 레지스터이므로 매개변수 전달에 사용할 수 있다.

정답: False

## 10. (True/False): The ArraySum procedure (Section 5.2.5) receives a pointer to any array of doublewords.

ArraySum 프로시저는 어떤 더블워드 배열의 포인터든 받을 수 있는가?
ArraySum은 배열의 첫 번째 원소 주소(더블워드 배열의 포인터)를 인자로 받아 합계를 계산한다.

정답: True

## 11. (True/False): The USES operator lets you name all registers that are modified within a procedure.

USES 연산자를 사용하면 프로시저 내에서 변경되는 모든 레지스터 이름을 지정할 수 있는가?
USES는 프로시저가 사용하는 레지스터를 나열하면 자동으로 PUSH/POP을 생성해준다.

정답: True

## 12. (True/False): The USES operator only generates PUSH instructions, so you must code POP instructions yourself.

USES 연산자는 PUSH 명령만 생성하므로, POP 명령은 직접 작성해야 하는가?
USES는 PUSH와 POP 모두 자동으로 생성하므로 수동으로 POP을 작성할 필요가 없다.

정답: False

## 13. (True/False): The register list in the USES directive must use commas to separate the register names.

USES 지시어에서 레지스터 이름은 쉼표로 구분해야 하는가?
MASM의 USES에서는 쉼표가 아니라 공백으로 구분한다.

정답: False

## 14. Which statement(s) in the ArraySum procedure (Section 5.2.5) would have to be modified so it could accumulate an array of 16-bit words? Create such a version of ArraySum and test it.

ArraySum 프로시저를 16비트 워드 배열의 합을 계산하도록 하려면 어떤 부분을 수정해야 하는가?
DWORD 대신 WORD 단위를 사용하고, 인덱스 증가량을 +2로 바꾸면 된다.

정답: add esi,2 / 배열 타입을 WORD로 변경

## 15. What will be the final value in EAX after these instructions execute?

```asm
push 5
push 6
pop eax
pop eax
```

이 명령들이 실행된 후 EAX에 남는 최종 값은 무엇인가?
스택은 LIFO이므로, 두 번째 pop eax가 마지막으로 푸시된 5를 꺼낸다.

정답: 00000005h

## 16. Which statement is true about what will happen when the example code runs?

```asm
main PROC
push 10
push 20
call Ex2Sub
pop eax
INVOKE ExitProcess,0
main ENDP

Ex2Sub PROC
pop eax
ret
Ex2Sub ENDP
```

정답: d. The program will halt with a runtime error on Line 11

## 17. Which statement is true about what will happen when the example code runs?

```asm
main PROC
mov eax,30
push eax
push 40
call Ex3Sub
INVOKE ExitProcess,0
main ENDP

Ex3Sub PROC
pusha
mov eax,80
popa 
ret
Ex3Sub ENDP
```

정답: d. The program will halt with a runtime error on Line 13

## 18. Which statement is true about what will happen when the example code runs?

```asm
main PROC
mov eax,40
push offset Here
jmp Here:
Ex4Sub
mov eax,30
INVOKE ExitProcess,0
main ENDP

Ex4Sub PROC
ret
Ex4Sub ENDP
```

정답: c. EAX will equal 30 on line 6

## 19. Which statement is true about what will happen when the example code runs?

```asm
main PROC
mov edx,0
mov eax,40
push eax
call Ex5Sub
INVOKE ExitProcess,0
main ENDP

Ex5Sub PROC
pop eax
pop edx
push eax
ret
Ex5Sub ENDP
```

정답: d. The program will halt with a runtime error on Line 11

## 20. What values will be written to the array when the following code executes?

```asm
.data
array DWORD 4 DUP(0)
.code
main PROC
mov eax,10
mov esi,0
call proc_1
add esi,4
add eax,10
mov array[esi],eax
INVOKE ExitProcess,0
main ENDP

proc_1 PROC
call proc_2
add esi,4
add eax,10
mov array[esi],eax
ret
proc_1 ENDP

proc_2 PROC
call proc_3
add esi,4
add eax,10
mov array[esi],eax
ret
proc_2 ENDP

proc_3 PROC
mov array[esi],eax
ret
proc_3 ENDP
```

정답: array = [10, 20, 30, 40]
