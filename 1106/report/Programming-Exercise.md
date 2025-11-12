# 🧩 Programming Exercises 문제 풀이 (Conditional Processing 범위, 주석 보강 버전)

---

## ★ 문제 1. Filling an Array  
**A :**
```asm
FillArray PROC USES eax ecx edx esi, pArray:PTR DWORD, N:DWORD, j:DWORD, k:DWORD
    mov esi, pArray         ; 배열 시작 주소
    mov ecx, N              ; 반복 횟수

NextVal:
    ; 난수 생성
    mov eax, k
    sub eax, j
    inc eax
    call RandomRange
    add eax, j              ; j~k 범위 조정
    mov [esi], eax          ; 배열에 값 저장
    add esi, 4
    loop NextVal
    ret
FillArray ENDP
```

---

## ★★ 문제 2. Summing Array Elements in a Range  
**A :**
```asm
SumInRange PROC USES ecx edx esi, pArray:PTR SDWORD, count:DWORD, j:SDWORD, k:SDWORD
    mov esi, pArray
    mov ecx, count
    xor eax, eax

NextVal:
    mov edx, [esi]
    cmp edx, j              ; 하한보다 작은가?
    jl SkipVal
    cmp edx, k              ; 상한보다 큰가?
    jg SkipVal
    add eax, edx            ; 범위 내면 합산
SkipVal:
    add esi, 4
    loop NextVal
    ret
SumInRange ENDP
```

---

## ★★ 문제 3. Test Score Evaluation  
**A :**
```asm
CalcGrade PROC USES eax
    ; 점수 비교 후 등급 반환
    cmp eax, 90
    jae GradeA
    cmp eax, 80
    jae GradeB
    cmp eax, 70
    jae GradeC
    cmp eax, 60
    jae GradeD
    mov al, 'F'
    ret

GradeA: mov al, 'A'  ; 90~100
    ret
GradeB: mov al, 'B'  ; 80~89
    ret
GradeC: mov al, 'C'  ; 70~79
    ret
GradeD: mov al, 'D'  ; 60~69
    ret
CalcGrade ENDP
```

---

## ★★ 문제 4. College Registration  
**A :**
```asm
INVOKE ReadInt       ; 평균 입력
mov ebx, eax
INVOKE ReadInt       ; 학점 입력

cmp eax, 1
jl InvalidInput
cmp eax, 30
jg InvalidInput

; 등록 조건 판단
cmp ebx, 60
jl NoRegister
cmp eax, 15
jl NoRegister
jmp CanRegister

InvalidInput:
    mWrite "Error: Credits must be 1–30",0dh,0ah
    jmp ExitProg
NoRegister:
    mWrite "The student cannot register",0dh,0ah
    jmp ExitProg
CanRegister:
    mWrite "The student can register",0dh,0ah
ExitProg:
```

---

## ★★★ 문제 5. Boolean Calculator (1)  
**A :**
```asm
.data
MenuTable DWORD AND_op, OR_op, NOT_op, XOR_op, ExitProg
MenuText BYTE "1.AND  2.OR  3.NOT  4.XOR  5.Exit",0dh,0ah,0

.code
Main PROC
MenuLoop:
    mWrite MenuText          ; 메뉴 표시
    call ReadInt             ; 사용자 선택
    cmp eax, 5
    ja ExitProg
    dec eax
    jmp [MenuTable + eax*4]  ; 선택된 연산 실행
ExitProg:
    ret
Main ENDP
```

---

## ★★★ 문제 6. Boolean Calculator (2)  
**A :**
```asm
AND_op PROC
    ; AND 연산 수행
    call ReadHex
    mov ebx, eax
    call ReadHex
    and eax, ebx
    call WriteHex
    call Crlf
    ret
AND_op ENDP

OR_op PROC
    ; OR 연산 수행
    call ReadHex
    mov ebx, eax
    call ReadHex
    or eax, ebx
    call WriteHex
    call Crlf
    ret
OR_op ENDP

NOT_op PROC
    ; NOT 연산 수행
    call ReadHex
    not eax
    call WriteHex
    call Crlf
    ret
NOT_op ENDP

XOR_op PROC
    ; XOR 연산 수행
    call ReadHex
    mov ebx, eax
    call ReadHex
    xor eax, ebx
    call WriteHex
    call Crlf
    ret
XOR_op ENDP
```

---

## ★★ 문제 7. Probabilities and Colors  
**A :**
```asm
mov ecx, 20
NextLine:
    mov eax, 10
    call RandomRange         ; 0~9 난수 생성

    cmp eax, 3
    jl WhiteText             ; 0~2: 흰색
    cmp eax, 4
    je BlueText              ; 3: 파랑
    jmp GreenText            ; 나머지: 초록

WhiteText: call SetTextColorWhite
    jmp Print
BlueText:  call SetTextColorBlue
    jmp Print
GreenText: call SetTextColorGreen
Print:
    mWrite "Assembly is fun!",0dh,0ah
    loop NextLine
```

---

## ★★★ 문제 8. Message Encryption  
**A :**
```asm
Encrypt PROC USES esi edi ecx ebx, pMsg:PTR BYTE, msgLen:DWORD, pKey:PTR BYTE, keyLen:DWORD
    mov esi, pMsg
    mov edi, pKey
    mov ecx, msgLen
    xor ebx, ebx

NextChar:
    ; 문자 단위 XOR 암호화
    mov al, [esi]
    xor al, [edi + ebx]
    mov [esi], al

    inc esi
    inc ebx
    cmp ebx, keyLen
    jb Continue
    xor ebx, ebx              ; 키 인덱스 리셋
Continue:
    loop NextChar
    ret
Encrypt ENDP
```

---

## ★★ 문제 9. Validating a PIN  
**A :**
```asm
Validate_PIN PROC USES esi ecx edx, pPIN:PTR BYTE
    mov esi, pPIN
    mov ecx, 5
    xor eax, eax

NextDigit:
    ; 각 자릿수 범위 검사
    mov dl, [esi]
    cmp dl, MinRange[eax]
    jb Invalid
    cmp dl, MaxRange[eax]
    ja Invalid

    inc esi
    inc eax
    loop NextDigit

    xor eax, eax              ; 유효 PIN
    ret
Invalid:
    inc eax                   ; 오류 반환
    ret
Validate_PIN ENDP
```

---

## ★★★★ 문제 10. Parity Checking  
**A :**
```asm
ParityCheck PROC USES ecx esi, pArr:PTR BYTE, len:DWORD
    mov esi, pArr
    mov ecx, len
    xor eax, eax

NextByte:
    xor al, [esi]             ; 모든 바이트 XOR
    inc esi
    loop NextByte

    test al, 1
    jz Even
    xor eax, eax              ; 홀수 패리티
    ret
Even:
    mov eax, 1                ; 짝수 패리티
    ret
ParityCheck ENDP
```
