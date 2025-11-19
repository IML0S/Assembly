# Programming Exercises 문제

## ★ 1. Display ASCII Decimal    
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc
DECIMAL_OFFSET = 5
.data
    dec1 BYTE "100123456789765", 0
    dec2 BYTE "12345678", 0
    dec3 BYTE "999999999999", 0
.code
main PROC
    mov edx, OFFSET dec1            
    mov ecx, LENGTHOF dec1 - 1
    mov ebx, DECIMAL_OFFSET
    call WriteScaled

    mov edx, OFFSET dec2            
    mov ecx, LENGTHOF dec2 - 1
    mov ebx, 4
    call WriteScaled

    mov edx, OFFSET dec3            
    mov ecx, LENGTHOF dec3 - 1
    mov ebx, 6
    call WriteScaled

    exit 
main ENDP

WriteScaled PROC
    push eax
    push esi

    mov esi, edx
    mov eax, ecx
    sub eax, ebx
left:
    cmp eax, 0
    je dot

    push eax
    mov al, BYTE PTR[esi]
    call WriteChar
    pop eax
    inc esi
    dec eax
    jmp left
dot:
    mov al, '.'
    call WriteChar
right:
    cmp ebx, 0
    je done
    push eax
    mov al, BYTE PTR[esi]
    call WriteChar
    pop eax
    inc esi
    dec ebx
    jmp right
done:
    call Crlf

    pop esi
    pop eax
    ret
WriteScaled ENDP

END main
```

---
## ★ 2. Extended Subtraction Procedure   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    val1 DWORD 12345678h
    val2 DWORD 11111111h

    val3 DWORD 12345678h, 9ABCDEF0h 
    val4 DWORD 11111111h, 22222222h 

    val5 DWORD 12345678h, 9ABCDEF0h, 00001234h 
    val6 DWORD 11111111h, 22222222h, 00000001h 
.code
main PROC
    mov edx, OFFSET val1
    mov esi, OFFSET val2
    mov ecx, SIZEOF val1
    call Extended_Sub

    mov eax, [edx]
    call WriteHex
    call Crlf

    mov edx, OFFSET val3
    mov esi, OFFSET val4
    mov ecx, SIZEOF val3
    call Extended_Sub

    mov ecx, 2
    mov edx, OFFSET val3
print64:
    mov eax, [edx]
    call WriteHex
    add edx, 4
    loop print64
    call Crlf

    mov edx, OFFSET val5
    mov esi, OFFSET val6
    mov ecx, SIZEOF val5
    call Extended_Sub

    mov ecx, 3
    mov edx, OFFSET val5
print96:
    mov eax, [edx]
    call WriteHex
    add edx, 4
    loop print96
    call Crlf

    exit 
main ENDP

Extended_Sub PROC
    push edx
Subloop:
    cmp ecx, 0
    je done

    mov eax, [edx]
    sbb eax, [esi]
    mov [edx], eax

    add edx, 4
    add esi, 4
    sub ecx, 4
    jmp Subloop
done:
    pop edx
    ret
Extended_Sub ENDP

END main
```

---
## ★★ 3. Packed Decimal Conversion   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    pack1 BYTE 12h, 34h, 56h, 78h
    pack2 BYTE 01h, 23h, 45h, 67h
    pack3 BYTE 18h, 27h, 36h, 45h
    pack4 BYTE 11h, 22h, 33h, 44h
    pack5 BYTE 87h, 65h, 43h, 21h

    buffer BYTE 16 DUP(0)
.code
main PROC
    mov edx, OFFSET pack1
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack2
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack3
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack4
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack5
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    exit 
main ENDP

PackedToAsc PROC
    push eax
    push esi
    push ecx

    mov esi, edx
    mov ecx, 4
    push edi
next_byte:
    mov al, [esi]
    mov ah, al
    shr al, 4
    and ah, 0Fh

    add al, '0'
    add ah, '0'

    mov [edi], al
    inc edi
    mov [edi], ah
    inc edi

    inc esi
    loop next_byte

    mov BYTE PTR[edi], 0
    pop edi
    mov edx, edi
    pop ecx
    pop esi
    pop eax
    ret
