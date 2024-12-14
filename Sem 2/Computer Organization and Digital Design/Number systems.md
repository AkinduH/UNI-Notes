

1. **Binary to Decimal**:
    
    - Binary: `1011`
    - Decimal:
        
        1*2^3 + 0*2^2 + 1*2^1 + 1*2^0 = 8 + 0 + 2 + 1 = 111∗23+0∗22+1∗21+1∗20=8+0+2+1=11
        
2. **Decimal to Binary**:
    
    - Decimal: `13`
    - Binary: `1101` (13 divided by 2 gives 6 remainder 1, 6 divided by 2 gives 3 remainder 0, 3 divided by 2 gives 1 remainder 1, and finally 1 divided by 2 gives 0 remainder 1. Reading the remainders from bottom to top gives `1101`.)
3. **Binary to Octal**:
    
    - Binary: `1101011`
    - Octal: `153` (Grouping from right `011`, `101`, `1` gives `3`, `5`, `1`.)
4. **Octal to Binary**:
    
    - Octal: `745`
    - Binary: `111010101` (Each octal digit `7`, `4`, `5` converted to binary gives `111`, `100`, `101`.)
5. **Binary to Hexadecimal**:
    
    - Binary: `11010111`
    - Hexadecimal: `D7` (Grouping from right `0111`, `1011` gives `7`, `D`.)
6. **Hexadecimal to Binary**:
    
    - Hexadecimal: `3F`
    - Binary: `111111` (Each hexadecimal digit `3`, `F` converted to binary gives `0011`, `1111`.)
7. **Decimal to Octal**:
    
    - Decimal: `63`
    - Octal: `77` (63 divided by 8 gives 7 remainder 7. Reading the remainders from bottom to top gives `77`.)
8. **Octal to Decimal**:
    
    - Octal: `77`
    - Decimal:
        
        7*8^1 + 7*8^0 = 56 + 7 = 637∗81+7∗80=56+7=63
        
9. **Decimal to Hexadecimal**:
    
    - Decimal: `255`
    - Hexadecimal: `FF` (255 divided by 16 gives 15 remainder 15. Reading the remainders from bottom to top gives `FF`.)
10. **Hexadecimal to Decimal**:
    
    - Hexadecimal: `B4`
    - Decimal:
        
        11*16^1 + 4*16^0 = 176 + 4 = 18011∗161+4∗160=176+4=180


When dealing with decimal points in binary, octal, or hexadecimal numbers, the process is similar to the integer part but with a twist. Here’s how you can handle it:

1. **Binary/ Octal/ Hexadecimal to Decimal**:
    
    - For the fractional part, start from the first digit after the decimal point and multiply each digit by
        
$$
        1/2^n​
$$
        
        ,
        
$$
        1/8^n
$$
        
        , or
        
$$
        1/16^n
$$
        
        respectively, where n is the position of the digit, starting from 1. Sum these values to get the decimal equivalent of the fractional part.
        
    - Example: Binary `101.101` to Decimal
        
        - Integer part:
            
            1∗22+0∗21+1∗20=4+0+1=5
            
        - Fractional part:
            
            1∗1/2​+0∗1/4​+1∗1/8=0.5+0+0.125=0.625
            
        - So, `101.101` in binary is equivalent to `5.625` in decimal.
2. **Decimal to Binary/ Octal/ Hexadecimal**:

    - For the fractional part, multiply the fraction by 2, 8, or 16 respectively. The integer part of the result is the first digit after the decimal point. Repeat this process with the fractional part of the result until you get a fraction of 0 or until you have a sufficient number of digits.
        
    - Example: Decimal `0.375` to Binary
        
        - `0.375 * 2 = 0.75`, first digit is `0`.
        - `0.75 * 2 = 1.5`, second digit is `1`.
        - `0.5 * 2 = 1.0`, third digit is `1`.
        - So, `0.375` in decimal is equivalent to `.011` in binary.




### Negative binary numbers



#### 1.**Sign-and-Magnitude**: 

The leftmost bit is used as the sign bit: `0` for positive and `1` for negative. The rest of the bits represent the magnitude of the number.

This has two representations for zero (`+0` and `-0`).

For example, `-7` in an 8-bit sign-and-magnitude format would be `10000111`.
		Range  --->  - (2^(n-1) - 1) to +(2^(n-1)  - 1)



#### 2.**One’s Complement**: 

Negative numbers are represented by inverting all the bits of their positive counterparts. 

This has two representations for zero (`+0` and `-0`).
This requires end-around carry for addition.(In addition carry bit needs to be added)

For example, `-7` in an 8-bit one’s complement format would be `11111000`.
		Range  --->  - (2^(n-1) - 1) to +(2^(n-1)  - 1)



#### 3.**Two’s Complement**: 

Negative numbers are represented by inverting all the bits of their positive counterparts and then adding 1 to the result. This is the most common method used in computers because it simplifies the addition and subtraction operations. 

This only has one representation for zero.
This doesn’t require end-around carry

For example, `-7` in an 8-bit two’s complement format would be `11111001`.
		Range  --->  - (2^(n-1)) to +(2^(n-1)  - 1)



### **Binary Coded Decimal (BCD) and Gray Codes**


![[Pasted image 20240805181943.png]]


Binary to Grey code

1. Let the binary number be \( B_n B_{n-1} ...... B_1 B_0 \).
2. The Gray code \( G \) is given by:
   - \( G_n = B_n \ (The MSB of the Gray code is the same as the MSB of the binary number)
   - \( G_{i} = B_i XOR B_{i+1} \) for \( i = n-1 \) to \( 0 \)


1001  ----->  1101


Grey code to Binary

1. Let the Gray code be \( G_n G_{n-1} ....... G_1 G_0 \).
2. The binary number \( B \) is given by:
   - \( B_n = G_n \ (The MSB of the binary number is the same as the MSB of the Gray code)
   - \( B_i = B_{i+1} XOR G_i \) for \( i = n-1 \) to \( 0 \)

0100  --------> 0111

[[Logic Circuit Representation And Simplification]]


