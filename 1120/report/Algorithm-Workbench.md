# Algorithm Workbench 문제 풀이

---

## 문제 1  
**Q : Here is a calling sequence for a procedure named AddThree that adds three doublewords (assume that the STDCALL calling convention is used):**  
→ 세 개의 더블워드 값을 더하는 AddThree 프로시저의 호출 순서이다(STDCALL 규약 사용 가정).

```asm
push 10h
push 20h
push 30h
call AddThree
```

**Draw a picture of the procedure’s stack frame immediately after EBP has been pushed on the runtime stack.**  
→ EBP가 런타임 스택에 push된 직후의 스택 프레임을 그려라.

**A :**

| 스택 주소 기준 | 내용        | 설명            |
|----------------|-------------|----------------|
| EBP + 16       | 10h         | 세 번째 인자   |
| EBP + 12       | 20h         | 두 번째 인자   |
| EBP + 8        | 30h         | 첫 번째 인자   |
| EBP + 4        | Return Addr | 리턴 주소      |
| EBP, ESP       | EBP, ESP    | 스택 프레임 포인터 |

---

## 문제 2  
**Q : Create a procedure named AddThree that receives three integer parameters and calculates and returns their sum in the EAX register.**  
→ 세 개의 정수 매개변수를 받아 합을 계산하여 EAX 레지스터에 반환하는 AddThree 프로시저를 작성하라.

**A :**

```asm
AddThree PROC
  enter 0, 0
  mov eax, [ebp + 8]
  add eax, [ebp + 12]
  add eax, [ebp + 16]
  leave
  ret 12
AddThree ENDP
```

---

## 문제 3  
**Q : Declare a local variable named pArray that is a pointer to an array of doublewords.**  
→ doubleword 배열을 가리키는 포인터 pArray라는 지역 변수를 선언하라.

**A :**

```asm
LOCAL pArray:PTR DWORD
```

---

## 문제 4  
**Q : Declare a local variable named buffer that is an array of 20 bytes.**  
→ 20바이트 크기의 배열 buffer라는 지역 변수를 선언하라.

**A :**

```asm
LOCAL buffer[20]:BYTE
```

---

## 문제 5  
**Q : Declare a local variable named pwArray that points to a 16-bit unsigned integer.**  
→ 16비트 부호 없는 정수를 가리키는 포인터 pwArray 지역 변수를 선언하라.

**A :**

```asm
LOCAL pwArray:PTR WORD
```

---

## 문제 6  
**Q : Declare a local variable named myByte that holds an 8-bit signed integer.**  
→ 8비트 부호 있는 정수 myByte를 지역 변수로 선언하라.

**A :**

```asm
LOCAL myByte:SBYTE
```

---

## 문제 7  
**Q : Declare a local variable named myArray that is an array of 20 doublewords.**  
→ 20개의 doubleword로 이루어진 배열 myArray 지역 변수를 선언하라.

**A :**

```asm
LOCAL myArray[20]:DWORD
```

---

## 문제 8  
**Q : Create a procedure named SetColor that receives two stack parameters: forecolor and backcolor, and calls the SetTextColor procedure from the Irvine32 library.**  
→ forecolor와 backcolor를 받고 SetTextColor를 호출하는 SetColor 프로시저 작성하라.

**A :**

```asm
SetColor PROC forecolor:BYTE, backcolor:BYTE
  INVOKE SetTextColor, forecolor, backcolor
  ret
SetColor ENDP
```

---

## 문제 9  
**Q : Create a procedure named WriteColorChar that receives three stack parameters: char, forecolor, and backcolor. It displays a single character, using the color attributes specified in forecolor and backcolor.**  
→ char, forecolor, backcolor를 받아 색상을 적용하여 문자 1개 출력하는 프로시저 작성.

**A :**

```asm
WriteColorChar PROC
  enter 0, 0
  mov al, [ebp+12]  ;
  mov ah, [ebp+8]   ;
  call SetTextColor

  mov al, [ebp+16]  ;
  call WriteChar
  leave
  ret
WriteColorChar ENDP
```

---

## 문제 10  
**Q : Write a procedure named DumpMemory that encapsulates the DumpMem procedure in the Irvine32 library. Use declared parameters and the USES directive.**  
→ Irvine32의 DumpMem을 캡슐화하는 DumpMemory 작성 (USES 사용).

**A :**

```asm
DumpMemory PROC USES eax edx, ptrMem:PTR BYTE, memLen:DWORD, memType:DWORD
  INVOKE DumpMem, ptrMem, memLen, memType
  ret
DumpMemory ENDP
```

---

## 문제 11  
**Q : Declare a procedure named MultArray that receives two pointers to arrays of doublewords, and a third parameter indicating the number of array elements. Also, create a PROTO declaration for this procedure.**  
→ 두 배열 포인터와 개수를 받는 MultArray 프로시저 + PROTO 선언 작성.

**A :**

```asm
MultArray PROTO ptrArray1:PTR DWORD, ptrArray2:PTR DWORD, count:DWORD
```

```asm
MultArray PROC ptrArray1:PTR DWORD, ptrArray2:PTR DWORD, count:DWORD
  ; ...
  ret
MultArray ENDP
```

