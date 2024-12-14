
1. **Computer Design**: This is the process of creating a new computer, including all the steps from conceptualization to the final product. It involves making decisions based on various constraints like circuit-level delays, Silicon real estate, heat generation, and cost. For example, the Intel Core i7-6800K and the Xeon E5-2643 v4 are two different designs made by the same company, Intel.
    
2. **Computer Organization**: This refers to the operational structure of a computer system, including the internal details of operational units, their interconnection, and control. It’s the view of a computer designer and involves decisions like how to support multiplication – whether to use a multiply circuit or repeated addition. For instance, Intel and AMD both support x86 but with different organizations, meaning they have different ways of implementing the same architecture.
    
3. **Computer Architecture**: This is the blueprint or plan that is visible to the programmer. It involves the key functional units, their interconnection, and the instructions to program them. This is often referred to as the Instruction Set Architecture (ISA). An example of this would be the differences between the x86 and ARM architectures. These are two different architectures supported by different types of processors.


Computer Organization refers to the operational structure of a computer system and how different components interact to perform the overall function of the system.

Organization of a Computer is defined by:
	• Internal Registers
	• Timing and control structures
	• Set of Instructions
Internal Organization of Digital System
	• Sequence of micro-operations performed on data stored in registers
	• Sequence is instructed by user as program


### Blocks of a Microprocessor 

![[Pasted image 20240501230952.jpg]]

- **Program Execution Section**: This part of the microprocessor is responsible for executing the instructions of a program. It includes components like the Program Counter, which keeps track of the program sequence, and the Instruction Register, which holds the current instruction being executed. The Instruction Decoder translates instructions into signals that control other parts of the microprocessor.

- **Register Processing Section**: This section deals with the processing of data within the microprocessor. It includes the Accumulator, which temporarily stores data during processing, and the Arithmetic Logic Unit (ALU), which performs arithmetic and logical operations. The FLAG & Special Function Registers store the status of operations and special instructions, while the RAM & Data Registers are used for general data storage.

![[Pasted image 20240501231040.jpg]]This type of register allows for fast data transfer because all bits are loaded and retrieved in parallel, which is useful in situations where speed is crucial.

![[Pasted image 20240501231131.jpg]]
In operation, data is shifted in one bit at a time with each clock pulse. When the first bit enters stage A, the next clock pulse shifts it to stage B, and the next bit enters stage A. This process continues until all bits have been shifted through the register.

![[Pasted image 20240501231817.jpg]]

- **Problem Statement**: Identifying the computational problem that needs to be solved.
- **Behavioral Description**: Using algorithms, flowcharts, and other tools to describe the behavior of the solution.
- **Boolean Logic and State**: Converting the behavioral description into binary form using Boolean algebra.
- **Device Selection and Wiring**: Choosing the appropriate hardware components and connecting them.
- **Hardware Implementation**: Actual construction of the digital solution using TTL gates, programmable logic, custom ASICs, FPGAs, microcontroller systems (MCS), and digital signal processors (DSPs).


[[Memory Organization]]