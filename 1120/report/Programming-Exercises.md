# Programming Exercises 문제 풀이
## ★ 1. FindLargest Procedure  
**Q : Create a procedure named FindLargest that receives two parameters: a pointer to a signed doubleword array, and a count of the array’s length. The procedure must return the value of the largest array member in EAX. Use the PROC directive with a parameter list when declaring the procedure. Preserve all registers (except EAX) that are modified by the procedure. Write a test program that calls FindLargest and passes three different arrays of different lengths. Be sure to include negative values in your arrays. Create a PROTO declaration for FindLargest.**  
**A :** 배열과 그 길이를 파라미터로 받아 배열의 최댓값을 반환하는 프로시저 작성, PROTO로 선언 및 해당 프로시저 3회 실행  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

FindLargest PROTO, 
    ptrArray:PTR SDWORD,
    arrayCount:DWORD

.data
    arr1 SDWORD 23, -45, 67, -12, 89
    arr1Size DWORD 5

    arr2 SDWORD -100, -50, 0, 50, 100, -200, 150
    arr2Size DWORD 7

    arr3 SDWORD -10, -2, -5
    arr3Size DWORD 3

    msg1 BYTE "Array 1 - Largest value: ", 0
    msg2 BYTE "Array 2 - Largest value: ", 0
    msg3 BYTE "Array 3 - Largest value: ", 0
.code
main PROC
    call Clrscr

    mov edx, OFFSET msg1
    call WriteString
    INVOKE FindLargest, OFFSET arr1, arr1Size
    call WriteInt
    call Crlf

    mov edx, OFFSET msg2
    call WriteString
    INVOKE FindLargest, OFFSET arr2, arr2Size
    call WriteInt
    call Crlf

    mov edx, OFFSET msg3
    call WriteString
    INVOKE FindLargest, OFFSET arr3, arr3Size
    call WriteInt
    call Crlf
    exit 
main ENDP

;----------------------------------------------------------
FindLargest PROC USES esi ecx edx,
    ptrArray:PTR SDWORD,
    arrayCount:DWORD
;
;  배열에서 가장 큰 값을 찾아 EAX로 반환하는 프로시저
;  
;  입력 파라미터:
;    ptrArray   = 배열 시작 주소
;    arrayCount = 배열의 요소 개수
;
;  반환값:
;    EAX = 배열의 최댓값
;
;  수정되는 레지스터:
;    EAX만 변경됨, 나머지는 USES로 보존됨
;----------------------------------------------------------

    mov esi, ptrArray        ; ESI ← 배열 주소
    mov ecx, arrayCount      ; ECX ← 배열 길이

    cmp ecx, 0               ; 배열 길이가 0이면
    jle L2                   ; 바로 종료(EAX는 의미 없음)

    mov eax, [esi]           ; 첫 번째 요소를 초기 최댓값으로 설정
    dec ecx                  ; 남은 요소 개수 감소
    jz L2                    ; 길이가 1이었다면 바로 반환

    add esi, 4               ; 다음 요소 위치로 이동

L1:
    mov edx, [esi]           ; EDX ← 현재 요소 값
    cmp edx, eax             ; 현재 요소와 최댓값 비교
    jle L3                   ; 현재 요소 ≤ 최댓값이면 갱신 X
    mov eax, edx             ; 현재 요소가 더 크면 최댓값 갱신

L3:
    add esi, 4               ; 다음 요소로 이동
    loop L1                  ; ECX-- 하면서 반복

L2:
    ret 8                    ; 스택에서 두 인자(8바이트) 정리 후 복귀

