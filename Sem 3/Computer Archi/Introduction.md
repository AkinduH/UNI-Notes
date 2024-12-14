
![[Pasted image 20241128125829.png]]

![[Pasted image 20241128140124.png]]

![[Pasted image 20241128143056.png]]

### **Programming Language Levels Overview**

1. **Machine Code (1940s-1950s)**
    - Written in binary (0s and 1s).
    - Example: `0001000000111000`
    - Directly executed by the CPU.
    
2. **Hexadecimal Notation (1950s-1960s)**
    - Human-readable representation of binary using hexadecimal digits.
    - Example: `1038 1234
    
3. **Assembly Language (1960s-1970s)**
    - Symbolic representation of machine code.
    - Uses mnemonics and labels for easier understanding.
    - Example:
```assembly
        .define const = 6
        num1: .byte [1]
        move.b num1, d0
        addq.b #const, d0
        move.b d0, num2
```
        
4. **High-Level Languages (1970s-1980s)**
    - Abstract, closer to human language, and easier to code.
    - Example:
        ```c
        #define const 6
        int num1, num2;
        num2 = num1 + const;
        ```
        
#### Why Assembly is Still Useful
- Can produce code that runs fast
- Better use of CPU resources
- Only way to use some advanced features
---
![[Pasted image 20241128144548.jpg]]

## **ISA: Instruction Set Architecture**

**Layer of abstraction between hardware and software

RISC –V Registers
![[Figure_4_Understanding_basics_risc-v.webp]]

