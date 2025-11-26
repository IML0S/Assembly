
# Short Answer 문제 풀이

1. **Which statements belong in a procedure's epilogue when the procedure has stack parameters and local variables?**  
→ 스택 매개변수와 지역 변수가 있는 프로시저의 에필로그에는 어떤 명령들이 포함되는가?  
**A:**
```
mov esp, ebp
pop ebp
; or leave
ret (n*4)
```

2. **When a C function returns a 32-bit integer, where is the return value stored?**  
→ C 함수가 32비트 정수를 반환할 때 반환값은 어디에 저장되는가?  
**A:** EAX register.

3. **How does a program using the STDCALL calling convention clean up the stack after a procedure call?**  
→ STDCALL 호출 규약에서는 프로시저 호출 후 스택을 어떻게 정리하는가?  
**A:** The callee cleans the stack using `ret (n*4)`.

4. **How is the LEA instruction more powerful than the OFFSET operator?**  
→ LEA 명령이 OFFSET보다 강력한 이유는 무엇인가?  
**A:** LEA can perform complex address calculations at runtime.  
```
lea eax, [ebx + ecx*2]
```

5. **In the C++ example shown in Section 8.2.3, how much stack space is used by a variable of type int?**  
→ 8.2.3 절의 C++ 예제에서 int 변수는 얼마의 스택 공간을 사용하는가?  
**A:** 4 bytes.

6. **What advantages might the C calling convention have over the STDCALL calling convention?**  
→ C 호출 규약이 STDCALL보다 갖는 장점은 무엇인가?  
**A:** Supports variable arguments and has higher portability.

7. **(True/False): When using the PROC directive, all parameters must be listed on the same line.**  
→ (참/거짓): PROC 지시어를 사용할 때 매개변수는 모두 같은 줄에 적어야 한다.  
**A:** False.  
매개변수는 여러 줄에 나누어 작성해도 된다.

8. **(True/False): If you pass a variable containing the offset of an array of bytes to a procedure that expects a pointer to an array of words, the assembler will flag this as an error.**  
→ (참/거짓): 바이트 배열 오프셋을 워드 배열 포인터에 전달하면 오류가 발생한다.  
**A:** False.  
MASM은 타입 검사를 강하게 하지 않아 컴파일 타임 오류가 발생하지 않는다.

9. **(True/False): If you pass an immediate value to a procedure that expects a reference parameter, you can generate a general-protection fault.**  
→ (참/거짓): 참조 매개변수에 즉시값을 전달하면 GPF가 발생할 수 있다.  
**A:** True.  
참조 매개변수는 유효한 메모리 주소가 필요하며 즉시값은 잘못된 주소가 되어 런타임 오류가 발생할 수 있다.