FindLargest ENDP
END main
```

---
## ★ 2. Chess Board  
**Q : Write a program that draws an 8×8 chess board, with alternating gray and white squares. You can use the SetTextColor and Gotoxy procedures from the Irvine32 library. Avoid the use of global variables, and use declared parameters in all procedures. Use short procedures that are focused on a single task.**  
**A :** 8x8 체스판을 출력한다.  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

;-----------------------------------------
; 체스보드 한 칸에 출력할 문자열 (▒▒)
; 화이트/그레이 색상값
;-----------------------------------------
.data
    space       BYTE "▒▒", 0 
    whiteColor  DWORD 15
    grayColor   DWORD 8

.code
main PROC
    call Clrscr
    INVOKE DrawChessBoard
    call Crlf
    exit 
main ENDP

;=====================================================
; DrawChessBoard
; 8x8 체스판을 출력하는 루틴
; 각 칸의 색은 (row + col) % 2 로 계산하여 호출
;=====================================================
DrawChessBoard PROC
    pushad
    mov esi, 0        ; row = 0
    mov ecx, 8        ; 총 8행
rowLoop:
        mov ebx, 0    ; col = 0
        mov edi, 8    ; 총 8열
colLoop:
        INVOKE GetColor, esi, ebx   ; EAX ← 칸 색상

        ; x = col*2, y = row
        mov edx, ebx
        shl edx, 1                  ; 한 칸 = 2문자

        INVOKE DrawSquare, edx, esi, eax

        inc ebx
        dec edi
        jnz colLoop

        inc esi        ; row++
        loop rowLoop   ; ecx--

    popad
    ret
DrawChessBoard ENDP

;=====================================================
; DrawSquare
; 지정한 (x,y) 좌표에 특정 색상으로 '▒▒' 출력
; xpos = 출력 X좌표 (문자 단위)
; ypos = 출력 Y좌표
; color = 문자 색상
;=====================================================
DrawSquare PROC USES eax edx esi,
    xpos:DWORD,
    ypos:DWORD,
    color:DWORD

    mov eax, color
    call SetTextColor      ; 색상 설정

    mov dl, BYTE PTR xpos
    mov dh, BYTE PTR ypos
    call Gotoxy            ; 커서 이동

    mov edx, OFFSET space
    call WriteString       ; "▒▒" 출력

    ret
DrawSquare ENDP

;=====================================================
; GetColor
; (row + col) % 2 == 0 → 흰색
; (row + col) % 2 == 1 → 회색
;=====================================================
GetColor PROC USES edx,
    row:DWORD,
    col:DWORD

    mov eax, row
    add eax, col       ; row + col
    and eax, 1         ; % 2

    cmp eax, 0
    jne oddSquare

    mov eax, whiteColor ; 짝수 → 화이트
    ret
oddSquare:
    mov eax, grayColor  ; 홀수 → 그레이
    ret
GetColor ENDP

END main

```

---
## ★★★ 3. Chess Board with Alternating Colors  
**Q : This exercise extends Exercise 2. Every 500 milliseconds, change the color of the colored squares and redisplay the board. Continue until you have shown the board 16 times, using all possible 4-bit background colors. (The white squares remain white throughout.)**  
**A :** 2번의 확장, 흰색 칸은 냅두고 회색 칸을 500ms마다 색 변경하며 출력하는 프로시저. 아무튼 됨  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

DrawChessBoard PROTO
DrawSquare PROTO, xpos:DWORD, ypos:DWORD, color:DWORD
GetColor PROTO, row:DWORD, col:DWORD

.data
    space BYTE "▒▒", 0                 ; 한 칸을 표현하는 2문자
    whiteColor DWORD 15                 ; 기본(짝수 칸) 색
    ChangeColor DWORD 0                 ; 홀수 칸 색을 계속 변화시키는 변수
.code

;============================================================
; main
; 체스판을 반복적으로 다시 그리며 ChangeColor 값을 증가시켜
; "홀수 칸의 색이 계속 바뀌는 애니메이션"을 만든다.
;============================================================
main PROC
    call Clrscr
    mov ecx, 16                         ; 16번 반복 (16 프레임)

colorLoop:
    call Clrscr                         ; 화면 지우기
    INVOKE DrawChessBoard               ; 새 체스판 그리기
    INVOKE Sleep, 500                   ; 0.5초 대기 (애니메이션 효과)
    inc ChangeColor                     ; 홀수 칸 색을 변화시키는 핵심 부분
    loop colorLoop

    exit 
main ENDP


;============================================================
; DrawChessBoard
; 8x8 체스판을 출력한다.
; 각 칸은 GetColor(row,col)로 색상을 결정하고 DrawSquare로 출력.
;============================================================
DrawChessBoard PROC
    pushad
    mov esi, 0                           ; row = 0
    mov ecx, 8                           ; 총 8행

