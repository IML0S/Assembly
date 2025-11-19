## 문제 1

**Q : Write a sequence of shift instructions that cause AX to be
sign-extended into EAX. In other words, the sign bit of AX is copied
into the upper 16 bits of EAX. Do not use the CWD instruction.**

AX를 EAX로 부호 확장하기 위해 AX의 부호 비트를 EAX의 상위
16비트에 복사하는 시프트 명령어를 작성하시오. 단, CWD는 사용하지 말 것.

**풀이**

``` asm
shl eax, 16
sar eax, 16
```

## 문제 2

**Q : Suppose the instruction set contained no rotate instructions. Show
how you would use SHR and a conditional jump instruction to rotate the
contents of the AL register 1 bit to the right.**

회전 명령이 없다고 가정할 때, SHR과 조건 분기를 사용하여 AL을
오른쪽으로 1비트 회전시키는 코드를 작성하시오.

**풀이**

``` asm
shr al, 1
jnc next
or  al, 80h
next:
```

## 문제 3

**Q : Write a logical shift instruction that multiplies the contents of
EAX by 16.**

EAX 값을 16배로 만드는 논리 시프트 명령을 작성하시오.

**풀이**

``` asm
shl eax, 4
```

## 문제 4

**Q : Write a logical shift instruction that divides EBX by 4.**

EBX를 4로 나누는 논리 시프트 명령을 작성하시오.

**풀이**

``` asm
shr ebx, 2
```

## 문제 5

**Q : Write a single rotate instruction that exchanges the high and low
halves of the DL register.**

DL 레지스터의 상위 4비트와 하위 4비트를 맞바꾸는 회전 명령
하나를 작성하시오.

**풀이**

``` asm
ror dl, 4
```

## 문제 6

**Q : Write a single SHLD instruction that shifts the highest bit of the
AX register into the lowest bit position of DX and shifts DX one bit to
the left.**

AX의 최상위 비트를 DX의 최하위 비트로 이동시키고, DX를 한 비트
왼쪽으로 이동시키는 SHLD 명령 하나를 작성하시오.

**풀이**

``` asm
shld dx, ax, 1
```

## 문제 7

**Q : Write a sequence of instructions that shift three memory bytes to
the right by 1 bit position.**

세 개의 메모리 바이트를 오른쪽으로 1비트씩 시프트하는 코드를
작성하시오.

**풀이**

``` asm
shr byte ptr [byteArray],   1
shr byte ptr [byteArray+1], 1
shr byte ptr [byteArray+2], 1
```

## 문제 8

**Q : Write a sequence of instructions that shift three memory words to
the left by 1 bit position.**

세 개의 메모리 워드를 왼쪽으로 1비트씩 시프트하는 코드를
작성하시오.

**풀이**

``` asm
shl word ptr [wordArray],   1
shl word ptr [wordArray+2], 1
shl word ptr [wordArray+4], 1
```

## 문제 9

**Q : Write instructions that multiply -5 by 3 and store the result in a
16-bit variable val1.**

-5 × 3 의 결과를 계산하여 16비트 변수 val1에 저장하는 코드를
작성하시오.

**풀이**

``` asm
mov ax, 3
imul ax, -5
mov val1, ax
```

## 문제 10

**Q : Write instructions that divide -276 by 10 and store the result in
a 16-bit variable val1.**

-276 ÷ 10 의 결과를 계산해 16비트 변수 val1에 저장하는 코드를
작성하시오. IDIV 사용 전 부호 확장을 해야 함.

**풀이**

``` asm
mov ax, -276
cwd
idiv 10
mov val1, ax
```

## 문제 11

**Q : Implement the following C++ expression in assembly language, using
32-bit unsigned operands:**\
`val1 = (val2 * val3) / (val4 - 3)`

다음 C++ 수식을 32비트 부호 없는 정수 기준으로 어셈블리로
구현하시오.

**풀이**

``` asm
mov eax, val2
mul val3
mov ebx, val4
sub ebx, 3
div ebx
mov val1, eax
```

## 문제 12

**Q : Implement the following C++ expression in assembly language, using
32-bit signed operands:**\
`val1 = (val2 / val3) * (val1 + val2)`

다음 C++ 수식을 32비트 부호 있는 정수 기준으로 어셈블리로
구현하시오.

**풀이**

``` asm
mov eax, val2
cdq
idiv val3
mov ebx, val1
add ebx, val2
imul eax, ebx
mov val1, eax
```

## 문제 13

**Q : Write a procedure that displays an unsigned 8-bit binary value in
decimal format. (0\~99)**

0\~99 범위의 8비트 값을 십진수로 출력하는 절차를 작성하시오.
사용할 수 있는 외부 함수는 WriteChar뿐임.

**풀이**

``` asm
showDecimal8:
    aam
    or ax, 3030h

    push eax
    mov al, ah
    call WriteChar

    pop eax
    call WriteChar
    ret
```

## 문제 14

**Q : Challenge: AX=0072h이고 AF=1일 때 AAA의 결과를 구하시오.**

AX = 0072h이고 보조 캐리(AF)가 설정된 상태에서 AAA 명령 실행
결과를 구하고 이유를 설명하시오.

**풀이**\
AAA는 AF가 1이므로 AH에 1을 더하고 AL에 6을 더함.\
따라서 결과는 `AX = 0108h`.

## 문제 15

**Q : Challenge: Using only SUB, MOV, AND instructions, compute x = n
mod y where y는 2의 거듭제곱.**

SUB, MOV, AND만 사용하여 n mod y를 구하시오. 단 y는 2의
거듭제곱.

**풀이**

``` asm
mov edx, divisor
sub edx, 1
mov eax, dividend
and eax, edx
mov answer, eax
```

## 문제 16

**Q : Challenge: Using only SAR, ADD, and XOR instructions, compute
absolute value of EAX.**

SAR, ADD, XOR만 사용하여 EAX의 절댓값을 계산하시오.

**풀이:**

``` asm
mov edx, eax
sar edx, 31
add eax, edx
xor eax, edx
```
