# Assembly Language Review Questions and Answers  
어셈블리 언어 리뷰 질문 및 해설

## 1. Provide examples of three different instruction mnemonics.  
세 가지 명령어 기호의 예시로 MOV, ADD, SUB가 있다.

## 2. What is a calling convention, and how is it used in assembly language declarations?  
호출 규약이란 함수 호출 시 인자를 전달하고 반환값을 처리하는 방식. 어셈블리에서는 stdcall, cdecl 같은 키워드로 선언하며, 스택 정리 책임이 달라진다.

## 3. How do you reserve space for the stack in a program?  
SUB ESP, n 명령어로 스택 포인터를 감소시켜 n 바이트 공간을 확보한다.

## 4. Explain why the term assembler language is not quite correct.  
올바른 용어는 ‘Assembly Language’이고, ‘Assembler’는 코드를 기계어로 번역하는 프로그램을 의미한다.

## 5. Explain the difference between big endian and little endian. Also, look up the origins of this term on the Web.  
Big Endian: 가장 중요한 바이트(MSB)를 먼저 저장  
Little Endian: 가장 덜 중요한 바이트(LSB)를 먼저 저장  
기원: 1980년 Danny Cohen이 Jonathan Swift의 『걸리버 여행기』에서 유래한 용어를 컴퓨터 과학에 도입했다

## 6. Why might you use a symbolic constant rather than an integer literal in your code?  
유지보수와 가독성을 높이며, 코드 변경 시 일괄 수정이 가능해 오류를 줄일 수 있다.

## 7. How is a source file different from a listing file?  
소스 파일: 개발자가 작성한 코드 (.asm)  
리스팅 파일: 어셈블러가 생성한 코드 + 주소 + 기계어 (.lst)

## 8. How are data labels and code labels different?  
데이터 라벨: 변수나 상수에 사용  
코드 라벨: 실행 위치를 지정 (예: 점프 대상)

## 9. (True/False): An identifier cannot begin with a numeric digit.  
True

## 10. (True/False): A hexadecimal literal may be written as 0x3A.  
True

## 11. (True/False): Assembly language directives execute at runtime.  
False, 컴파일 타임에 의해 처리된다.

## 12. (True/False): Assembly language directives can be written in any combination of uppercase and lowercase letters.  
True

## 13. Name the four basic parts of an assembly language instruction.  
라벨(label)  
명령어(mnemonic)  
피연산자(operands)  
주석(comment)

## 14. (True/False): MOV is an example of an instruction mnemonic.  
True

## 15. (True/False): A code label is followed by a colon (:), but a data label does not end with a colon.  
False

## 16. Show an example of a block comment.  
COMMENT !  
 This line is a comment.  
 This line is also a comment.  
!

## 17. Why is it not a good idea to use numeric addresses when writing instructions that access variables?  
변수 접근 시 숫자 주소를 사용하지 말아야 하는 이유는 유지보수가 어렵고, 코드 가독성이 떨어지며, 주소 변경 시 오류 발생 가능성이 높다.

## 18. What type of argument must be passed to the ExitProcess procedure?  
32비트 정수값(DWORD) 전달

## 19. Which directive ends a procedure?  
RET 또는 ENDP

## 20. In 32-bit mode, what is the purpose of the identifier in the END directive?  
프로그램의 시작 지점을 지정한다. 예시로 main이 있다.

## 21. What is the purpose of the PROTO directive?  
프로시저의 원형을 선언하여 호출 시 인자와 반환값을 명확히 한다.

## 22. (True/False): An Object file is produced by the Linker.  
False, 오브젝트 파일은 링커가 아닌 어셈블러가 생성한다.

## 23. (True/False): A Listing file is produced by the Assembler.  
True

## 24. (True/False): A link library is added to a program just before producing an Executable file.  
True

## 25. Which data directive creates a 32-bit signed integer variable?  
SDWORD

## 26. Which data directive creates a 16-bit signed integer variable?  
SWORD

## 27. Which data directive creates a 64-bit unsigned integer variable?  
QWORD

## 28. Which data directive creates an 8-bit signed integer variable?  
SBYTE

## 29. Which data directive creates a 10-byte packed BCD variable?  
TBYTE
