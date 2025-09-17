## Programming Exercises
Use any high-level programming language you wish for the following programming exercises.
Do not call built-in library functions that accomplish these tasks automatically. (Examples are `sprintf` and `sscanf` from the Standard C library.)

---

## 1. Convert 16-bit Binary String to Integer

**16비트의 이진수를 입력받고 10진수로 변환하기**

```python
def binary_to_int(bin_str):
    result = 0
    for i in range(16):
        if bin_str[i] == '1':
            result += 1 << (15 - i)
    return result

user_input = input()
if len(user_input) == 16 and all(c == '0' or c == '1' for c in user_input):
    decimal = binary_to_int(user_input)
    print(decimal)
else:
    print("입력 오류")
```

---

## 2. Convert 32-bit Hexadecimal String to Integer

**32비트의 16진수를 입력받고 10진수로 변환하기**

```python
def hex_to_int(hex_str):
    result = 0
    hex_str = hex_str.upper()
    if hex_str.startswith("0X"):
        hex_str = hex_str[2:]

    if len(hex_str) != 8:
        return -1

    for i in range(8):
        c = hex_str[i]
        if '0' <= c <= '9':
            digit = ord(c) - ord('0')
        elif 'A' <= c <= 'F':
            digit = ord(c) - ord('A') + 10
        else:
            return -1
        result += digit * (16 ** (7 - i))

    return result

user_input = input()
if len(user_input) == 8 or (len(user_input) == 10 and user_input[:2].upper() == "0X"):
    decimal = hex_to_int(user_input)
    if decimal != -1:
        print(decimal)
    else:
        print("입력 오류")
else:
    print("입력 오류")
```

---

## 3. Convert Integer to Binary String

**정수를 입력받아 이진수 문자열로 변환하기**

```python
def int_to_binary(n):
    if n == 0:
        return "0"

    result = ""
    while n > 0:
        remain = n % 2
        result = str(remain) + result
        n = n // 2

    return result

user_input = input()
if user_input.isdigit():
    num = int(user_input)
    binary_str = int_to_binary(num)
    print(binary_str)
else:
    print("입력 오류")
```

---

## 4. Convert Integer to Hexadecimal String

**정수를 입력받아 16진수 문자열로 변환하기**

```python
def int_to_hex(n):
    if n == 0:
        return "0"
    hex_chars = "0123456789ABCDEF"
    result = ""
    while n > 0:
        remain = n % 16
        result = hex_chars[remain] + result
        n = n // 16
    return result

user_input = input()
if user_input.isdigit():
    num = int(user_input)
    hex_str = int_to_hex(num)
    print(hex_str)
else:
    print("입력 오류")
```

---

## 5. Addition in Base `b` (2 ≤ b ≤ 10)

**임의의 진법에서 두 수를 더하는 함수**

```python
def add_base_b(a, b, base):
    if not (2 <= base <= 10):
        return "기수 오류"
    max_len = max(len(a), len(b))
    a = a.zfill(max_len)
    b = b.zfill(max_len)

    carry = 0
    result = ""

    for i in range(max_len - 1, -1, -1):
        digit_a = ord(a[i]) - ord('0')
        digit_b = ord(b[i]) - ord('0')

        if digit_a >= base or digit_b >= base:
            return "잘못된 숫자"

        total = digit_a + digit_b + carry
        carry = total // base
        digit_result = total % base
        result = chr(digit_result + ord('0')) + result

    if carry > 0:
        result = chr(carry + ord('0')) + result

    return result

try:
    base = int(input("진법: "))
    a = input("첫 번째 수: ")
    b = input("두 번째 수: ")

    result = add_base_b(a, b, base)
    print("결과:", result)

except ValueError:
    print("입력이 잘못되었습니다. 숫자만 입력해주세요.")
```

---

## 6. Addition of Hexadecimal Strings (up to 1,000 digits)

**길이가 1000자리까지의 16진수를 문자열로 받아 합하고 반환하기**