rowLoop:
    push ecx                              ; 열 루프를 위해 행 카운트 백업
    mov ecx, 8                            ; 총 8열
    mov edi, 0                            ; col = 0

colLoop:
    INVOKE GetColor, esi, edi             ; EAX ← 이 칸의 색상

    mov edx, edi
    shl edx, 1                            ; x = col * 2 (문자 폭 2칸)
    INVOKE DrawSquare, edx, esi, eax

    inc edi
    dec ecx
    jnz colLoop

    inc esi                               ; 다음 행
    pop ecx
    loop rowLoop

    popad
    ret
DrawChessBoard ENDP


;============================================================
; DrawSquare
; 하나의 칸(▒▒)을 특정 좌표·색상으로 출력한다.
;============================================================
DrawSquare PROC USES eax edx,
    xpos:DWORD,
    ypos:DWORD,
    color:DWORD

    mov eax, color
    call SetTextColor                    ; 문자 색 설정

    mov dl, BYTE PTR xpos
    mov dh, BYTE PTR ypos
    call Gotoxy                          ; 커서 이동

    mov edx, OFFSET space
    call WriteString                     ; '▒▒' 출력
    ret
DrawSquare ENDP


;============================================================
; GetColor
; (row + col) % 2 를 이용하여 색을 선택한다.
;  - 짝수 칸 → 고정된 whiteColor
;  - 홀수 칸 → 계속 증가하는 ChangeColor (애니메이션 효과)
;============================================================
GetColor PROC USES edx,
    row:DWORD,
    col:DWORD

    mov eax, row
    add eax, col                         ; row + col
    and eax, 1                           ; % 2

    cmp eax, 0
    jne oddSquare                        ; 홀수 → 다른 색

    mov eax, whiteColor                  ; 짝수 칸 → 흰색
    ret

oddSquare:
    mov eax, ChangeColor                 ; 홀수 칸 → 매 프레임 변하는 색
    ret
GetColor ENDP

END main
```

---
## ★★ 4. FindThrees Procedure  
**Q : Create a procedure named FindThrees that returns 1 if an array has three consecutive values of 3 somewhere in the array. Otherwise, return 0. The procedure’s input parameter list contains a pointer to the array and the array’s size. Use the PROC directive with a parameter list when declaring the procedure. Preserve all registers (except EAX) that are modified by the procedure. Write a test program that calls FindThrees several times with different arrays. **  
**A :** 배열에 3개의 연속된 3이 있다면 1을 반환, 없다면 0을 반환하는 프로시저, 다른 레지스터는 보존하며 3번 작동  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

FindThrees PROTO arrayPtr:DWORD, sizearr:DWORD

.data
    arr1 BYTE 1,3,3,3,4,5,6         ; true  (연속 3개의 3 존재)
    arr2 BYTE 3,3,2,3,2,3,3,2,2,3   ; false (연속 3개 없음)
    arr3 BYTE 1,2,3,4,5,6,5,4,3,2,1 ; false
.code

main PROC
    INVOKE FindThrees, ADDR arr1, SIZEOF arr1
    call DumpRegs
    INVOKE FindThrees, ADDR arr2, SIZEOF arr2
    call DumpRegs
    INVOKE FindThrees, ADDR arr3, SIZEOF arr3
    call DumpRegs
    exit 
main ENDP


;==============================================================
; FindThrees
; 배열 안에 **연속된 세 개의 값이 모두 3인지**를 검사한다.
; 조건을 만족 → EAX = 1
; 만족하지 않음 → EAX = 0
;==============================================================
FindThrees PROC USES ebx ecx edx,
    arrayPtr:DWORD,
    sizearr:DWORD

    mov ecx, sizearr
    cmp ecx, 3
    jl noThrees                 ; 원소 3개 미만이면 검사 불가 → 바로 0

    xor ebx, ebx                ; ebx = 현재 인덱스 i
    mov esi, arrayPtr           ; esi = 배열 주소

checkLoop:
    ;-------------------------------
    ; arr[i] == 3 ?
    ;-------------------------------
    mov al, [esi + ebx]
    cmp al, 3
    jne nextIndex

    ;-------------------------------
    ; arr[i+1] == 3 ?
    ;-------------------------------
    mov al, [esi + ebx + 1]
    cmp al, 3
    jne nextIndex

    ;-------------------------------
    ; arr[i+2] == 3 ?  → 3개 연속 검사
    ;-------------------------------
    mov al, [esi + ebx + 2]
    cmp al, 3
    jne nextIndex

    ;-------------------------------
    ; 세 칸 모두 3 → 성공
    ;-------------------------------
    mov eax, 1
    jmp done

;==============================================================
; 다음 인덱스로 이동 (i++)
; 단, i가 size - 3 미만일 때만 검사 가능
;==============================================================
nextIndex:
    inc ebx

    mov edx, ecx
    sub edx, 3                  ; 마지막으로 검사가 가능한 시작 인덱스
    cmp ebx, edx
    jl checkLoop                ; 아직 검사 가능한 구간이 남아있으면 반복

;==============================================================
; 3개 연속된 3이 존재하지 않음
;==============================================================
noThrees:
    xor eax, eax                ; 결과 = 0

done:
    ret
FindThrees ENDP

END main
```

