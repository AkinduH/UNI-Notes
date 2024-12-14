
| Example             | Explanation                                                                             |
| ------------------- | --------------------------------------------------------------------------------------- |
| `add a3, a2, a1`    | Adds the contents of registers `a1` and `a2` and stores the result in `a3`.             |
| `sub t0, t1, t2`    | Subtracts the contents of register `t2` from `t1` and stores the result in `t0`.        |
| `mul t3, t4, t5`    | Multiplies the contents of registers `t4` and `t5` and stores the result in `t3`.       |
| `div t6, t7`        | Divides the contents of register `t6` by `t7`, quotient in LO, remainder in HI.         |
| `lw s0, 4(s1)`      | Loads a word from the address in register `s1` plus offset `4` into register `s0`.      |
| `sw s2, 8(s3)`      | Stores the contents of register `s2` into the address in register `s3` plus offset `8`. |
| `beq s4, s5, label` | Branches to `label` if the contents of registers `s4` = `s5`                            |
| `bne s6, s7, label` | Branches to `label` if the contents of registers `s6` and `s7` are not equal.           |
| `j target`          | Jumps to the specified `target` address.                                                |
| `jal subroutine`    | Jumps to the `subroutine` address and stores the return address in the link register.   |
| `jr ra`             | Jumps to the address stored in the register `ra` (return address).                      |
| `li t0, 100`        | Loads the immediate value `100` into register `t0`.                                     |
| `blt a1, a2, label` | Branches to `label` if the contents of register `a1` < `a2`.                            |
| `slli t0, t1, 2`    | Shifts the contents of register `t1` left by 2 bits and stores the result in `t0`.      |
| `bge s1, s2, label` | Branches to `label` if the contents of register `s1` >= `s2`.                           |
| `blez s3, label`    | Branches to `label` if the contents of register `s3` are less than or equal to zero.    |
| `bnez s4, label`    | Branches to `label` if the contents of register `s4` are not equal to zero.             |


