# Short Answer 문제 풀이  

## 문제 1  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,0FFFFh
and  bx,6Bh
```
0FFFFh AND 006Bh → 하위 비트만 남아 006Bh이 된다.

**A :** BX = 006Bh  

---
## 문제 2  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,91BAh
and  bx,92h
```
91BAh AND 0092h → 하위 8비트만 남아 0092h이 된다.

**A :** BX = 0092h  

---
## 문제 3  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,0649Bh
or   bx,3Ah
```
0649Bh OR 003Ah → 하위 비트가 합쳐져 64BBh이 된다.

**A :** BX = 64BBh  

---
## 문제 4  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,029D6h
xor  bx,8181h
```
029D6h XOR 8181h → 서로 다른 비트가 1로 설정되어 A857h이 된다.

**A :** BX = A857h  

---
## 문제 5  
**Q : What will be the value of EBX after the following instructions execute?**  
```asm
mov  ebx,0AFAF649Bh
or   ebx,3A219604h
```
OR 연산으로 두 피연산자의 1비트가 합쳐져 0BFAFF69Fh가 된다.

**A :** EBX = 0BFAFF69Fh  

---
## 문제 6  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0AFAF649Bh
xor  rbx,0FFFFFFFFh
```
XOR 0FFFFFFFFh는 모든 비트를 반전시켜 0000000050509B64h가 된다.

**A :** RBX = 0000000050509B64h  

---
## 문제 7  
**Q : In the following instruction sequence, show the resulting value of AL where indicated, in binary:**  
```asm
mov  al,01101111b
and  al,00101101b        ; a.
mov  al,6Dh
and  al,4Ah              ; b.
mov  al,00001111b
or   al,61h              ; c.
mov  al,94h
xor  al,37h              ; d.
```
각 단계의 비트 연산 결과는 다음과 같다.

**A :**  
a. 00101101b  
b. 01001000b  
c. 01101111b  
d. 10100011b  

---
## 문제 8  
**Q : In the following instruction sequence, show the resulting value of AL where indicated, in hexadecimal:**  
```asm
mov  al,7Ah
not  al               ; a.
mov  al,3Dh
and  al,74h           ; b.
mov  al,9Bh
or   al,35h           ; c.
mov  al,72h
xor  al,0DCh          ; d.
```
각 단계의 논리 연산 결과를 16진수로 표시하면 다음과 같다.

**A :**  
a. 85h  
b. 34h  
c. 0BFh  
d. 0AEh  

---
## 문제 9  
**Q : In the following instruction sequence, show the values of the Carry, Zero, and Sign flags where indicated:**  
```asm
mov  al,00001111b
test al,00000010b       ; a.
mov  al,00000110b
cmp  al,00000101b       ; b.
mov  al,00000101b
cmp  al,00000111b       ; c.
```
각 명령의 플래그 상태를 분석하면 다음과 같다.

**A :**  
a. CF = 0, ZF = 0, SF = 0  
b. CF = 0, ZF = 0, SF = 0  
c. CF = 1, ZF = 0, SF = 1  

---
## 문제 10  
**Q : Which conditional jump instruction executes a branch based on the contents of ECX?**  
ECX가 0일 때 점프하는 명령은 JECXZ이다.

**A :** JECXZ  

---
## 문제 11  
**Q : How are JA and JNBE affected by the Zero and Carry flags?**  
JA와 JNBE는 동일한 조건으로, ZF=0이고 CF=0일 때 점프한다.

**A :** Zero=0, Carry=0일 때 점프  

---
## 문제 12  
**Q : What will be the final value in EDX after this code executes?**  
```asm
mov  edx,1
mov  eax,7FFFh
cmp  eax,8000h
jl   L1
mov  edx,0
L1:
```
JL은 부호 있는 비교를 수행한다. 7FFFh < 8000h이므로 점프 조건이 참이다.

**A :** EDX = 1  

---
## 문제 13  
**Q : What will be the final value in EDX after this code executes?**  
```asm
mov  edx,1
mov  eax,7FFFh
cmp  eax,8000h
jb   L1
mov  edx,0
L1:
```
JB는 부호 없는 비교를 수행한다. 7FFFh < 8000h이므로 점프한다.

**A :** EDX = 1  

---
## 문제 14  
**Q : What will be the final value in EDX after this code executes?**  
```asm
mov  edx,1
mov  eax,7FFFh
cmp  eax,0FFFF8000h
jl   L2
mov  edx,0
L2:
```
7FFFh > FFFF8000h이므로 JL 조건이 거짓이다. 점프하지 않는다.

**A :** EDX = 0  

---
## 문제 15  
**Q : (True/False): The following code will jump to the label named Target.**  
```asm
mov  eax,-30
cmp  eax,-50
jg   Target
```
-30 > -50이므로 점프 조건이 참이다.

**A :** True  

---
## 문제 16  
**Q : (True/False): The following code will jump to the label named Target.**  
```asm
mov  eax,-42
cmp  eax,26
ja   Target
```
JA는 부호 없는 비교이므로, -42는 0xFFD6로 간주되어 26보다 크다.

**A :** True  

---
## 문제 17  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,80h
```
FFFF...h AND 80h → 상위 비트를 제외하고 0FFFFFFFFFFFFFF80h가 된다.

**A :** RBX = 0FFFFFFFFFFFFFF80h  

---
## 문제 18  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,808080h
```
FFFF...h AND 808080h → 결과는 0FFFFFFFFFF808080h가 된다.

**A :** RBX = 0FFFFFFFFFF808080h  

---
## 문제 19  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,80808080h
```
FFFF...h AND 80808080h → 결과는 0FFFFFFFF80808080h가 된다.

**A :** RBX = 0FFFFFFFF80808080h  

