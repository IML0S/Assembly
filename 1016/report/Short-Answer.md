# Chapter 4 Review Questions and Exercises (Assembly)

---

## 1. 
What will be the value in **EDX** after each of the lines marked (a) and (b) execute?

```asm
.data
one WORD 8002h
two WORD 4321h
.code
mov edx,21348041h
movsx edx,one       ; (a)
movsx edx,two       ; (b)
```

**해설:**  
- `movsx`는 부호 확장 명령
- `one = 8002h` → 최상위 비트가 1이므로 음수.  
  - 8002h = -32766 (16비트 signed)  
  - 부호 확장하면 EDX = FFFF8002h  
- `two = 4321h` → 양수
  - EDX = 00004321h  

**정답:**  
(a) FFFF8002h  
(b) 00004321h  

---

## 2.
What will be the value in **EAX** after the following lines execute?

```asm
mov eax,1002FFFFh
inc ax
```

**해설:**  
- `AX`는 `EAX`의 하위 16비트(`FFFFh`)  
- `inc ax` → `FFFFh + 1 = 0000h`  
- 상위 16비트(`1002h`)는 그대로 유지됨 → EAX = 10020000h  

**정답:** 10020000h  

---

## 3.
What will be the value in **EAX** after the following lines execute?

```asm
mov eax,30020000h
dec ax
```

**해설:**  
- `AX = 0000h`  
- `dec ax` → `0000h - 1 = FFFFh`  
- 상위 16비트(`3002h`) 유지 → EAX = 3002FFFFh  

**정답:** 3002FFFFh  

---

## 4.
What will be the value in **EAX** after the following lines execute?

```asm
mov eax,1002FFFFh
neg ax
```

**해설:**  
- `AX = FFFFh`  
- `neg ax` → 0 - FFFFh = 0001h (Carry=1)  
- 상위 16비트 그대로 → EAX = 10020001h  

**정답:** 10020001h  

---

## 5.
What will be the value of the **Parity flag** after the following lines execute?

```asm
mov al,1
add al,3
```

**해설:**  
- 1 + 3 = 4 → AL = 04h = 00000100b  
- 1비트의 개수 = 1 (홀수) → Parity flag = 0  

**정답:** Parity flag = 0  

---

## 6.
What will be the value of **EAX** and the **Sign flag** after the following lines execute?

```asm
mov eax,5
sub eax,6
```

**해설:**  
- 5 - 6 = -1 → EAX = FFFFFFFFh  
- Sign flag = 1 (음수)  

**정답:**  
EAX = FFFFFFFFh  
SF = 1  

---

## 7.
In the following code, the value in AL is intended to be a signed byte. Explain how the Overflow flag helps, or does not help you, to determine whether the final value in AL falls within a valid signed range.

```asm
mov al,-1
add al,130
```

**해설:**  
- -1 + 130 = 129 → AL = 81h  
- 81h = -127 (signed 8-bit 범위 내)  
- 오버플로우 X → OF = 0  
- 즉, OF는 결과가 signed 범위를 벗어나는지 알려주지만,  
  이번 경우는 범위 내에 있으므로 도움을 주지 않음

**정답:** Overflow flag = 0, 결과는 유효

---

## 8.
What value will **RAX** contain after the following instruction executes?

```asm
mov rax,44445555h
```

**해설:**  
- 32비트 즉시값을 64비트 레지스터에 넣을 때 상위 32비트는 0으로 채워짐

**정답:** RAX = 00000000_44445555h  

---

## 9.
What value will **RAX** contain after the following instructions execute?

```asm
.data
dwordVal DWORD 84326732h
.code
mov rax,0FFFFFFFF00000000h
mov rax,dwordVal
```

**해설:**  
- 두 번째 명령에서 RAX의 하위 32비트가 덮어써지고, 상위 32비트는 0으로 됨

**정답:** RAX = 00000000_84326732h  

---

## 10.
What value will **EAX** contain after the following instructions execute?

```asm
.data
dVal DWORD 12345678h
.code
mov ax,3
mov WORD PTR dVal+2,ax
mov eax,dVal
```

**해설:**  
- `dVal = 12 34 56 78h` (리틀엔디안 저장: 78 56 34 12)  
- `dVal+2`는 상위 2바이트 (34 12 부분)  
- 거기에 0003h를 저장 → 새 값: 78 56 03 00  
- 다시 읽으면 EAX = 00035678h  