---
## ★★ 5. DifferentInputs Procedure  
**Q : Write a procedure named DifferentInputs that returns EAX = 1 if the values of its three input parameters are all different; otherwise, return with EAX = 0. Use the PROC directive with a parameter list when declaring the procedure. Create a PROTO declaration for your procedure, and call it five times from a test program that passes different inputs.**  
**A :** A, B, C 3개의 숫자를 받아 모두 다르면 1을 반환하는 프로시저 5번 실행  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

DifferentInputs PROTO pA:DWORD, pB:DWORD, pC:DWORD

.data
    t1 DWORD 1, 2, 3
    t2 DWORD 4, 4, 5
    t3 DWORD 6, 7, 6
    t4 DWORD 8, 9, 9
    t5 DWORD 11, 11, 11

.code
main PROC
    INVOKE DifferentInputs, 1, 2, 3
    call DumpRegs

    INVOKE DifferentInputs, 4, 4, 5
    call DumpRegs

    INVOKE DifferentInputs, 6, 7, 6
    call DumpRegs

    INVOKE DifferentInputs, 8, 9, 9
    call DumpRegs

    INVOKE DifferentInputs, 11, 11, 11
    call DumpRegs
    exit 
main ENDP


;============================================================
; DifferentInputs
; 입력된 세 값 pA, pB, pC가 **모두 다른지** 검사한다.
;
; 조건:
;   pA ≠ pB
;   pA ≠ pC
;   pB ≠ pC
;
; 모든 조건을 만족 → EAX = 1
; 하나라도 같음       → EAX = 0
;============================================================
DifferentInputs PROC pA:DWORD, pB:DWORD, pC:DWORD

    ;----------------------------------------
    ; 먼저 A와 B 비교
    ;----------------------------------------
    mov eax, pA
    cmp eax, pB
    je notDiff      ; A == B면 실패

    ;----------------------------------------
    ; A와 C 비교
    ;----------------------------------------
    cmp eax, pC
    je notDiff      ; A == C면 실패

    ;----------------------------------------
    ; B와 C 비교
    ;----------------------------------------
    mov eax, pB
    cmp eax, pC
    je notDiff      ; B == C면 실패

    ;----------------------------------------
    ; 모든 값이 서로 다름
    ;----------------------------------------
    mov eax, 1
    ret

;----------------------------------------
; 하나라도 동일하면 0 반환
;----------------------------------------
notDiff:
    xor eax, eax
    ret

DifferentInputs ENDP

