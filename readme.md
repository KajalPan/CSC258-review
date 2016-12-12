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

| A    | B    | AND  | OR   | XOR  | NAND |
| :--: | :--: | :--: | :--: | :--: | :--: |
| 0    | 0    | 0    | 0    | 0    | 1    |
| 0    | 1    | 0    | 1    | 1    | 1    |
| 1    | 0    | 0    | 1    | 1    | 1    |
| 1    | 1    | 1    | 1    | 0    | 0    |

#### Creating complex logic
1.  Create truth tables
2.  Express as boolean expression
3.  Convert to Gates

#### Minterms and Maxterms
Not so sure about this table

| Maxterm       | Minterm       | A             | B             | C             |
| :-----------: | :-----------: | :-----------: | :-----------: | :-----------: |
| M<sub>7</sub> | m<sub>0</sub> | 0             | 0             | 0             |
| M<sub>6</sub> | m<sub>1</sub> | 0             | 0             | 1             |
| M<sub>5</sub> | m<sub>2</sub> | 0             | 1             | 0             |
| M<sub>4</sub> | m<sub>3</sub> | 0             | 1             | 1             |
| M<sub>3</sub> | m<sub>4</sub> | 1             | 0             | 0             |
| M<sub>2</sub> | m<sub>5</sub> | 1             | 0             | 1             |
| M<sub>1</sub> | m<sub>6</sub> | 1             | 1             | 0             |
| M<sub>0</sub> | m<sub>7</sub> | 1             | 1             | 1             |

m<sub>x</sub> == M<sub>x</sub>'

##### Minterm
An __AND__ expression with every input present in true or complemented form

###### Valid minterms:
A · B' · C · D, A' · B · C' · D, A · B · C · D

###### Sum-of-Minterms (SOM)
__Union__ of minterm expressions. A way of expressing which inputs cause the output to go high.

##### Maxterm
An __OR__ expression with every input present in true or complemented form

###### Valid minterms:
A + B' + C + D, A' + B + C' + D, A + B + C + D

###### Product-of-Maxterms (POM)
__intersection__ of maxterm expressions.

#### Karnaugh Maps (K-Map)
2D grid of minterms, where adjacent minterm locations in the grid differ by a single literal

|                | C' · D'        | C' · D         | C · D          | C · D'         |
| :------------: | :------------: | :------------: | :------------: | :------------: |
| A' · B'        | m<sub>0</sub>  | m<sub>1</sub>  | m<sub>3</sub>  | m<sub>2</sub>  |
| A' · B         | m<sub>4</sub>  | m<sub>5</sub>  | m<sub>7</sub>  | m<sub>6</sub>  |
| A · B          | m<sub>8</sub>  | m<sub>9</sub>  | m<sub>11</sub> | m<sub>10</sub> |
| A · B'         | m<sub>12</sub> | m<sub>13</sub> | m<sub>15</sub> | m<sub>14</sub> |

Once maps are created, draw boxes over groups of high output values.
-   Boxes bust be rectangular, and aligned with map.
-   Number of values contained within each box must be power of 2.
-   Boxes may overlap with each other
-   Boxes may wrap across edges of map

## Logic Devices
Combinational Circuits: Multiplexers (MUX), decoders, Adders, Subtractors, Comparators. Any circuits where the outputs rely strictly on the inputs. Another category is _sequencial cirtuits_ :(.

### Multiplexer
2-to-1 MUX: S = 0, M => X; S = 1, M => Y;

| X | Y | S | M |
| : | : | : | : |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 |

4-to-1 MUX:

| _s<sub>1</sub>, s<sub>0</sub>_ | m                              |
| :----------------------------: | :----------------------------: |
| 00                             | u                              |
| 01                             | v                              |
| 10                             | w                              |
| 11                             | x                              |

Verilog code:

```varilog
module mux_logic( select, d, q );

input[1:0]  select;
input[3:0]  d;
output      q;

wire        q;
wire        not_s0, not_s1;
wire[1:0]   select;
wire[3:0]   d;

assign q =  (~select[1] & ~select[0])   & d[0] |
            (~select[1] & select[0])    & d[1] |
            (select[1]  & ~select[0])   & d[2] |
            (select[1]  & select[0])    & d[3] ;

endmodule
```


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
