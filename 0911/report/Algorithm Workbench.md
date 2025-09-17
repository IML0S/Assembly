# Programming Exercises

Use any high-level programming language you wish for the following programming exercises.
Do not call built-in library functions that accomplish these tasks automatically. (Examples are sprintf and sscanf from the Standard C library.)

---

## 1. Write a function that receives a string containing a 16-bit binary integer. The function must return the string’s integer value.

**16비트의 이진수를 입력받고 10진수로 변환하기**

```python
def binary_to_int(bin_str):
    result=0
    for i in range(16):
        if bin_str[i]=='1':
            result += 1<<(15-i)
    return result

user_input=input()
if len(user_input)==16 and all(c=='0' or c=='1' for c in user_input): #all 내의 조건이 참이면 1을 반환함
    decimal = binary_to_int(user_input)
    print(decimal)
else:
    print("입력 오류")
```

---

## 2. Write a function that receives a string containing a 32-bit hexadecimal integer. The function must return the string’s integer value.

**32비트의 16진수를 입력받고 10진수로 변환하기**

```python
def hex_to_int(hex_str):
    result = 0
    hex_str = hex_str.upper()  # 대소문자 통일
    if hex_str.startswith("0X"):
        hex_str = hex_str[2:]#입력 받은 것에 0x가 들어가있으면 인덱스2부터 시작해 0x를 없앰

    if len(hex_str)!=8:
        return -1

    for i in range(8):
        c = hex_str[i]
        if '0' <= c <= '9':
            digit = ord(c) - ord('0')
        elif 'A' <= c <= 'F':
            digit = ord(c) - ord('A') + 10
        else:
            return -1  # 16진수를 벗어나는 입력은 -1 반환
        result+=digit*(16 ** (7 - i))

    return result
user_input = input()
if len(user_input) == 8 or (len(user_input) ==
    10 and user_input[:2].upper() == "0X"):
    decimal = hex_to_int(user_input)
    if decimal != -1:
        print(decimal)
    else:
        print("입력 오류")
else:
    print("입력 오류")
```

---

## 3. Write a function that receives an integer. The function must return a string containing the binary representation of the integer.

```python
def int_to_binary(n):
    if n == 0:
        return "0"

    result=""
    while n > 0:
        remain=n%2
        result = str(remain)+result   #result앞에 나머지(0or1)을 붙임
        n=n//2

    return result

user_input=input()

if user_input.isdigit():
    num=int(user_input)
    binary_str=int_to_binary(num)
    print(binary_str)
else:
    print("입력 오류")
```

---

## 4. Write a function that receives an integer. The function must return a string containing the hexadecimal representation of the integer.

```python
def int_to_hex(n):
    if n == 0:
        return "0"
    hex_chars = "0123456789ABCDEF"
    result=""
    while n > 0:
        remain=n%16
        result=hex_chars[remain] + result
        n=n//16
    return result

user_input = input()

if user_input.isdigit():
    num=int(user_input)
    hex_str=int_to_hex(num)
    print(hex_str)
else:
    print("입력 오류")
```

---

## 5. Addition in base b

```python
def add_base_b(a, b, base):
    if not (2 <= base <= 10):
        return "기수 오류"
    max_len=max(len(a), len(b))
    a=a.zfill(max_len)
    b=b.zfill(max_len)

    carry=0
    result=""

    for i in range(max_len - 1, -1, -1):
        digit_a=ord(a[i])-ord('0')
        digit_b=ord(b[i])-ord('0')

        if digit_a >= base or digit_b >= base:
            return "잘못된 숫자"

        total=digit_a+digit_b+carry
        carry=total//base#합이 기수보다 크기 때문에 캐리 발생
        digit_result=total%base
        result=chr(digit_result+ord('0'))+result

    if carry > 0:
        result=chr(carry+ord('0'))+result#캐시가 발생해 한 자리 추가됨

    return result

try:
    base=int(input("진법: "))
    a=input("첫 번째 수: ")
    b=input("두 번째 수: ")

    result=add_base_b(a, b, base)
    print("결과:", result)

except ValueError:
    print("입력이 잘못되었습니다. 숫자만 입력해주세요.")
```

---

## 6. Write a function that adds two hexadecimal strings, each as long as 1,000 digits. Return a hexadecimal string that represents the sum of the inputs.

**길이가 1000자리까지의 16진수를 문자열로 받아 합하고 반환하기**

```python
def add_hex_strings(hex1, hex2):
    hex_map = {ch: i for i, ch in enumerate('0123456789ABCDEF')}
    rev_hex_map = {i: ch for ch, i in hex_map.items()}
    hex1=hex1.upper()
    hex2=hex2.upper()
    max_len=max(len(hex1), len(hex2))
    hex1=hex1.zfill(max_len)
    hex2=hex2.zfill(max_len)
    carry=0
    result=[]
    for i in range(max_len - 1, -1, -1):
        digit1=hex_map.get(hex1[i], -1)
        digit2=hex_map.get(hex2[i], -1)
        if digit1 == -1 or digit2 == -1:
            return "잘못된 입력: 16진수 문자만 사용하세요."
        total=digit1+digit2+carry
        carry=total//16  #5번과 원리는 같음
        result.append(rev_hex_map[total % 16])
    if carry > 0:
        result.append(rev_hex_map[carry])
    return ''.join(reversed(result))

hex1=input("첫 번째 수: ").strip()
hex2=input("두 번째 수: ").strip()
sum_result = add_hex_strings(hex1, hex2)
print(sum_result)
```

