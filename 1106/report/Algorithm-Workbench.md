# Algorithm Workbench

## 문제 1  
**Q : Write a single instruction that converts an ASCII digit in AL to its corresponding binary value. If AL already contains a binary value (00h to 09h), leave it unchanged.**  
AL 레지스터에 저장된 값이 ASCII 숫자라면 해당 이진 값으로 변환하고, 이미 0~9의 값이라면 그대로 둔다.  
ASCII 코드에서 ‘0’은 0x30, ‘9’는 0x39이므로 하위 4비트만 추출하면 실제 숫자 값이 된다.  
**A :**  
```asm
and AL, 0Fh
```

---

## 문제 2  
**Q : Write instructions that calculate the parity of a 32-bit memory operand. Hint: Use the formula presented earlier in this section: B0 XOR B1 XOR B2 XOR B3.**  
32비트 메모리 피연산자의 패리티(짝수/홀수 비트 수)를 계산하는 명령어를 작성하라.  
32비트 값은 4개의 바이트로 구성되며, 각각의 바이트를 XOR하여 결과를 구한다.  
**A :**  
```asm
.data
   Value DWORD ?
.code
   mov al, BYTE PTR Value
   xor al, BYTE PTR Value+1
   xor al, BYTE PTR Value+2
   xor al, BYTE PTR Value+3
```

---

## 문제 3  
**Q : Given two bit-mapped sets named SetX and SetY, write a sequence of instructions that generate a bit string in EAX that represents members in SetX that are not members of SetY.**  
두 개의 비트 집합(SetX, SetY)이 주어졌을 때, SetX에는 포함되지만 SetY에는 없는 원소를 EAX에 저장하는 명령어를 작성하라.  
즉, SetX - SetY 연산을 수행한다.  
**A :**  
```asm
.data
   SetX DWORD ?
   SetY DWORD ?
.code
   mov eax, SetX
   xor eax, SetY
   and eax, SetX
```

---

## 문제 4  
**Q : Write instructions that jump to label L1 when the unsigned integer in DX is less than or equal to the integer in CX.**  
DX가 CX보다 작거나 같은 **부호 없는 정수**일 때 L1으로 점프하는 코드를 작성하라.  
부호 없는 비교에는 `JBE` 명령을 사용한다.  
**A :**  
```asm
cmp dx, cx
jbe L1
```

---

## 문제 5  
**Q : Write instructions that jump to label L2 when the signed integer in AX is greater than the integer in CX.**  
AX가 CX보다 큰 **부호 있는 정수**일 때 L2로 점프하는 코드를 작성하라.  
부호 있는 비교에는 `JG` 명령을 사용한다.  
**A :**  
```asm
cmp ax, cx
jg L2
```

---

## 문제 6  
**Q : Write instructions that first clear bits 0 and 1 in AL. Then, if the destination operand is equal to zero, the code should jump to label L3. Otherwise, it should jump to label L4.**  
AL의 하위 두 비트를 0으로 만든 뒤, 결과가 0이면 L3으로, 아니면 L4로 점프하라.  
비트를 지우는 데는 `AND`를, 0 확인에는 `JZ`를 사용한다.  
**A :**  
```asm
and al, 11111100b
jz L3
jmp L4
```

---

## 문제 7  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that val1 and X are 32-bit variables.**  
다음 의사코드를 어셈블리로 구현하라. `val1`과 `X`는 32비트 변수이다.  
```c
if( val1 > ecx ) AND ( ecx > edx )
   X = 1
else
   X = 2;
```  
두 조건 모두 참일 때만 X에 1을 대입하고, 그렇지 않으면 2를 대입한다.  
**A :**  
```asm
cmp val1, ecx
jbe next
cmp ecx, edx
jbe next
mov X, 1
jmp END
next:
mov X, 2
END:
```

---

## 문제 8  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that X is a 32-bit variable.**  
다음 의사코드를 어셈블리로 구현하라. `X`는 32비트 변수이다.  
```c
if( ebx > ecx ) OR ( ebx > val1 )
   X = 1
else
   X = 2
```  
두 조건 중 하나라도 참이면 X에 1을, 모두 거짓이면 2를 대입한다.  
**A :**  
```asm
cmp ebx, ecx
ja set_one
cmp ebx, val1
jbe set_two
set_one:
mov X, 1
jmp END
set_two:
mov X, 2
END:
```

---

## 문제 9  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that X is a 32-bit variable.**  
다음 의사코드를 어셈블리로 구현하라. `X`는 32비트 변수이다.  
```c
if( (ebx > ecx AND ebx > edx) OR (edx > eax) )
   X = 1
else
   X = 2
```  
두 조건 중 하나라도 참이면 X에 1을, 아니면 2를 대입한다.  
**A :**  
```asm
cmp ebx, ecx
jbe check2
cmp ebx, edx
ja set_one
check2:
cmp edx, eax
jbe set_two
set_one:
mov X, 1
jmp END
set_two:
mov X, 2
END:
```

---

## 문제 10  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that A, B, and N are 32-bit signed integers.**  
다음 의사코드를 어셈블리로 구현하라. A, B, N은 32비트 부호 있는 정수이다.  
```c
while N > 0
  if N != 3 AND (N < A OR N > B)
     N = N – 2
  else
     N = N – 1
end while
```  
N이 0보다 큰 동안 반복하며, 조건에 따라 1 또는 2를 빼는 코드이다.  
**A :**  
```asm
begin:
   cmp N, 0
   jng endwhile
   cmp N, 3
   je else_part
   cmp N, A
   jl sub_two
   cmp N, B
   jg sub_two
else_part:
   sub N, 1
   jmp begin
sub_two:
   sub N, 2
   jmp begin
endwhile:
```
