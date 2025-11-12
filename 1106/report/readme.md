# 6. Conditional Processing 요약

---

## 기본 개념
- 어셈블리에서는 조건 분기(Conditional Branching)로 모든 논리 구조를 표현할 수 있다.
- CMP, TEST, AND, OR, XOR, NOT 같은 논리/비교 명령과 Jxx (조건 점프) 명령으로 if, while 등을 구현한다.

---

### AND
- 특정 비트만 유지(필요한 비트만 남기기)에 사용한다.
- 오버플로우, 캐리 플래그는 항상 0으로 만든다.
- 플래그 영향: S, Z, P 변경 / O, C=0
- 예: `and al, 0Fh` → 하위 4비트만 유지.
- 아스키 대소문자 변환에도 사용 (6번째 비트를 0으로 → 소문자→대문자).

### OR
- 두 비트 중 하나라도 1이면 결과 1.
- 집합의 합집합(union) 표현 가능.
- C, O 플래그 = 0 / S, Z, P 수정.

### XOR
- 비트가 다르면 1, 같으면 0.
- 배타적 OR, 토글(toggle), 암호화에 자주 사용.
- `xor eax, eax` → 0 만드는 빠른 방법.
- C, O = 0 / S, Z, P 수정.

### NOT
- 모든 비트 반전 (1의 보수).
- 플래그 변화 없음.

### TEST
- AND처럼 비트를 검사하지만 결과를 저장하지 않는다.
- 주로 플래그 설정용 (ZF, SF, PF 등).
- 예: `test al, 1` → al의 마지막 비트가 1인지 확인.
- C, O=0.

### CMP
- SUB처럼 동작하지만 결과를 저장하지 않는다.
- 부호 있는/없는 비교 가능.
- 플래그 변화: O, S, Z, C, A, P
- 결과 해석:
  - ZF=1 → 두 값이 같음
  - CF=1 → unsigned 작음
  - SF≠OF → signed 작음

---

## Flag 요약

| 연산 | ZF | SF | CF | OF |
|------|----|----|----|----|
| AND/TEST | O | O | 0 | 0 |
| OR/XOR   | O | O | 0 | 0 |
| NOT      | X | X | X | X |
| CMP/SUB  | O | O | O | O |

---

## Conditional Jumps (조건 점프)

- CMP 또는 TEST 결과에 따라 Jxx 명령으로 분기.
- JZ, JNZ, JA, JB, JG, JL, JAE, JBE, JLE 등 다양.

예시:
```asm
cmp eax, ebx
ja  Greater
jb  Smaller
je  Equal
```

- JZ / JE: ZF=1  
- JNZ / JNE: ZF=0  
- JG (signed greater): ZF=0, SF=OF  
- JA (unsigned above): CF=0, ZF=0

---

## 조건 루프 (Conditional Loop)

- LOOPZ / LOOPE : ECX≠0 & ZF=1 일 때 계속  
- LOOPNZ / LOOPNE : ECX≠0 & ZF=0 일 때 계속  
- 조건 만족 시 루프 반복, 아니면 종료.

```asm
test [esi], 8000h
pushfd
add esi, TYPE array
popfd
loopnz next
```

---

## 조건 구조 (Conditional Structures)

### C 스타일 while문
```c
while(index < n) {
   if(arr[index] > sample)
       sum += arr[index];
   index++;
}
```

어셈블리 변환 예시:
```asm
cmp esi, ecx
jl  Check
jmp End
Check:
cmp array[esi*4], edx
jg  AddIt
jmp Skip
AddIt: add eax, array[esi*4]
Skip: inc esi
jmp L1
End:
```

---

## Table-Driven Selection (테이블 분기)

- 여러 케이스(멀티 분기)를 테이블로 관리.
- 구조:

| 문자 | 프로시저 주소 |
|------|---------------|
| 'A'  | Process_A     |
| 'B'  | Process_B     |

```asm
cmp al, [ebx]
jne Next
call NEAR PTR [ebx + 1]
add ebx, EntrySize
loop L1
```

---

## 고급 조건 지시문 (.IF, .ELSE)

- 32비트 MASM에서는 .IF, .ELSE, .ELSEIF, .ENDIF 사용 가능.
- 내부적으로 CMP + Jxx로 변환됨.

```asm
.IF eax > val
    mov result, 1
.ENDIF
```

---
