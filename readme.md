# <img src="img/overview.png" width="40px"/> CSC258 Review
<img src="img/overview.png" width="300px"/>

## Transistors 晶体管
Logic Gates are made from transistors based on pn-junctions made from semiconductors that conduct electricity

### Semiconductors
Si and Ge

#### Impurity (Doping)

##### n-type
Add Phosphorus

##### p-type
Add Boron

#### MOSFET
Metal Oxide Semiconductor Field Effect Transistors

##### nMOS
nMOS conduct when +tive voltage (5V) applied

##### pMOS
pMOS conduct when voltage is logic-zero

### Logic Gates
Are made by a combination of pMOS and nMOS Transistors. pMOS transistors conduct logic-1 values batter, and nMOS transistors conduct logic-0 values batter.

#### Gates Truth Table
| A | B | AND | OR | XOR | NAND |
| : | : | -:- | :- | -:- | :--- |
| 0 | 0 | 0   | 0  | 0   | 1    |
| 0 | 1 | 0   | 1  | 1   | 1    |
| 1 | 0 | 0   | 1  | 1   | 1    |
| 1 | 1 | 1   | 1  | 0   | 0    |
#### Creating complex logic
1.  Create truth tables
2.  Express as boolean expression
3.  Convert to Gates

#### Minterms and Maxterms
##### Minterm
An AND expression with every input present in true or complemented form
m<sub>0</sub>
##### Maxterm
An OR expression with every input present in true or complemented form




## Data path
## Controller
## Assembly Language
- Fibonacci.asm

    ```nasm
    .text
    FIB:	addi $t3, $zero, 10	# initialize n=10
            addi $t4, $zero, 1	# initialize f1=1
            addi $t5, $zero, -1	# initialize f2=-1
    LOOP:	beq $t3, $zero, END	# done loop if n==0
            add $t4, $t4, $t5	# f1 = f1 + f2
            sub $t5, $t4, $t5	# f2 = f1 - f2
            addi $t3, $t3, -1	# n = n – 1
            j LOOP			# repeat until done
    #END:	sb $t4, RES		# store result (we'll talk about this next week)
    END: 	j END			# infinite loop. just for demonstration!

    ```
