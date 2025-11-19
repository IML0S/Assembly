# 7. Integer Arithmetic 요약

---

## 1) Shift & Rotate (시프트/로테이트)

비트를 이동하거나 회전시키는 명령들로, 비트 연산·정수 연산 보조·데이터 변환 등에 널리 사용된다.

### ▫ Logical Shift (논리 시프트)
- **SHL(=SAL)**: 왼쪽 이동, LSB=0 삽입 → 값 ×2
- **SHR**: 오른쪽 이동, MSB=0 삽입 → unsigned 나눗셈(/2)

### ▫ Arithmetic Shift (산술 시프트)
- **SAR**: 오른쪽 이동, MSB 유지 → signed 나눗셈(/2)

### ▫ Rotate (회전)
비트 손실이 없으며, CF까지 포함할 수 있다.

- **ROL**: 왼쪽 회전 (MSB → CF → LSB)
- **ROR**: 오른쪽 회전 (LSB → CF → MSB)
- **RCL / RCR**: Carry 포함 회전

### ▫ Double Shift (SHLD / SHRD)
- **SHLD**: 왼쪽 시프트 + source의 상위 비트로 채움  
- **SHRD**: 오른쪽 시프트 + source의 하위 비트로 채움  
→ 두 레지스터를 연결하여 확장된 시프트 구현 가능.

### ▫ Overflow Flag
- 시프트/로테이트 **1비트 이동 시에만 OF 유효**  
- 1비트 초과 이동하면 OF는 정의되지 않음.

---

## 2) Shift/Rotate 활용

### ▫ 배열 비트 이동
배열의 각 바이트를 1비트씩 오른쪽으로 이동시키는 예시 제공.

### ▫ Binary → ASCII 변환 (BinToAsc)
EAX의 32비트 값을 ASCII ‘0’/‘1’ 문자열로 변환.
- SHL로 MSB → CF로 이동
- CF에 따라 ‘0’ 또는 ‘1’ 기록  
→ 바이너리 출력 처리 시 자주 사용.

---

## 3) Multiplication & Division (곱셈/나눗셈)

### ▫ MUL (unsigned)
- 피연산자 크기가 동일해야 함.
- 결과는 **2배 크기** (byte→word, word→dword 등)
- 상위 절반이 0인지 확인하려면 **CF 확인**

### ▫ IMUL (signed)
- **1-operand**: AX / DX:AX / EDX:EAX에 저장
- **2/3-operand**: 결과가 destination 크기로 잘림  
  → **오버플로우 발생 시 OF/CF = 1**
- IMUL 2/3-operand는 연산 후 반드시 OF 체크.

### ▫ Sign Extension (부호 확장)
나눗셈 전 dividend는 **완전한 sign-extension** 필요.
- CBW, CWD, CWDE, CDQ 활용

### ▫ DIV / IDIV
- 8/16/32비트 divisor 사용
- 상태 플래그는 **undefined**
- 나눗셈 overflow 발생 시 **프로그램 예외 발생**
- divisor가 0인지 반드시 검사

---

## 4) Extended Addition / Subtraction

### ▫ ADC (add with carry)
`dst = dst + src + CF`  
→ 다중 정밀도 덧셈에서 필수

### ▫ SBB (subtract with borrow)
`dst = dst - src - CF`  
→ 다중 정밀도 뺄셈에서 사용

---

## 5) ASCII & Unpacked Decimal Arithmetic

### ▫ 특징
ASCII 숫자: 상위 4비트 = `0011b`  
Unpacked decimal: 상위 4비트 = `0000b`

### ▫ 명령들
- **AAA**: ADD 이후 ASCII 보정  
- **AAS**: SUB 이후 ASCII 보정  
- **AAM**: MUL 결과 → unpacked decimal 변환  
- **AAD**: DIV 전 unpacked decimal → binary 변환  

### ▫ ASCII 덧셈 예시
- 두 ASCII 문자열의 십진 덧셈
- AAA로 조정
- carry는 AH에 저장 후 ASCII('0'~'9')로 변환

---

## 6) Packed Decimal (BCD) Arithmetic

### ▫ 명령들
- **DAA**: ADD/ADC 후 packed decimal 보정  
- **DAS**: SUB/SBB 후 packed decimal 보정  

### ▫ Packed BCD 덧셈 예시
1. low byte 덧셈 → DAA  
2. high byte 덧셈 (ADC 포함) → DAA  
3. 최종 carry 반영  
→ BCD 계산 시 필요한 조정 처리

---

## 7장 핵심 요약

| 주제 | 핵심 포인트 |
|------|-------------|
| Shift/Rotate | 논리/산술 시프트, 회전, SHLD/SHRD |
| Multiplication | MUL/IMUL, OF/CF로 overflow 확인 |
| Division | DIV/IDIV, sign-extension 필수, divide overflow 주의 |
| Extended Ops | ADC/SBB → 다중 정밀도 연산 |
| ASCII Decimal | AAA/AAS/AAM/AAD |
| Packed Decimal | DAA/DAS로 BCD 형식 보정 |

---