**정답:** EAX = 00035678h  

---

## 11.
What will **EAX** contain after the following instructions execute?

```asm
.data
dVal DWORD ?
.code
mov dVal,12345678h
mov ax,WORD PTR dVal+2
add ax,3
mov WORD PTR dVal,ax
mov eax,dVal
```

**해설:**  
- dVal = 12345678h → 메모리: 78 56 34 12  
- `ax ← dVal+2` → 1234h  
- `ax + 3 = 1237h`  
- 저장: 하위워드에 1237h → 메모리: 37 12 34 12  
- 읽기: EAX = 12341237h  

**정답:** EAX = 12341237h  

---

## 12.
Is it possible to set the **Overflow flag** if you add a positive integer to a negative integer?

**해설:**  
- 부호가 다른 두 수를 더하면 오버플로우가 발생하지 않는다.  

**정답:** False

---

## 13.
Will the **Overflow flag** be set if you add a negative integer to a negative integer and produce a positive result?

**해설:**  
- 음수 + 음수 → 양수가 되면 범위를 벗어남 → OF = 1  

**정답:** True

---

## 14.
Is it possible for the **NEG** instruction to set the Overflow flag?

**해설:**  
- 최솟값(예: 80h)을 반전하면 표현 불가능 → OF = 1  

**정답:** True 

---

## 15.
Is it possible for both the **Sign** and **Zero** flags to be set at the same time?

**해설:**  
- Zero = 1은 결과가 0일 때, Sign = 1은 음수일 때  
- 동시에 참일 수 없음.  

**정답:** False  

---

## 16.
For each of the following statements, state whether or not the instruction is valid:

```asm
.data
var1 SBYTE -4,-2,3,1
var2 WORD 1000h,2000h,3000h,4000h
var3 SWORD -16,-42
var4 DWORD 1,2,3,4,5
```

a. `mov ax,var1` → invalid (8-bit → 16-bit direct move 불가)  
b. `mov ax,var2` → valid (16-bit → 16-bit)  
c. `mov eax,var3` → invalid (signed word → doubleword 직접 불가)  
d. `mov var2,var3` → invalid (메모리→메모리 이동 불가)  
e. `movzx ax,var2` → invalid (ax는 16비트, var2도 16비트)  
f. `movzx var2,al` → invalid (destination이 메모리)  
g. `mov ds,ax` → valid  
h. `mov ds,1000h` → invalid (즉시값을 세그먼트 레지스터에 직접 저장 불가)

---

## 17.
What will be the hexadecimal value of the destination operand after each of the following instructions execute in sequence?

```asm
mov al,var1
mov ah,[var1+3]
```

**해설:**  
- var1 = -4, -2, 3, 1 → 0xFC, 0xFE, 0x03, 0x01  
- `al = FC`  
- `ah = [var1+3] = 01` → AX = 01FCh  

**정답:** AX = 01FCh  

---

## 18.
What will be the value of the destination operand after each of the following execute?

```asm
mov ax,var2
mov ax,[var2+4]
mov ax,var3
mov ax,[var3-2]
```

**해설:**  
- var2 = 1000h,2000h,3000h,4000h  
  - (a) `ax = 1000h`  
  - (b) `[var2+4] = 3000h`  
- var3 = -16 (FFF0h), -42 (FFD6h)
  - (c) `ax = FFF0h`  
  - (d) `[var3-2]`는 유효하지 않은 메모리 참조 (잘못된 접근)  

**정답:**  
(a) 1000h  
(b) 3000h  
(c) FFF0h  
(d) invalid  

---

## 19.
What will be the value of the destination operand after each of the following execute?

```asm
mov edx,var4
movzx edx,var2
mov edx,[var4+4]
movsx edx,var1
```

**해설:**  
- var4 = 1,2,3,4,5  
- var2 = 1000h,2000h,3000h,4000h  
- var1 = -4,-2,3,1

(a) `edx = var4 = 00000001h`  
(b) `movzx edx,var2` → var2(첫 WORD=1000h) → 00001000h  
(c) `[var4+4]` = var4[1] = 00000002h  
(d) `movsx edx,var1` → 첫 SBYTE(-4 = FCh) → FFFFFFFCh  

**정답:**  
(a) 00000001h  
(b) 00001000h  
(c) 00000002h  
(d) FFFFFFFCh  
