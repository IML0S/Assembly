# Short Answer 문제 풀이 (원문 + 번역 + 풀이)

## 문제 1  
**Q : In the following code sequence, show the value of AL after each shift or rotate instruction has executed:**  
_다음 코드에서 각 shift 또는 rotate 명령 실행 후 AL 값은 무엇인가?_  

```asm
mov al,0D4h
shr al,1             ; a.
mov al,0D4h
sar al,1             ; b.
mov al,0D4h
sar al,4             ; c.
mov al,0D4h
rol al,1             ; d.
```

### 풀이  
기초값 0D4h는 2진수로 11010100b이다.  
- a. `shr al,1` → 01101010b → **6Ah**  
- b. `sar al,1` → 11101010b → **0EAh**  
- c. `sar al,4` → 11111101b → **0FDh**  
- d. `rol al,1` → 10101001b → **0A9h**

---

## 문제 2  
**Q : In the following code sequence, show the value of AL after each shift or rotate instruction has executed:**  
_다음 코드에서 각 shift/rotate 명령 실행 후 AL 값은 무엇인가?_  

```asm
mov al,0D4h
ror al,3             ; a.
mov al,0D4h
rol al,7             ; b.
stc
mov al,0D4h
rcl al,1             ; c.
stc
mov al,0D4h
rcr al,3             ; d.
```

### 풀이  
0D4h = 11010100b, `stc` 명령으로 CF = 1  
- a. `ror al,3` → 10011010b → **9Ah**  
- b. `rol al,7` → 01101010b → **6Ah**  
- c. `rcl al,1` → 10101001b → **0A9h**  
- d. `rcr al,3` → 00111010b → **3Ah**

---

## 문제 3  
**Q : What will be the contents of AX and DX after the following operation?**  
_다음 명령 실행 후 AX와 DX에는 어떤 값이 들어가는가?_  

```asm
mov dx,0
mov ax,222h
mov cx,100h
mul cx
```

### 풀이  
mul은 DX:AX = AX × CX 계산을 수행한다.  
0x0222 × 0x0100 = 0x022200  
→ DX = **0002h**, AX = **2200h**

---

## 문제 4  
**Q : What will be the contents of AX after the following operation?**  
_다음 명령 실행 후 AX 값은?_  

```asm
mov ax,63h
mov bl,10h
div bl
```

### 풀이  
99 ÷ 16  
→ 몫 6 (06h), 나머지 3 (03h)  
→ AX = **0306h**

---

## 문제 5  
**Q : What will be the contents of EAX and EDX after the following operation?**  
_다음 명령 실행 후 EAX와 EDX 값은?_  

```asm
mov eax,123400h
mov edx,0
mov ebx,10h
div ebx
```

### 풀이  
EDX:EAX ÷ EBX  
0x00123400 ÷ 0x10 = 0x0012340  
→ EAX = **00012340h**, EDX = **00000000h**

---

## 문제 6  
**Q : What will be the contents of AX and DX after the following operation?**  
_다음 명령 실행 후 AX와 DX 값은?_  

```asm
mov ax,4000h
mov dx,500h
mov bx,10h
div bx
```

### 풀이  
몫이 AX 범위를 초과하여 저장될 수 없으므로  
→ **오버플로우 발생, 프로그램 종료**

---

## 문제 7  
**Q : What will be the contents of BX after the following instructions execute?**  
_다음 명령 실행 후 BX 레지스터 값은?_  

```asm
mov bx,5
stc
mov ax,60h
adc bx,ax
```

### 풀이  
5 + 96 + CF(1) = 102 = 0x66  
→ BX = **0066h**

---

## 문제 8  
**Q : Describe the output when the following code executes in 64-bit mode:**  
_64비트 모드에서 해당 코드 실행 시 어떤 결과가 발생하는가?_  

```asm
.data
dividend_hi  QWORD 00000108h
dividend_lo  QWORD 33300020h
divisor      QWORD 00000100h
.code
mov  rdx,dividend_hi
mov  rax,dividend_lo
div  divisor
```

### 풀이  
나눗셈 결과의 몫이 RAX에 저장될 수 있는 범위를 초과한다.  
→ **오버플로우 발생, 프로그램 종료**

---

## 문제 9  
**Q : The following program is supposed to subtract val2 from val1. Find and correct all logic errors:**  
_다음 프로그램은 val1에서 val2를 빼려고 한다. 모든 논리적 오류를 찾고 수정하시오._  

```asm
.data
 val1   QWORD 20403004362047A1h
 val2   QWORD 055210304A2630B2h
 result QWORD 0
.code
   mov  cx,8
   mov  esi,val1
   mov  edi,val2
   clc
top:
   mov  al,BYTE PTR[esi]
   sbb  al,BYTE PTR[edi]
   mov  BYTE PTR[esi],al
   dec  esi
   dec  edi
   loop top
```

### 풀이  
오류는 다음과 같다.  
1. `mov esi,val1` 은 값 자체를 로드한다. 주소를 로드하려면 `OFFSET val1` 사용 필요.  
2. QWORD는 리틀 엔디안 구조이다. 가장 낮은 주소가 LSB이다.  
   바이트 단위로 뒤에서 앞으로 가려면 `inc`를 사용해야 한다.  
3. 계산 결과를 val1에 덮어쓰지 말고 따로 `result`에 저장해야 한다.

---

## 문제 10  
**Q : What will be the hexadecimal contents of RAX after the following instructions execute in 64-bit mode?**  
_64비트 모드에서 명령 실행 후 RAX에 들어갈 16진수 값은?_  

```asm
.data
multiplicand QWORD 0001020304050000h
.code
imul rax,multiplicand, 4
```

### 풀이  
4배는 왼쪽 시프트 2회와 동일하다.  
결과 → **0004080C10140000h**


