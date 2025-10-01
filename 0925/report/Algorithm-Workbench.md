# 🛠️ Algorithm Workbench 문제 풀이 (1~15번)

---

## 1. Define four symbolic constants that represent integer 25 in decimal, binary, octal, and hexadecimal formats.  
25를 10진수, 2진수, 8진수, 16진수 형식으로 나타내는 4개의 심볼릭 상수 정의  

```asm
DEC25 = 25  
BIN25 = 11001b  
OCT25 = 31o  
HEX25 = 19h  
```

---

## 2. Find out, by trial and error, if a program can have multiple code and data segments.  
`.data`를 여러 번 선언할 순 없지만 `SEGMENT`와 `ENDS`를 사용해 여러 데이터 세그먼트를 둘 수 있다. 코드 세그먼트 역시 제한적으로 가능하다.  

```asm
data1 SEGMENT
a1 DWORD 1234h
data1 ENDS

data2 SEGMENT
a2 DWORD 5678h
data2 ENDS

code1 SEGMENT
main PROC
    ret
main ENDP
code1 ENDS

code2 SEGMENT
other PROC
    ret
other ENDP
code2 ENDS
```

---

## 3. Create a data definition for a doubleword that stored it in memory in big endian format.  
MASM은 빅 엔디안 저장을 지원하지 않으므로 수동으로 처리해야 한다.  

```asm
.data
big BYTE 12h, 34h, 56h, 78h
```

---

## 4. Find out if you can declare a variable of type DWORD and assign it a negative value.  
DWORD에 음수를 넣으면 2의 보수 형태로 저장된다. 어셈블러는 타입을 엄격히 검사하지 않는다.  

```asm
mov DWORD PTR DS:[402000], -1
```

→ 메모리에 `FF FF FF FF` 저장

---

## 5. Write a program that adds 5 to EAX and EDX, then examine the machine code.  
리스팅 파일 확인 결과, 레지스터에 따라 기계어가 다르게 생성된다.  

```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096

.code
main PROC
    add eax, 5  
    add edx, 5  
    ret
main ENDP
END main
```

리스팅 파일:  

```asm
00000000  83 C0 05    ; add eax, 5  
00000003  83 C2 05    ; add edx, 5
```

---

## 6. Given the number 456789ABh, list its byte values in little-endian order.  
```asm
AB 89 67 45
```

---

## 7. Declare an array of 120 uninitialized unsigned doubleword values.  
```asm
.data
myArray DWORD 120 DUP(?)
```

---

## 8. Declare an array of byte and initialize it to the first 5 letters of the alphabet.  
```asm
.data
array BYTE "ABCDE"
```

---

## 9. Declare a 32-bit signed integer variable with the smallest possible negative value.  
```asm
.data
val2 SDWORD -2147483648
```

---

## 10. Declare an unsigned 16-bit integer variable with three initializers.  
```asm
.data
wArray WORD 1234h, 5678h, 9876h
```

---

## 11. Declare a null-terminated string containing your favorite color.  
```asm
.data
favColor BYTE "CYAN", 0
```

---

## 12. Declare an uninitialized array of 50 signed doublewords.  
```asm
.data
dArray SDWORD 50 DUP(?)
```

---

## 13. Declare a string containing “TEST” repeated 500 times.  
```asm
.data
msg BYTE 500 DUP("TEST"), "$"
```

---

## 14. Declare an array of 20 unsigned bytes initialized to zero.  
```asm
.data
bArray BYTE 20 DUP(0)
```

---

## 15. Show the order of individual bytes in memory for val1 DWORD 87654321h.  
```asm
21h 43h 65h 87h
```
