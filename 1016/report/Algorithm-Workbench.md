## 1. Exchange upper and lower words in a doubleword variable named `three`
```asm
mov ax, WORD PTR three
mov bx, WORD PTR three+2
mov WORD PTR three, bx
mov WORD PTR three+2, ax
```
두 16비트 영역을 교환하여 상하위 워드를 바꾼다.

---

## 2. Reorder A,B,C,D → B,C,D,A using XCHG (no more than 3 times)
```asm
xchg al, bl
xchg bl, cl
xchg cl, dl
```
세 번의 XCHG로 레지스터를 한 칸씩 순환시킨다.

---

## 3. Use Parity flag with arithmetic instruction to check even/odd parity
```asm
add al, 0
jpo OddParity
; even parity otherwise
```
`add al,0`은 AL의 값은 그대로 두고 Parity Flag를 설정한다. `jpo`는 홀수 패리티일 때 분기한다.

---

## 4. Add two negative integers and set Overflow flag
```asm
mov al, -100
add al, -60
```
음수끼리 더하면 부호 비트가 넘쳐서 OF=1이 된다.

---

## 5. Set Zero and Carry flags simultaneously using addition
```asm
mov al, 0FFh
add al, 1
```
0xFF + 1 → AL=0, CF=1, ZF=1.

---

## 6. Set Carry flag using subtraction
```asm
mov al, 0
sub al, 1
```
0 - 1 = 0xFF로 빌림 발생 → CF=1.

---

## 7. Implement EAX = –val2 + 7 – val3 + val1
```asm
mov eax, val1
sub eax, val3
add eax, 7
sub eax, val2
```
순서대로 계산하면 식과 동일한 결과를 얻는다.

---

## 8. Sum elements of a DWORD array with scaled indexing
```asm
mov ecx, LENGTHOF array
xor eax, eax
xor ebx, ebx
L1:
add eax, [array + ebx*4]
inc ebx
loop L1
```
`ebx*4`를 사용해 32비트 단위로 배열 접근.

---

## 9. Implement AX = (val2 + BX) – val4
```asm
mov ax, val2
add ax, bx
sub ax, val4
```
단순한 덧셈 후 뺄셈 구현.

---

## 10. Set both Carry and Overflow flags simultaneously
```asm
mov al, 80h
add al, 80h
```
128 + 128 = 256 → AL=00h, CF=1, OF=1.

---

## 11. Use Zero flag to indicate unsigned overflow after INC and DEC
```asm
mov al, 0FFh
inc al   ; AL=00h, ZF=1 (overflow)
dec al   ; AL=FFh, ZF=0
```
ZF=1이면 8비트 증가 시 오버플로우 발생.

---

## 12. Align `myBytes` to an even address
```asm
ALIGN 2
myBytes BYTE 10h,20h,30h,40h
```
`ALIGN 2`는 짝수 주소 정렬을 보장한다.

---

## 13. Determine EAX value after each instruction
```asm
.data
myBytes BYTE 10h,20h,30h,40h
myWords WORD 3 DUP(?),2000h
myString BYTE "ABCDE"
```
| Instruction | Result (EAX) | Explanation |
|--------------|--------------|--------------|
| mov eax, TYPE myBytes | 1 | BYTE = 1 byte |
| mov eax, LENGTHOF myBytes | 4 | 4 elements |
| mov eax, SIZEOF myBytes | 4 | 4 bytes total |
| mov eax, TYPE myWords | 2 | WORD = 2 bytes |
| mov eax, LENGTHOF myWords | 4 | 3 DUP + 1 literal |
| mov eax, SIZEOF myWords | 8 | 4×2 = 8 bytes |
| mov eax, SIZEOF myString | 5 | "ABCDE" = 5 bytes |

---

## 14. Move the first two bytes in myBytes to DX (result 2010h)
```asm
mov dx, WORD PTR myBytes
```
메모리의 두 바이트(10h,20h)를 DX로 옮긴다 → DX=2010h.

---

## 15. Move the second byte in myWords to AL
```asm
mov al, BYTE PTR myWords+1
```
두 번째 바이트(상위 바이트)를 AL로 이동.

---

## 16. Move all four bytes in myBytes to EAX
```asm
mov eax, DWORD PTR myBytes
```
4바이트(10h,20h,30h,40h)를 한 번에 EAX로 옮긴다.

---

## 17. Insert LABEL for moving myWords to 32-bit register
```asm
myWords WORD 3 DUP(?),2000h
myWordsD LABEL DWORD
```
`LABEL DWORD`를 사용해 32비트 접근 가능.

---

## 18. Insert LABEL for moving myBytes to 16-bit register
```asm
myBytes BYTE 10h,20h,30h,40h
myBytesW LABEL WORD
```
`LABEL WORD`로 16비트 단위 접근 가능.