```python
def add_hex_strings(hex1, hex2):
    hex_map = {ch: i for i, ch in enumerate('0123456789ABCDEF')}
    rev_hex_map = {i: ch for ch, i in hex_map.items()}
    hex1 = hex1.upper()
    hex2 = hex2.upper()
    max_len = max(len(hex1), len(hex2))
    hex1 = hex1.zfill(max_len)
    hex2 = hex2.zfill(max_len)
    carry = 0
    result = []
    for i in range(max_len - 1, -1, -1):
        digit1 = hex_map.get(hex1[i], -1)
        digit2 = hex_map.get(hex2[i], -1)
        if digit1 == -1 or digit2 == -1:
            return "잘못된 입력: 16진수 문자만 사용하세요."
        total = digit1 + digit2 + carry
        carry = total // 16
        result.append(rev_hex_map[total % 16])
    if carry > 0:
        result.append(rev_hex_map[carry])
    return ''.join(reversed(result))

hex1 = input("첫 번째 수: ").strip()
hex2 = input("두 번째 수: ").strip()
sum_result = add_hex_strings(hex1, hex2)
print(sum_result)
```

---

## 7. Multiply Hexadecimal String by Single Hexadecimal Digit

**한 자리 16진수와 최대 천 자리 16진수의 곱**

```python
def multiply_hex_by_digit(hex_str, single_digit):
    hex_map = {ch: i for i, ch in enumerate('0123456789ABCDEF')}
    rev_hex_map = {i: ch for ch, i in hex_map.items()}
    hex_str = hex_str.upper()
    single_digit = single_digit.upper()
    if single_digit not in hex_map:
        return "잘못된 입력: 한 자리 16진수 문자만 사용하세요."

    digit_val = hex_map[single_digit]
    carry = 0
    result = []
    for i in range(len(hex_str) - 1, -1, -1):
        if hex_str[i] not in hex_map:
            return "잘못된 입력: 16진수 문자열만 사용하세요."
        val = hex_map[hex_str[i]]
        product = val * digit_val + carry
        carry = product // 16
        result.append(rev_hex_map[product % 16])

    if carry > 0:
        while carry > 0:
            result.append(rev_hex_map[carry % 16])
            carry //= 16

    return ''.join(reversed(result))

hex_str = input("16진수 문자열: ").strip()
single_digit = input("한 자리 16진수 문자: ").strip()
product = multiply_hex_by_digit(hex_str, single_digit)
print("곱셈 결과:", product)
```

---

## 8. Java Program & Bytecode Analysis

**계산식 `X = (Y + 4) * 3` 포함 코드 및 바이트코드 분석**

```java
public class CalcExample {
    public static void main(String[] args) {
        int Y = 5; // 초기값 필요
        int X = (Y + 4) * 3;
        System.out.println(X);
    }
}
```

**컴파일 & 바이트코드 분석**

```bash
cd "C:\java class\src\first"
javac HexCalc.java
javap -c HexCalc
```

**바이트코드 및 주석**

```
Compiled from "HexCalc.java"
public class HexCalc {
  public HexCalc();
    Code:
       0: aload_0
       1: invokespecial #1  // Object 생성자 호출
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: iconst_2         // Y = 2
       1: istore_1         // Y 저장
       2: iload_1          // Y 불러오기
       3: iconst_4         // 상수 4
       4: iadd             // Y + 4
       5: iconst_3         // 상수 3
       6: imul             // (Y + 4) * 3
       7: istore_2         // X 저장
       8: getstatic #2     // System.out
      11: iload_2          // X 불러오기
      12: invokevirtual #3 // println(int)
      15: return
}
```

---

## 9. Subtraction of Unsigned Binary Integers

**부호 없는 이진수의 뺄셈 (2의 보수 이용)**

```python
def binary_add(a: str, b: str) -> str:
    max_len = max(len(a), len(b))
    a = a.zfill(max_len)
    b = b.zfill(max_len)
    carry = 0
    result = ''
    for i in range(max_len - 1, -1, -1):
        total = int(a[i]) + int(b[i]) + carry
        carry = total // 2
        result = str(total % 2) + result
    if carry:
        result = '1' + result
    return result

def twos_complement(bin_str: str) -> str:
    ones = ''.join('1' if b == '0' else '0' for b in bin_str)
    return binary_add(ones, '1'.zfill(len(bin_str)))

def binary_subtract(a: str, b: str) -> str:
    max_len = max(len(a), len(b))
    a = a.zfill(max_len)
    b = b.zfill(max_len)
    b_twos = twos_complement(b)
    result = binary_add(a, b_twos)
    return result[-max_len:]

print("Test 1: 10001000 - 00000101 =", binary_subtract("10001000", "00000101"))
print("Test 2: 01100110 - 00010010 =", binary_subtract("01100110", "00010010"))
print("Test 3: 11110000 - 00110000 =", binary_subtract("11110000", "00110000"))
```

---