END main
```

---
## ★★ 6. Exchanging Integers  
**Q : Create an array of randomly ordered integers. Using the Swap procedure from Section 8.4.6 as a tool, write a loop that exchanges each consecutive pair of integers in the array.**  
**A :** Swap을 이용해 배열의 정수 쌍을 바꾸는 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

; Swap(pVal1, pVal2)
; 두 DWORD 값의 위치를 교환하는 프로시저
Swap PROTO pVal1:PTR DWORD, pVal2:PTR DWORD

.data
    arr DWORD 1,2,3,4,5,6,7,8       ; 대상 배열
    n   DWORD LENGTHOF arr          ; 배열 길이

.code
main PROC
    mov ecx, n
    shr ecx, 1                      ; n/2 번 반복 (짝 교환)
    xor ebx, ebx                    ; EBX = 배열 인덱스(0,2,4...)

swapLoop:
    lea esi, arr[ebx*4]             ; 첫 번째 값의 주소
    lea edi, arr[ebx*4 + 4]         ; 두 번째 값의 주소
    INVOKE Swap, esi, edi           ; 두 값 교환
    add ebx, 2                      ; 다음 쌍으로 이동
    loop swapLoop

    ;----------------------------
    ; 배열 출력
    ;----------------------------
    xor ebx, ebx
printLoop:
    mov eax, arr[ebx*4]
    call WriteDec
    call Crlf
    inc ebx
    cmp ebx, n
    jl printLoop

    exit 
main ENDP

;---------------------------------------------------
; Swap : 두 DWORD 값을 교환하는 서브루틴
;       pVal1 ↔ pVal2
;---------------------------------------------------
Swap PROC USES esi edi,
    pVal1:PTR DWORD,
    pVal2:PTR DWORD

    mov esi, pVal1
    mov edi, pVal2

    mov eax, [esi]      ; eax ← *pVal1
    xchg eax, [edi]     ; eax ↔ *pVal2
    mov [esi], eax      ; *pVal1 ← eax(원래 pVal2)

    ret
Swap ENDP

END main
```

---
## ★★ 7. Greatest Common Divisor  
**Q : Write a recursive implementation of Euclid’s algorithm for finding the greatest common divisor (GCD) of two integers. Descriptions of this algorithm are available in algebra books and on the Web. Write a test program that calls your GCD procedure five times, using the following pairs of integers: (5,20), (24,18), (11,7), (432,226), (26,13). After each procedure call, display the GCD.**  
**A :** 5쌍의 숫자의 GCD(최대공약수)를 출력하는 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

GCD PROTO a:DWORD, b:DWORD

.data
    pair DWORD 5, 20, 24, 18, 11, 7, 432, 226, 26, 13
.code
main PROC
    mov esi, 0
    mov ecx, 5

printLoop:
    mov eax, pair[esi]        ; 첫 번째 값
    mov ebx, pair[esi+4]      ; 두 번째 값

    INVOKE GCD, eax, ebx      ; GCD 계산

    call WriteDec
    call Crlf

    add esi, 8                ; 다음 쌍으로 이동
    loop printLoop

    exit 
main ENDP

;------------------------------------------------
; GCD(a, b)
; 유클리드 호제법으로 최대공약수 반환
;------------------------------------------------
GCD PROC a:DWORD, b:DWORD
    
    mov eax, a
    mov ebx, b

check:
    cmp ebx, 0              ; b==0?
    je finish               ; 종료: gcd = a

    mov edx, 0
    div ebx                 ; EDX = a mod b

    mov eax, ebx            ; a = b
    mov ebx, edx            ; b = a mod b
    jmp check               ; 반복

finish:
    ; eax = gcd
    ret

GCD ENDP

