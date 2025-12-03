# Algorithm Workbench 문제 풀이

## 문제 1

**Q : Show an example of a base-index operand in 32-bit mode.**\
**A :** \[ebx + esi\]\
32비트 모드에서 base-index 피연산자의 예시는 \[ebx + esi\]
형태이다.

------------------------------------------------------------------------

## 문제 2

**Q : Show an example of a base-index-displacement operand in 32-bit
mode.**\
**A :** array\[ebx + esi\]\
32비트 모드에서 base-index-displacement 피연산자는
array\[ebx + esi\]와 같이 사용된다.

------------------------------------------------------------------------

## 문제 3

**Q : Two-dimensional array of doublewords... address third column in
second row.**\
**A :**

    asm
    TYPE pArray = sizeA
    mov esi, 2*4
    mov edi, 3
    mov eax, [esi*sizeA + edi*sizeA]

2차원 배열에서 두 번째 행, 세 번째 열 요소의 주소를 ESI와
EDI로 계산하는 예시이다.

------------------------------------------------------------------------

## 문제 4

**Q : Write instructions using CMPSW that compare two arrays (sourcew,
targetw).**\
**A :**

    asm
    mov ecx, LENGTHOF sourcew
    mov esi, OFFSET sourcew
    mov edi, OFFSET targetw
    cld
    repe CMPSW

CMPSW를 사용해 16비트 배열 두 개를 비교하는 코드이다.

------------------------------------------------------------------------

## 문제 5

**Q : Use SCASW to scan for 0100h in wordArray and copy offset to
EAX.**\
**A :**

    asm
    mov eax, 100h
    mov ecx, LENGTHOF wordArray
    mov edi, OFFSET wordArray
    cld
    repne SCASW
    sub edi, 2
    mov eax, edi

SCASW로 wordArray에서 0100h 값을 찾고 그 위치를 EAX에 저장하는
코드이다.

------------------------------------------------------------------------

## 문제 6

**Q : Use Str_compare to determine larger string and print it.**\
**A :**

    asm
    .data
    str1 BYTE "PINK", 0
    str2 BYTE "GRAY", 0
    .code
        INVOKE Str_compare ADDR str1, ADDR str2
        ja firstBig
        mov edx, OFFSET str2
        jmp printStr
    firstBig:
        mov edx, OFFSET str1
    printStr:
        call WriteString
        call Crlf

Str_compare로 두 문자열 중 더 큰 문자열을 출력하는 코드이다.

------------------------------------------------------------------------

## 문제 7

**Q : Call Str_trim to remove trailing '@' characters.**\
**A :**

    asm
    INVOKE Str_trim, ADDR str1, '@'

문자열 끝의 '@' 문자들을 제거하기 위해 Str_trim을 호출하는
코드이다.

------------------------------------------------------------------------

## 문제 8

**Q : Modify Str_ucase so it changes characters to lowercase.**\
**A :**

    asm
    Str_ucase PROC USES eax esi,
    pString:PTR BYTE
        mov esi, pString
        cmp al, 0
        je L3
        cmp al, 'A'
        jb L2
        cmp al, 'Z'
        ja L2
        or BYTE PTR[esi], 00100000b
    L2:
        inc esi
        jmp L1
    L3:
        ret
    Str_ucase ENDP

모든 문자를 소문자로 변환하도록 수정한 Str_ucase 버전이다.

------------------------------------------------------------------------

## 문제 9

**Q : Create a 64-bit version of Str_trim.**\
**A :**

    asm
    Str_trim PROC USES rax rcx rdi,
        pString:PTR BYTE,
        char:BYTE
        mov rdi, pString
        INVOKE Str_length, rdi
        cmp rax, 0
        je L3
        mov rcx, rax
        dec rax
        add rdi, rax
    L1:
        mov al, [rdi]
        cmp al, char
        jne L2
        dec rdi
        loop L1
    L2: mov BYTE PTR[rdi+1], 0
    L3: ret
    Str_trim ENDP

64비트 형태로 작성된 Str_trim 구현이다.

------------------------------------------------------------------------

## 문제 10

**Q : Show an example of a base-index operand in 64-bit mode.**\
**A :** array\[rsi\*TYPE array\]\
64비트 모드에서 base-index 피연산자의 예시이다.

------------------------------------------------------------------------

## 문제 11

**Q : 32-bit 2D int array addressing using EBX(row) and EDI(col).**\
**A :** mov eax, myArray\[ebx*row_Size*4 + edi\*4\]\
32비트 2차원 배열에서 주어진 요소를 EAX로 가져오는 문장이다.

------------------------------------------------------------------------

## 문제 12

**Q : 64-bit 2D int array addressing using RBX(row) and RDI(col).**\
**A :** mov rax, myArray\[rbx*row_Size*8 + rdi\*8\]\
64비트 2차원 배열에서 주어진 요소를 RAX로 가져오는 문장이다.
