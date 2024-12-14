
##### Logic Circuits
1. Combinational
2. Sequential


**Combinational Logic Circuits**: 
	These are circuits where the output depends solely on the current inputs. Examples include AND, OR, and NOT gates.

**Sequential Logic Circuits**: 
	Unlike combinational logic circuits, the output in sequential circuits depends on both the current inputs and past outputs. This is because they have a ‘memory’ component that stores previous outputs. Examples include Flip-Flops and Counters.


**State Variables of Digital Circuits**: 
	These are typically binary values (0 or 1). The number of different states a system can be in is finite and is usually represented as,  $$ 2^n $$where n is the number of state variables.

ex :  2-bit binary counter 
				Since there are 2 bits, and each bit can be in one of two states (0 or 1) and 4 states are typically represented as 00, 01, 10, and 11 in binary


Sequential digital circuits are often referred to as finite-state machines (FSMs) because they can only be in a finite number of states.


**Time Keeping by Free-Running Clock**:
	A free-running clock is a clock signal that oscillates between a high state and a low state, typically at a constant frequency. This clock signal is used to time the state changes in the sequential circuit.

**Active High signal** - where the ‘active’ or ‘on’ state is represented by a high voltage level

**Active Low signal** - where the ‘active’ or ‘on’ state is represented by a low voltage level

### Sequential circuit types

**Feedback Sequential Circuits**: 
These circuits use individual logic gates and feedback loops to create memory elements. The feedback loop allows the circuit to retain its state, making it possible to ***create memory devices like latches and flip-flops***.

 **Clocked Synchronous State Machines**: 
 These are a type of sequential circuit where the changes in state of the memory elements are synchronized by a clock signal. ***Latches and flip-flops are used as building blocks in these circuits.*** The clock signal ensures that state transitions occur at predictable times.


**Latches**: 
	A latch is a basic form of a sequential circuit. It’s a bistable multivibrator, meaning it has two stable states. The simplest type of latch is an SR (Set-Reset) latch.


**Flip-Flops**:
	A flip-flop is essentially a latch that is edge-triggered, meaning it only changes state when it receives a control signal (usually a clock pulse). The most common type of flip-flop is the D (Data) flip-flop.


#### S - R Latch

![[Pasted image 20240305031102.png]]

![[Pasted image 20240305101427.png]]

Here’s a simple truth table for the SR Latch:

| S   | R   | Q        | QN        |
| --- | --- | -------- | --------- |
| 0   | 0   | Q (prev) | QN (prev) |
| 0   | 1   | 0        | 1         |
| 1   | 0   | 1        | 0         |
| 1   | 1   | -        | -         |

In the last row (S=1, R=1), the outputs are undefined or unpredictable. This state is generally avoided in practical applications


![[Pasted image 20240305101355.png]]




#### 𝑆 − 𝑅 Latch with Enable

![[Pasted image 20240305101504.png]]

or 

![[Pasted image 20240317125622.png]]


![[Pasted image 20240305101525.png]]

The **SR latch with Enable** (also known as a gated SR latch) adds an additional level of control to the basic SR latch.
 The SR latch with Enable has an additional input, C. When only the Enable input is high (1), the latch responds to the S and R inputs just like a basic SR latch.

| C   | S   | R   | Q        | QN        |
| --- | --- | --- | -------- | --------- |
| 0   | X   | X   | Q (last) | QN (last) |
| 1   | 0   | 0   | Q (last) | QN (last) |
| 1   | 0   | 1   | 0        | 1         |
| 1   | 1   | 0   | 1        | 0         |
| 1   | 1   | 1   | -        | -         |

![[Pasted image 20240305102247.png]]


#### D Latch

![[Pasted image 20240305102220.png]]

![[Pasted image 20240806101632.png]]

![[Pasted image 20240305102454.png]]

The **D latch** (Data latch) is a type of latch which is used to store one bit of information.

| C   | D   | Q        | QN        |
| --- | --- | -------- | --------- |
| 0   | X   | Q (last) | QN (last) |
| 1   | 0   | 0        | 1         |
| 1   | 1   | 1        | 0         |

![[Pasted image 20240305102512.png]]


#### Edge triggered flipflops

An **Edge-Triggered Flip-Flop** is a type of flip-flop that changes its output value only at the edge of the clock signal.

![[Pasted image 20240305103646.png]]


![[Pasted image 20240305103733.png]]


#### Edge triggered D flipflops

![[Pasted image 20240317130113.png]]

 
#### Edge triggered J-K Flipflop

![[Pasted image 20240317130154.png]]


#### T flipflop

T flip – flop is also known as “Toggle Flip – flop”.
The flip – flop acts as a Toggle switch. Toggling means ‘Changing the next state output to complement of the present state output’.


![[Pasted image 20240317140409.jpg]]


| **T** | **Q PREV** | **QN PREV** | **Q NEXT** | **QN NEXT** |
| ----- | ---------- | ----------- | ---------- | ----------- |
| 0     | 0          | 1           | 0          | 1           |
| 0     | 1          | 0           | 1          | 0           |
| 1     | 0          | 1           | 1          | 0           |
| 1     | 1          | 0           | 0          | 1           |


#### Excitation Tables

JK Flip Flop

![[Pasted image 20240317142319.png]]

S-R Flip Flop

![[Pasted image 20240317142401.png]]

D Flip Flop

![[Pasted image 20240317142430.png]]

T Flip Flop

![[Pasted image 20240317142506.png]]



### Counter using J K Flip Flops


![[Pasted image 20240317142619.png]]

![[Pasted image 20240317142637.png]]


![[Pasted image 20240317142648.png]]

[[Computer Organization]]