PackedToAsc ENDP

END main
```

---
## ★★ 4. Encryption Using Rotate Operations   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    key BYTE -2, 4, 1, 0, -3, 5, 2, -4, -4, 6

    m1 BYTE "HELLO WORLD!!!", 0
    len1 = ($ - m1)
    m2 BYTE "SECOND MESSAGE ENCRYPTION", 0
    len2 = ($ - m2)
.code
main PROC
    mov esi, OFFSET m1
    mov ecx, len1
    call EncryptROT
    mov edx, OFFSET m1
    call WriteString
    call Crlf

    mov esi, OFFSET m2
    mov ecx, len2
    call EncryptROT
    mov edx, OFFSET m2
    call WriteString
    call Crlf

    exit 
main ENDP

EncryptROT PROC
    push ebx
    push edi
    mov edi, 0
EncryptLoop:
    cmp ecx, 0
    je done

    push ecx
    mov al, [esi]
    movsx ebx, key[edi]
    cmp ebx, 0
    je Daum

    cmp ebx, 0
    jl LeftROT
RightROT:
    mov cl, bl
    ror al, cl
    jmp Daum
LeftROT:
    mov cl, bl
    neg cl
    ror al, cl
    jmp Daum
Daum:
    mov [esi], al
    inc esi
    inc edi
    pop ecx
    dec ecx
    
    cmp edi, 10
    jne EncryptLoop
    mov edi, 0
    jmp EncryptLoop
done:
    pop edi
    pop ebx
    ret
EncryptROT ENDP

END main
```

---
## ★★★ 5. Prime Numbers   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    val BYTE 1001 DUP(1)
    space BYTE ' ', 0

.code
main PROC
    mov BYTE PTR val[0], 0
    mov BYTE PTR val[1], 0

    call FindPrime
    call PrintPrimes

    exit 
main ENDP

FindPrime PROC
    mov ebx, 2
NextPRime:
    cmp ebx, 32
    jge done

    mov al, val[ebx]
    cmp al, 1
    jne EndMulti

    mov edi, ebx
Multi:
    add edi, ebx
    cmp edi, 1001
    jge EndMulti
    mov BYTE PTR val[edi], 0
    jmp Multi
EndMulti:
    inc ebx
    jmp NextPrime
done:
    ret
FindPrime ENDP

PrintPrimes PROC
    mov esi, OFFSET val
    mov ecx, 1001
    xor edi, edi
PrintLoop:
    mov al, [esi]
    cmp al, 1
    jne NotPrime

    mov eax, esi
    sub eax, OFFSET val
    call WriteDec
    mov edx, OFFSET space
    call WriteString

    inc edi
    cmp edi, 10 
    jne NotTen
    call Crlf
    xor edi, edi
NotTen:
NotPrime:
    inc esi
    loop PrintLoop
    ret
PrintPrimes ENDP

END main
```

---
## ★★★ 6. Greatest Common Divisor (GCD)   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    val DWORD 48, 18, 81, 153, 270, 96, 42, 6
    pair = LENGTHOF val / 2

    msgOpen BYTE "GCD(", 0
    msgComma BYTE ", ", 0
    msgClose BYTE ") = ", 0
.code
main PROC
    mov esi, OFFSET val
    mov ecx, pair
TestLoop:
    mov edx, OFFSET msgOpen
    call WriteString

    mov eax, [esi]
    call WriteInt

    mov edx, OFFSET msgComma
    call WriteString

    mov eax, [esi+4]
    call WriteInt

    mov edx, OFFSET msgClose
    call WriteString

    mov eax, [esi]
    mov ebx, [esi + 4]
    call GCDP

    call WriteInt
    call Crlf
    add esi, 8
    loop TestLoop

    exit 
main ENDP

GCDP PROC
    cmp eax, 0
    jge AbsXdone
    neg eax
AbsXdone:
    cmp ebx, 0
    jge AbsYdone
    neg ebx
AbsYdone:

    cmp ebx, 0
    jne GCDLoop
    ret
GCDLoop:
    mov edx, 0
    div ebx
    mov eax, ebx
    mov ebx, edx
    cmp ebx, 0
    jne GCDLoop

    ret
GCDP ENDP

END main
```