END main
```

---
## ★★ 8. Counting Matching Elements  
**Q : Write a procedure named CountMatches that receives points to two arrays of signed doublewords, and a third parameter that indicates the length of the two arrays. For each element xi in the first array, if the corresponding yi in the second array is equal, increment a count. At the end, return a count of the number of matching array elements in EAX. Write a test program that calls your procedure and passes pointers to two different pairs of arrays. Use the INVOKE statement to call your procedure and pass stack parameters. Create a PROTO declaration for CountMatches. Save and restore any registers (other than EAX) changed by your procedure.**  
**A :** 두 배열의 동일 인덱스의 값이 같은 개수를 구하는 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

CountMatches PROTO array1Ptr:PTR SDWORD,
                    array2Ptr:PTR SDWORD,
                    lenArr:DWORD

.data
    arr1 SDWORD 10, -3, 25, 40, 8, 4, 9, -12
    arr2 SDWORD 10, -3, 20, 40, 7, 4, 10, -12
    len1 DWORD LENGTHOF arr1

    arr3 SDWORD 1,2,3,4,5,6,7,8,9
    arr4 SDWORD 1,0,3,0,5,0,0,0,9
    len2 DWORD LENGTHOF arr3

.code
main PROC
    INVOKE CountMatches, ADDR arr1, ADDR arr2, len1
    call WriteDec
    call Crlf

    INVOKE CountMatches, ADDR arr3, ADDR arr4, len2
    call WriteDec
    call Crlf

    exit 
main ENDP

;-----------------------------------------------------------
CountMatches PROC USES ebx ecx edx,
    array1Ptr:PTR SDWORD,
    array2Ptr:PTR SDWORD,
    lenArr:DWORD
; CountMatches
; 두 배열의 같은 인덱스에 있는 값이 동일한 경우의 개수를 센다.
;
; 입력:
;   array1Ptr = 첫 번째 배열 주소
;   array2Ptr = 두 번째 배열 주소
;   lenArr    = 배열 길이
;
; 반환:
;   EAX = x[i] == y[i] 인 요소 개수
;-----------------------------------------------------------

    xor eax, eax             ; count = 0
    mov ecx, lenArr          ; 반복 횟수
    mov ebx, 0               ; 인덱스 = 0

match:
    cmp ecx, 0
    je finish

    ; array1[i] 가져오기
    mov edx, array1Ptr
    mov esi, [edx + ebx*4]

    ; array2[i] 가져오기
    mov edx, array2Ptr
    mov edi, [edx + ebx*4]

    ; 두 값 비교
    cmp esi, edi
    jne noMatch
    inc eax                  ; 같으면 count++

noMatch:
    inc ebx                  ; 인덱스 증가
    loop match               ; ecx--

finish:
    ret
CountMatches ENDP

END main
```

---
## ★★★ 9. Counting Nearly Matching Elements  
**Q : Write a procedure named CountNearMatches that receives pointers to two arrays of signed doublewords, a parameter that indicates the length of the two arrays, and a parameter that indicates the maximum allowed difference (called diff) between any two matching elements. For each element xi in the first array, if the difference between it and the corresponding yi in the second array is less than or equal to diff, increment a count. At the end, return a count of the number of nearly matching array elements in EAX. Write a test program that calls CountNearMatches and passes pointers to two different pairs of arrays. Use the INVOKE statement to call your procedure and pass stack parameters. Create a PROTO declaration for CountMatches. Save and restore any registers (other than EAX) changed by your procedure.**  
**A :** 8번의 확장. x[i]와 y[i]의 차가 diff 이하인 경우의 개수를 세는 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

CountNearMatches PROTO array1Ptr:PTR SDWORD,
                    array2Ptr:PTR SDWORD,
                    lenArr:DWORD,
                    diff:DWORD

.data
    arr1 SDWORD 10, 20, 30, 40, 50, 60, 70, 80
    arr2 SDWORD 12, 18, 29, 45, 56, 57, 61, 74   ; 예상 = 4
    len1 DWORD LENGTHOF arr1
    d1 DWORD 3

    arr3 SDWORD -5, 100, 23, 44, 60, 80, 123, -222
    arr4 SDWORD -3, 95, 20, 50, 55, 70, 128, -211 ; 예상 = 5
    len2 DWORD LENGTHOF arr3
    d2 DWORD 5
.code
main PROC
    INVOKE CountNearMatches, ADDR arr1, ADDR arr2, len1, d1
    call WriteDec
    call Crlf

    INVOKE CountNearMatches, ADDR arr3, ADDR arr4, len2, d2
    call WriteDec
    call Crlf

    exit
main ENDP

;-----------------------------------------------------------
CountNearMatches PROC USES ebx ecx edx esi edi,
    array1Ptr:PTR SDWORD,
    array2Ptr:PTR SDWORD,
    lenArr:DWORD,
    diff:DWORD
