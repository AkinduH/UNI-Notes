

| Row | X   | Y   | Z   | F   | Minterm |          | Maxterm<br> |             |
| --- | --- | --- | --- | --- | ------- | -------- | ----------- | ----------- |
| 0   | 0   | 0   | 0   | 1   | m0      | X’.Y’.Z’ | M0          | X+Y+Z       |
| 1   | 0   | 0   | 1   | 0   | m1      | X’.Y’.Z  | M1          | X+Y+Z'      |
| 2   | 0   | 1   | 0   | 0   | m2      | X’.Y.Z’  | M2          | X+Y’+Z<br>  |
| 3   | 0   | 1   | 1   | 1   | m3      | X’.Y.Z   | M3          | X+Y’+Z’<br> |
| 4   | 1   | 0   | 0   | 1   | m4      | X.Y’.Z’  | M4          | X’+Y+Z<br>  |
| 5   | 1   | 0   | 1   | 0   | m5      | X.Y’.Z   | M5          | X’+Y+Z’     |
| 6   | 1   | 1   | 0   | 1   | m6      | X.Y.Z’   | M6          | X’+Y’+Z     |
| 7   | 1   | 1   | 1   | 1   | m7      | X.Y.Z    | M7          | X’+Y’+Z’    |

##### Canonical sum 
    Sum of the minterms corresponding to the truth table rows for which the output is 1. ΣX,Y,Zm(0,3,4,6,7) = X’.Y’.Z’ + X’.Y.Z + X.Y’.Z’ + X.Y.Z’ + X.Y.Z

##### Minterm list/on-set 
    ΣX,Y,Zm(0,3,4,6,7) 

##### Canonical product 
    Product of maxterms corresponding to the truth table rows for which the output is 0. ⲠX,Y,ZM(1,2,5) = (X+Y+Z’).(X+Y’+Z).(X’+Y+Z’)

##### Maxterm list/off-set
    ⲠX,Y,ZM(1,2,5)


#### Sum of Products

Converting a SoP into a canonical sum of minterms
    SoP (non-canonical): AB + ABC + CB
    
	Canonical sum of minterms:
	AB.(C+C') + ABC + (A+A')CB
	= ABC + ABC' + ABC + ACB + A'CB
	Since X + X = X
	ABC + ABC' + ABC + ACB + A'CB

	ABC + ABC' + A'CB


#### Product of Sums

Converting a PoS into a canonical product of maxterms
	PoS: (A+B).A
	
	Canonical PoS:
	Use A = A+0 and B.B’=0
	(A+B). (A+0) 
	= (A+B).(A+B.B’)
	= (A+B).(A+B).(A+B’)
	=(A+B).(A+B’)


#### Duality of canonical forms

A function expressed as an SoP can also be expressed as a PoS

ΣX,Y,Zm(0,3,4,6,7) = ⲠX,Y,ZM(1,2,5)


#### Canonical forms and De Morgan’s law

A sum of minterms equals the inverse of the product of the corresponding 
maxterms

![[Pasted image 20240302153253.png]]

E.g.  M0’ = (A+B+C)’ = A’B’C’ = m0


[[Implementing boolean logic circuits]]