---

## 7. Write a function that multiplies a single hexadecimal digit by a hexadecimal digit string as long as 1,000 digits. Return a hexadecimal string that represents the product.

**한 자리 수의 16진수와 최대 천 자리의 16진수의 곱을 출력한다.**

```python
def multiply_hex_by_digit(hex_str, single_digit):
    hex_map={ch: i for i, ch in enumerate('0123456789ABCDEF')}
    rev_hex_map={i: ch for ch, i in hex_map.items()}
    hex_str=hex_str.upper()
    single_digit=single_digit.upper()
    if single_digit not in hex_map:
        return "잘못된 입력: 한 자리 16진수 문자만 사용하세요."

    digit_val=hex_map[single_digit]
    carry=0
    result=[]
    for i in range(len(hex_str) - 1, -1, -1):
        if hex_str[i] not in hex_map:
            return "잘못된 입력: 16진수 문자열만 사용하세요."

        val=hex_map[hex_str[i]]
        product=val*digit_val+carry
        carry=product//16
        result.append(rev_hex_map[product%16])

    if carry > 0:
        while carry > 0:        #캐리가 16보다 클 수도 있으니 반복문을 돌린다
            result.append(rev_hex_map[carry % 16])
            carry//=16

    return ''.join(reversed(result))

hex_str=input("16진수 문자열: ").strip()
single_digit=input("한 자리 16진수 문자: ").strip()
product=multiply_hex_by_digit(hex_str, single_digit)
print("곱셈 결과:", product)
```

---

## 8. Write a Java program that contains the calculation shown below. Then, use the javap –c command to disassemble your code. Add comments to each line that provide your best guess as to its purpose.

`int Y; int X = (Y + 4) * 3;`

**계산 X = (Y + 4) \* 3을 포함하는 java 코드를 작성하고 javap-c 명령어로 바이트코드를 분석한 뒤 주석을 다는 문제**

자바코드

```java
public class CalcExample {
    public static void main(String[] args) {
        int Y = 5; // 초기값을 반드시 지정해야 함
        int X = (Y + 4) * 3;
        System.out.println(X);
    }
}
```

```bash
cd "C:\java class\src\first"
javac HexCalc.java
```

명령어를 입력해 바이트 코드를 생성한다
`javap -c HexCalc` 명령어로 바이트 코드를 분석한다.

바이트 코드 및 주석

```
Compiled from "HexCalc.java"
public class HexCalc {
  public HexCalc();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: iconst_2                          // Y = 2
       1: istore_1                          // 저장: local variable 1 (Y)
       2: iload_1                           // Y 불러오기
       3: iconst_4                          // 상수 4
       4: iadd                              // Y + 4
       5: iconst_3                          // 상수 3
       6: imul                              // (Y + 4) * 3
       7: istore_2                          // 저장: local variable 2 (X)
       8: getstatic     #2                  // System.out
      11: iload_2                           // X 불러오기
      12: invokevirtual #3                  // println(int)
      15: return
}
```

---

## 9. Devise a way of subtracting unsigned binary integers. Test your technique by subtracting binary 00000101 from binary 10001000, producing 10000011. Test your technique with at least two other sets of integers, in which a smaller value is always subtracted from a larger one.

**부호 없는 이진수를 빼는 방법으로 2의 보수를 사용할 것이다**

```python
def binary_add(a: str, b: str) -> str:
    max_len = max(len(a), len(b))
    a=a.zfill(max_len)
    b=b.zfill(max_len)
    carry=0
    result=''
    for i in range(max_len - 1, -1, -1):
        total=int(a[i])+int(b[i])+carry
        carry=total//2
        result=str(total%2)+result
    if carry:
        result='1'+result
    return result

def twos_complement(bin_str: str) -> str:#타입 힌트라 한다고 한다. 협업에 실수가 줄어드는 등의 장점이 있다.
    ones = ''.join('1' if b == '0' else '0' for b in bin_str)
    return binary_add(ones,'1'.zfill(len(bin_str)))

def binary_subtract(a: str, b: str) -> str:
    max_len = max(len(a), len(b))
    a=a.zfill(max_len)
    b=b.zfill(max_len)

    b_twos=twos_complement(b)
    result=binary_add(a, b_twos)

    return result[-max_len:]  # 캐리 비트 없애기
print("Test 1: 10001000 - 00000101 =", binary_subtract("10001000", "00000101"))
print("Test 2: 01100110 - 00010010 =", binary_subtract("01100110", "00010010"))
print("Test 3: 11110000 - 00110000 =", binary_subtract("11110000", "00110000"))
```
