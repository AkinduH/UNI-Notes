 
#### Decoders

A circuit that converts binary information from n input lines to a maximum of m = 2^n unique output lines.
	Can also be less than 2^n unique output lines


![[Pasted image 20240316191330.png]]

Y0 = EN.I1’.I0’
Y1 = EN.I1’.I0
Y2 = EN.I1.I0’
Y3 = EN.I1.I0

![[Pasted image 20240316191442.png]]

For large decoders, this requires multiple-input AND gates, which is not always feasible
Better to use a hierarchical approach
	Build a larger decoder using a few smaller decoders


![[Pasted image 20240316192707.png]]


Implementing logic functions using a decoder

![[Pasted image 20240316192803.png]]

solution

![[Pasted image 20240316192825.png]]



### Encoders


The opposite of decoding
Convert m-bit input code to an n-bit output code with n ≤ m ≤ 2^n

![[Pasted image 20240316194659.png]]


![[Pasted image 20240316194723.png]]

![[Pasted image 20240316194740.png]]


### Priority encoder

 The priority function resolves the conflict when more than one input is given a value of `1`. In such cases, the priority encoder will only consider the input with the highest priority.

![[Pasted image 20240316194921.png]]

![[Pasted image 20240316195044.png]]


If multiple inputs are active, for example, if the input is `00000101`, the output will be `010` because the input line with priority `2` is active, and it has a higher priority than the input line with priority `0`.


### Multiplexers

The most basic type of multiplexer device is that of a one-way rotary switch as shown.

![[Pasted image 20240316204738.png]]


#### 2-input Multiplexer Design

![[Pasted image 20240316204815.png]]


#### 4-to-1 Channel Multiplexer


![[Pasted image 20240316204842.png]]


#### 4-to-1 Multiplexer using 2-to-1 Multiplexers

![[Pasted image 20240316210238.jpg]]


### Digital Buffers

#### Single input digital buffer

![[Pasted image 20240316214034.png]]

Why do we need this?

To isolate logic gates from each other.
Drive or switch higher than normal loads from a logic gate output.
High fan-out capability


#### Three state (Tri-state) buffer

There are four types of tri-state buffers:
1. Active High tri-state buffer
2. Active high inverting tri-state buffer
3. Active Low tri-state buffer
4. Active low inverting tri-state buffer

![[Pasted image 20240316214412.png]]


![[Pasted image 20240316214427.png]]



### Comparators

Types of comparators
1. Equality comparator
	a. Single output
	b. HIGH if A = B. LOW if not
2. Magnitude comparator
	a. Three outputs
		i. A < B
		ii. A = B
		iii. A > B


#### 1-bit magnitude comparator

![[Pasted image 20240316124010.png]]

![[Pasted image 20240316130848.png]]


### Adders

![[Pasted image 20240316131013.png]]

![[Pasted image 20240316131059.png]]

![[Pasted image 20240316131409.png]]


#### Ripple Carry Adder

![[Pasted image 20240316131505.png]]

In a 32-bit ripple carry adder, there are 32 full adders, so the critical path (worst case) delay is 31 * 2(for carry propagation) + 3(for sum) = 65 gate delays.

#### Carry Lookahead Adder (CLA)

Technique to speedup binary addition by computing the carry bits in parallel, 
without waiting for them to propagate from LSB to MSB.
A carry look-ahead adder reduces the propagation delay by introducing more complex hardware.

![[Pasted image 20240316143817.png]]

![[Pasted image 20240316143832.png]]


#### 4-bit Adder-Subtractor

![[Pasted image 20240316153804.png]]


An overflow can only occur when two numbers added are both positive or both negative.

           0111                1111              1011
           0001                1001              1001
        0 1000                1000            1 0100
        
    carry  0111                1111              1011
    in each step


### Arithmetic Logic Unit (ALU)

![[Pasted image 20240316161337.png]]


### Lookup Tables (LUTs)

LUTs are essentially a memory-based approach to compute the output of a logic function, instead 
of using traditional logic gates like AND, OR, and NOT.

![[Pasted image 20240316161651.png]]


[[Sequential Logic]]