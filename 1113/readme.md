
# Assembly Instructions Summary

## 1. Shift & Rotate Instructions

### ▶ Logical Shift

#### **SHL dest, count**

비트를 왼쪽으로 이동(LSB=0)
예제: `shl ax, 1   ; ax를 2배로 만들기`

#### **SHR dest, count**

비트를 오른쪽으로 이동(MSB=0)
예제: `shr bx, 1   ; bx를 2로 나누기(논리)`

### ▶ Arithmetic Shift

#### **SAL dest, count**

SHL과 동일한 동작
예제: `sal al, 1   ; al을 왼쪽 이동`

#### **SAR dest, count**

부호 유지한 채 오른쪽 이동
예제: `sar ax, 1   ; signed ax / 2`

### ▶ Rotate

#### **ROL dest, count**

왼쪽 회전, MSB → CF → LSB
예제: `rol al, 1   ; al의 비트를 순환시킴`

#### **ROR dest, count**

오른쪽 회전
예제: `ror bl, 1   ; bl의 비트를 오른쪽으로 순환`

### ▶ Rotate Through Carry

#### **RCL dest, count**

왼쪽 회전 + CF 포함
예제: `rcl ax, 1   ; CF 포함하여 왼쪽 회전`

#### **RCR dest, count**

오른쪽 회전 + CF 포함
예제: `rcr dx, 1   ; CF 포함하여 오른쪽 회전`

### ▶ Double Shift

#### **SHLD dest, src, count**

dest을 왼쪽으로 밀고 src의 MSB를 채움
예제: `shld ax, bx, 4   ; ax << 4, 빈 비트를 bx의 상위비트로`

#### **SHRD dest, src, count**

dest을 오른쪽으로 밀고 src의 LSB를 채움
예제: `shrd eax, edx, 2   ; eax >> 2, 빈 비트는 edx의 하위비트`

------------------------------------------------------------------------

## 2. Multiply Instructions

### ▶ Unsigned Multiply

#### **MUL r/m**

AL·r/m → AX, AX·r/m → DX:AX
예제: `mul bl   ; AL × BL → AX`

### ▶ Signed Multiply

#### **IMUL r/m**

부호 있는 곱셈
예제: `imul byte ptr [num]   ; AL × num → AX`

#### **IMUL reg, r/m**

결과는 reg 크기로 잘림
예제: `imul ax, bx   ; ax = ax × bx`

#### **IMUL reg, r/m, imm**

세 번째 피연산자 포함
예제: `imul eax, ebx, 10   ; eax = ebx × 10`

------------------------------------------------------------------------

## 3. Division Instructions

### ▶ Unsigned Divide

#### **DIV r/m**

AX ÷ r/m → AL,AH / DX:AX ÷ r/m → AX,DX
예제:
`div bl   ; AX ÷ BL → AL=몫, AH=나머지`

### ▶ Signed Divide

#### **IDIV r/m**

signed 나눗셈
예제:
`idiv cx   ; DX:AX ÷ CX`

------------------------------------------------------------------------

## 4. Sign Extension Instructions

#### **CBW**

AL → AX
예제: `cbw   ; AL을 AH로 sign extend`

#### **CWDE**

AX → EAX
예제: `cwde   ; AX sign-extend → EAX`

#### **CWD**

AX → DX:AX
예제: `cwd   ; AX sign-extend → DX:AX`

#### **CDQ**

EAX → EDX:EAX
예제: `cdq   ; EAX sign-extend → EDX:EAX`

------------------------------------------------------------------------

## 5. Extended Addition / Subtraction

#### **ADC dest, src**

Carry 포함 덧셈\
예제: `adc eax, ebx   ; eax = eax + ebx + CF`

#### **SBB dest, src**

Borrow 포함 뺄셈
예제: `sbb ebx, ecx   ; ebx = ebx - ecx - CF`

------------------------------------------------------------------------

## 6. ASCII / Unpacked Decimal Instructions

#### **AAA**

ADD 후 AL을 unpacked decimal로 조정
예제:

    add al, bl
    aaa

#### **AAS**

SUB 후 AL을 unpacked decimal로 조정
예제:

    sub al, bl
    aas

#### **AAM**

MUL 뒤 AL값을 두 자리 decimal로 분리
예제: `aam   ; AL = (AL/10, AL%10)`

#### **AAD**

DIV 전에 AX의 unpacked decimal을 binary로 변환
예제: `aad   ; AH*10 + AL → AX`

------------------------------------------------------------------------

## 7. Packed Decimal Instructions

#### **DAA**

ADD 후 AL을 packed BCD로 조정
예제:

    add al, bl
    daa

#### **DAS**

SUB 후 AL을 packed BCD로 조정
예제:

    sub al, bl
    das
