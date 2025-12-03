# Short Answer 문제 풀이  

## 문제 1  
**Q : Which Direction flag setting causes index registers to move backward through memory when executing string primitives?**  
문자열 명령을 실행할 때 인덱스 레지스터가 메모리를 역방향으로 이동하게 만드는 방향 플래그 설정은 무엇인가?  
**A :** 1  

---

## 문제 2  
**Q : When a repeat prefix is used with STOSW, what value is added to or subtracted from the index register?**  
STOSW에 반복 접두사를 사용하면 인덱스 레지스터에 얼마가 더해지거나 빠지는가?  
**A :** 2  

---

## 문제 3  
**Q : In what way is the CMPS instruction ambiguous?**  
CMPS 명령은 어떤 점에서 모호한가?  
**A :** rep 접두사와 함께 사용할 때, CMPS 명령어는 두 값이 어떻든 정해진 횟수만큼 비교하면서 지나간다.  

---

## 문제 4  
**Q : When the Direction flag is clear and SCASB has found a matching character, where does EDI point?**  
방향 플래그가 0이고 SCASB가 일치하는 문자를 찾았을 때, EDI는 어디를 가리키는가?  
**A :** AL과 같은 값이 저장된 주소에서 +1 된 곳을 가리킨다.  

---

## 문제 5  
**Q : When scanning an array for the first occurrence of a particular character, which repeat prefix would be best?**  
특정 문자가 처음 나타나는 위치를 배열에서 찾을 때 어떤 반복 접두사가 가장 적합한가?  
**A :** REPNE 또는 REPNZ  

---

## 문제 6  
**Q : What Direction flag setting is used in the Str_trim procedure from Section 9.3?**  
9.3절의 Str_trim 프로시저에서는 어떤 방향 플래그 설정이 사용되는가?  
**A :** 뒤에서부터 비교하기 위해 1로 세팅된다.  

---

## 문제 7  
**Q : Why does the Str_trim procedure from Section 9.3 use the JNE instruction?**  
9.3절의 Str_trim 프로시저가 왜 JNE 명령을 사용하는가?  
**A :** 받은 문자와 다른 부분을 찾았을 때, 그 다음 주소에 널 스트링을 넣어 문자열을 매듭짓기 위함이다.  

---

## 문제 8  
**Q : What happens in the Str_ucase procedure from Section 9.3 if the target string contains a digit?**  
9.3절의 Str_ucase 프로시저에서 문자열에 숫자가 포함되어 있으면 어떻게 되는가?  
**A :** 'a' ~ 'z' 범위 내에 있는 경우에만 대문자로 변환한다. 숫자는 'a'보다 작으므로 그냥 아무 일없이 점프한다.  

---

## 문제 9  
**Q : If the Str_length procedure from Section 9.3 used SCASB, which repeat prefix would be most appropriate?**  
9.3절의 Str_length 프로시저가 SCASB를 사용한다면 어떤 반복 접두사가 가장 적절한가?  
**A :** REPNZ 또는 REPNE  

---

## 문제 10  
**Q : If the Str_length procedure from Section 9.3 used SCASB, how would it calculate and return the string length?**  
9.3절의 Str_length 프로시저가 SCASB를 사용한다면 문자열 길이를 어떻게 계산하고 반환하는가?  
**A :** 길이는 (EDI - pString) - 1 이다.  

---

## 문제 11  
**Q : What is the maximum number of comparisons needed by the binary search algorithm when an array contains 1,024 elements?**  
배열이 1,024개의 요소를 포함할 때 이진 탐색 알고리즘이 필요로 하는 최대 비교 횟수는 얼마인가?  
**A :** 10회  

---

## 문제 12  
**Q : In the FillArray procedure from the Binary Search example in Section 9.5, why must the Direction flag be cleared by the CLD instruction?**  
9.5절의 FillArray 프로시저에서 왜 CLD 명령으로 방향 플래그를 0으로 설정해야 하는가?  
**A :** 낮은 주소부터 차례대로 값을 채우기 위해  

---

## 문제 13  
**Q : In the BinarySearch procedure from Section 9.5, why could the statement at label L2 be removed without affecting the outcome?**  
9.5절의 BinarySearch 프로시저에서 왜 L2의 명령을 지워도 결과가 달라지지 않는가?  
**A :** 이미 edx와 edi는 비교되었다.  

---

## 문제 14  
**Q : In the BinarySearch procedure from Section 9.5, how might the statement at label L4 be eliminated?**  
9.5절의 BinarySearch 프로시저에서 L4의 명령은 어떻게 제거할 수 있는가?  
**A :** L4를 지우고 다른 점프문을 `jmp L1`으로 바꾼다.  
