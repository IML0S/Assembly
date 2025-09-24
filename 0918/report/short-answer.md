## 문제 1  
1. In 32-bit mode, aside from the stack pointer (ESP), what other register points to variables on the stack?  
32비트에서 ESP가 아닌 스택의 변수를 가리키는 포인터는 EBP이다. EBP는 함수 호출 시 지역 변수와 매개변수를 참조할 때 사용된다.

## 문제 2  
2. Name at least four CPU status flags.  
CPU 상태 플래그는 CF, ZF, SF, OF, PF, AC가 있다.

## 문제 3  
3. Which flag is set when the result of an unsigned arithmetic operation is too large to fit into the destination?  
부호가 없는 연산에서 올림이 생길 때 설정 되는 건 캐리플래그(CF)이다

## 문제 4  
4. Which flag is set when the result of a signed arithmetic operation is either too large or too small to fit into the destination?  
부호가 있는 연산에선 오버플로우 플래그(OF)가 설정된다.

## 문제 5  
5. (True/False): When a register operand size is 32 bits and the REX prefix is used, the R8D register is available for programs to use.  
True

## 문제 6  
6. Which flag is set when an arithmetic or logical operation generates a negative result?  
산술 또는 논리 연산의 결과가 음수일 땐 사인 플래그(SF)가 설정된다.

## 문제 7  
7. Which part of the CPU performs floating-point arithmetic?  
부동소수점 연산은 FPU가 수행한다.

## 문제 8  
8. On a 32-bit processor, how many bits are contained in each floating-point data register?  
32비트에서 각 부동소수점 레지스터는 80비트이며 x87 FPU에선 ST0부터 ST7까지 8개의 80비트 레지스터를 지닌다.

## 문제 9  
9. (True/False): The x86-64 instruction set is backward-compatible with the x86 instruction set.  
True

## 문제 10  
10. (True/False): In current 64-bit chip implementations, all 64 bits are used for addressing.  
False, 대부분 시스템은 48비트 이하로만 사용한다고 한다.

## 문제 11  
11. (True/False): The Itanium instruction set is completely different from the x86 instruction set.  
True

## 문제 12  
12. (True/False): Static RAM is usually less expensive than dynamic RAM.  
False, SRAM은 DRAM에 비해 용량 대비 가격이 비싸고 속도가 빠르다.

## 문제 13  
13. (True/False): The 64-bit RDI register is available when the REX prefix is used.  
True

## 문제 14  
14. (True/False): In native 64-bit mode, you can use 16-bit real mode, but not the virtual-8086 mode.  
True

## 문제 15  
15. (True/False): The x86-64 processors have 4 more general-purpose registers than the x86 processors.  
False, R8~R15까지 8개가 추가된다.

## 문제 16  
16. (True/False): The 64-bit version of Microsoft Windows does not support virtual-8086 mode.  
True

## 문제 17  
17. (True/False): DRAM can only be erased using ultraviolet light.  
False, DRAM은 전기적 방식으로 데이터를 지우며, 주기적으로 리프레쉬된다.

## 문제 18  
18. (True/False): In 64-bit mode, you can use up to eight floating-point registers.  
False, SSE를 사용하면 16개까지 사용가능하다

## 문제 19  
19. (True/False): A bus is a plastic cable that is attached to the motherboard at both ends, but does not sit directly on the motherboard.  
False, 버스는 컴퓨터 내부에서 데이터, 신호, 주소를 전달하는 전기 회로이다.

## 문제 20  
20. (True/False): CMOS RAM is the same as static RAM, meaning that it holds its value without any extra power or refresh cycles.  
False, CMOS는 추가 전력이 필요하다

## 문제 21  
21. (True/False): PCI connectors are used for graphics cards and sound cards.  
True

## 문제 22  
22. (True/False): The 8259A is a controller that handles external interrupts from hardware devices.  
True

## 문제 23  
23. (True/False): The acronym PCI stands for programmable component interface.  
False, PCI는 Peripheral Component Interconnect의 약자이다.

## 문제 24  
24. (True/False): VRAM stands for virtual random access memory.  
False, VRAM은 Video RAM이다.

## 문제 25  
25. At which level(s) can an assembly language program manipulate input/output?  
하드웨어와 운영체제 모두 가능하다

## 문제 26  
26. Why do game programs often send their sound output directly to the sound card’s hardware ports?  
지연을 줄이고 성능을 높이기 위해서 사운드 출력을 하드웨어 포트로 직접 보낸다.