---
## ★★★ 7. Bitwise Multiplication   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    val DWORD 7, 5, 1234, 56, 1, 4096, 255, 255
    pair = LENGTHOF val / 2

    msgFront BYTE "BitwiseMultiply(", 0
    msgMid BYTE ", ", 0
    msgRear BYTE ") = ", 0
    
.code
main PROC
    mov esi, OFFSET val
    mov ecx, pair
ShowLoop:
    mov edx, OFFSET msgFront
    call WriteString

    mov eax, [esi]
    call WriteDec

    mov edx, OFFSET msgMid
    call WriteString

    mov eax, [esi+4]
    call WriteDec

    mov edx, OFFSET msgRear
    call WriteString

    mov eax, [esi]
    mov ebx, [esi+4]
    call BitwiseMultiply

    call WriteDec
    call Crlf
    add esi, 8
    loop ShowLoop

    exit 
main ENDP

BitwiseMultiply PROC
    xor edx, edx
CheckMul:
    cmp ebx, 0
    je done

    test ebx, 1
    jz SkipAdd
    add edx, eax
SkipAdd:
    shl eax, 1
    shr ebx, 1
    jmp CheckMul
done:
    mov eax, edx
    ret
BitwiseMultiply ENDP

END main
```

---
## ★★★ 8. Add Packed Integers   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    packed4_A  BYTE 78h,56h,34h,12h
    packed4_B  BYTE 21h,43h,65h,87h
    sum4       BYTE 5 DUP(0)

    packed8_A  BYTE 56h,34h,12h,90h,78h,56h,34h,12h
    packed8_B  BYTE 44h,44h,33h,33h,22h,22h,11h,11h
    sum8       BYTE 9 DUP(0)

    packed16_A BYTE 78h,56h,34h,12h,90h,78h,56h,34h,12h,90h,78h,56h,34h,12h,90h,78h
    packed16_B BYTE 11h,22h,33h,44h,55h,66h,77h,88h,99h,00h,11h,22h,33h,44h,55h,66h
    sum16      BYTE 17 DUP(0)

    msgCase4   BYTE "== 4바이트 패킷 10진수 ==",0
    msgCase8   BYTE "== 8바이트 패킷 10진수 ==",0
    msgCase16  BYTE "== 16바이트 패킷 10진수 ==",0
    msgOp1     BYTE "첫 번째: ",0
    msgOp2     BYTE "두 번째: ",0
    msgSum     BYTE "합계: ",0
    msgCarry   BYTE "자리올림 1이 추가로 발생했습니다.",0
.code
main PROC
    call RunCase4
    call RunCase8
    call RunCase16

    exit 
main ENDP

RunCase4 PROC
    mov edx, OFFSET msgCase4             
    call WriteString
    call Crlf

    mov edx, OFFSET msgOp1
    call WriteString
    mov esi, OFFSET packed4_A
    mov ecx, LENGTHOF packed4_A
    call DisplayPacked
    call Crlf

    mov edx, OFFSET msgOp2
    call WriteString
    mov esi, OFFSET packed4_B
    mov ecx, LENGTHOF packed4_B
    call DisplayPacked
    call Crlf

    mov esi, OFFSET packed4_A
    mov edi, OFFSET packed4_B
    mov edx, OFFSET sum4
    mov ecx, LENGTHOF packed4_A
    call AddPacked

    setc al
    mov sum4[LENGTHOF packed4_A], al

    mov edx, OFFSET msgSum
    call WriteString
    mov esi, OFFSET sum4
    mov ecx, LENGTHOF packed4_A
    call DisplayPacked
    call Crlf

    cmp al, 0
    je  NoCarry4
    mov edx, OFFSET msgCarry
    call WriteString
    call Crlf
NoCarry4:
    call Crlf
    ret
RunCase4 ENDP

RunCase8 PROC
    mov edx, OFFSET msgCase8
    call WriteString
    call Crlf

    mov edx, OFFSET msgOp1
    call WriteString
    mov esi, OFFSET packed8_A
    mov ecx, LENGTHOF packed8_A
    call DisplayPacked
    call Crlf

    mov edx, OFFSET msgOp2
    call WriteString
    mov esi, OFFSET packed8_B
    mov ecx, LENGTHOF packed8_B
    call DisplayPacked
    call Crlf

    mov esi, OFFSET packed8_A
    mov edi, OFFSET packed8_B
    mov edx, OFFSET sum8
    mov ecx, LENGTHOF packed8_A
    call AddPacked

    setc al
    mov sum8[LENGTHOF packed8_A], al

    mov edx, OFFSET msgSum
    call WriteString
    mov esi, OFFSET sum8
    mov ecx, LENGTHOF packed8_A
    call DisplayPacked
    call Crlf

    cmp al, 0
    je  NoCarry8
    mov edx, OFFSET msgCarry
    call WriteString
    call Crlf
NoCarry8:
    call Crlf
    ret
RunCase8 ENDP

RunCase16 PROC
    mov edx, OFFSET msgCase16
    call WriteString
    call Crlf

    mov edx, OFFSET msgOp1
    call WriteString
    mov esi, OFFSET packed16_A
    mov ecx, LENGTHOF packed16_A
    call DisplayPacked
    call Crlf

    mov edx, OFFSET msgOp2
    call WriteString
    mov esi, OFFSET packed16_B
    mov ecx, LENGTHOF packed16_B
    call DisplayPacked
    call Crlf

    mov esi, OFFSET packed16_A
    mov edi, OFFSET packed16_B
    mov edx, OFFSET sum16
    mov ecx, LENGTHOF packed16_A
    call AddPacked

    setc al
    mov sum16[LENGTHOF packed16_A], al

    mov edx, OFFSET msgSum
    call WriteString
    mov esi, OFFSET sum16
    mov ecx, LENGTHOF packed16_A
    call DisplayPacked
    call Crlf

    cmp al, 0
    je  NoCarry16
    mov edx, OFFSET msgCarry
    call WriteString
    call Crlf
NoCarry16:
    call Crlf
    ret
RunCase16 ENDP

AddPacked PROC
    clc
AddLoop:
    mov al, BYTE PTR [esi]
    adc al, BYTE PTR [edi]
    daa
    mov BYTE PTR [edx], al
    inc esi
    inc edi
    inc edx
    loop AddLoop
    ret
AddPacked ENDP

DisplayPacked PROC
    pushad

    mov ebx, ecx
    add esi, ecx
    dec esi
    xor edi, edi

NextByte:
    mov al, [esi]
    mov ah, al

    mov dl, ah
    shr dl, 4
    cmp dl, 0
    jne PrintHigh
    cmp edi, 0
    jne PrintHigh
    jmp SkipHigh

PrintHigh:
    cmp dl, 9
    ja  SkipHigh
    mov edi, 1
    mov al, dl
    add al, '0'
    call WriteChar

SkipHigh:
    mov dl, ah
    and dl, 0Fh
    cmp dl, 0
    jne PrintLow
    cmp edi, 0
    jne PrintLow
    jmp SkipLow

PrintLow:
    cmp dl, 9
    ja  SkipLow
    mov edi, 1
    mov al, dl
    add al, '0'
    call WriteChar

SkipLow:
    dec esi
    dec ecx
    jnz NextByte

    cmp edi, 0
    jne DisplayDone
    mov al, '0'
    call WriteChar
DisplayDone:
    popad
    ret
DisplayPacked ENDP

END main
```