; CountNearMatches
; 두 배열에서 x[i]와 y[i]의 차이(|x - y|)가 diff 이하인 요소의 개수를 센다.
;
; 입력:
;   array1Ptr = 첫 번째 배열 주소
;   array2Ptr = 두 번째 배열 주소
;   lenArr    = 배열 길이
;   diff      = 허용 오차 범위
;
; 반환:
;   EAX = |x[i] - y[i]| <= diff 인 요소 개수
;-----------------------------------------------------------

    xor eax, eax            ; count = 0
    mov ecx, lenArr         ; 반복 횟수
    mov ebx, 0              ; 인덱스 = 0

match:
    cmp ecx, 0
    je finish

    ; x[i] 읽기
    mov esi, array1Ptr
    mov edx, [esi + ebx*4]  ; edx = x[i]

    ; y[i] 읽기
    mov esi, array2Ptr
    mov edi, [esi + ebx*4]  ; edi = y[i]

    ; |x - y| 계산
    mov esi, edx            ; esi ← x[i]
    sub esi, edi            ; esi = x - y
    cmp esi, 0
    jge absOK
    neg esi                 ; 음수면 절댓값으로 변환
absOK:

    ; diff 비교
    cmp esi, diff
    jg noMatch              ; |x - y| > diff → 불일치
    inc eax                 ; 만족하면 count++

noMatch:
    inc ebx                 ; index++
    loop match              ; ecx--

finish:
    ret
CountNearMatches ENDP

END main
```

---
## ★★★★ 10. Show Procedure Parameters  
**Q : Write a procedure named ShowParams that displays the address and hexadecimal value of the 32-bit parameters on the runtime stack of the procedure that called it. The parameters are to be displayed in order from the lowest address to the highest. Input to the procedure will be a single integer that indicates the number of parameters to display. For example, suppose the following statement in main calls MySample, passing three arguments:**  
```asm
INVOKE MySample, 1234h, 5000h, 6543h
```
**Next, inside MySample, you should be able to call ShowParams, passing the number of parameters you want to display:**  
```asm
MySample PROC first:DWORD, second:DWORD, third:DWORD
paramCount = 3
call ShowParams, paramCount
```
**ShowParams should display output in the following format:**  
```
tack parameters:
---------------------------
Address 0012FF80 = 00001234
Address 0012FF84 = 00005000
Address 0012FF88 = 00006543
```.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

ShowParams PROTO count:DWORD
MySample   PROTO first:DWORD, second:DWORD, third:DWORD

.data
    msgTitle BYTE "Stack parameters:", 0
    msgLine  BYTE "---------------------------", 0
    space    BYTE " ", 0
    eqStr    BYTE "= ", 0
    addrStr  BYTE "Address ", 0

.code
main PROC
    INVOKE MySample, 1234h, 5000h, 6543h
    exit
main ENDP

;-----------------------------------------------------------
MySample PROC first:DWORD, second:DWORD, third:DWORD
    paramCount = 3
    INVOKE ShowParams, paramCount
    ret
MySample ENDP

;-----------------------------------------------------------
ShowParams PROC count:DWORD
; 호출자의 스택에 있는 파라미터 주소와 값을 출력하는 프로시저
; count = 출력할 매개변수 개수
;-----------------------------------------------------------

    ;=== ❗정확한 EBP 프레임 생성 ===
    push ebp
    mov  ebp, esp

    pushad      ; 레지스터 보존 (이제 ebp는 고정됨)

    ; 상단 출력
    mov edx, OFFSET msgTitle
    call WriteString
    call Crlf
    mov edx, OFFSET msgLine
    call WriteString
    call Crlf

    ; ShowParams의 count 인자 = [ebp+8]
    mov ecx, [ebp+8]

    ; 호출자의 첫 번째 매개변수 = [ebp+20]
    mov esi, ebp
    add esi, 20

PrintLoop:
    cmp ecx, 0
    je done

    ; "Address <addr>"
    mov edx, OFFSET addrStr
    call WriteString

    mov eax, esi
    call WriteHex
    mov edx, OFFSET space
    call WriteString

    mov edx, OFFSET eqStr
    call WriteString

    mov eax, [esi]
    call WriteHex
    call Crlf

    add esi, 4
    dec ecx
    jmp PrintLoop

done:
    popad
    pop ebp      ; 프레임 복구
    ret
ShowParams ENDP

END main

```

---
