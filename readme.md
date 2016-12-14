# <img src="img/overview.png" width="40px"/> CSC258 Review
<img src="img/overview.png" width="300px"/>

## Transistors
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
### Combinational Circuits
Multiplexers (MUX), decoders, Adders, Subtractors, Comparators. Any circuits where the outputs rely strictly on the inputs. Another category is _sequencial cirtuits_ :(.

#### Multiplexer
2-to-1 MUX: S = 0, M => X; S = 1, M => Y;

|  X  |  Y  |  S  |  M  |
| :-: | :-: | :-: | :-: |
|  0  |  0  |  0  |  0  |
|  0  |  0  |  1  |  0  |
|  0  |  1  |  0  |  0  |
|  0  |  1  |  1  |  1  |
|  1  |  0  |  0  |  1  |
|  1  |  0  |  1  |  0  |
|  1  |  1  |  0  |  1  |
|  1  |  1  |  1  |  1  |

4-to-1 MUX:

| _s<sub>1</sub>, s<sub>0</sub>_ | m                              |
| :----------------------------: | :----------------------------: |
| 00                             | u                              |
| 01                             | v                              |
| 10                             | w                              |
| 11                             | x                              |

Verilog code:

```verilog
module mux_logic( select, d, q );

input[1:0]  select;
input[3:0]  d;
output      q;

wire        q;
wire[1:0]   select;
wire[3:0]   d;

assign q =  (~select[1] & ~select[0])   & d[0] |
            (~select[1] & select[0])    & d[1] |
            (select[1]  & ~select[0])   & d[2] |
            (select[1]  & select[0])    & d[3] ;

endmodule
```

#### decoders
Translate from the output of one circuit to input of another.

##### 7-segment decoder
-   Translate from a 4-digit binary number to 7 segments of a digital display
-   each output segment has a particular logic

#### Adders
AKA Binary Adders
-   Small circuit devices that add two digits together

##### Half Adders
Adds two bits to produce a two-bit sum, as a sum bit and a carry bit.

```
C = X · Y

S = X · Y' + X' · Y
  = X ⊕ Y
```

##### Full Adders
Similar to Half adders, but with another input `Z`, which represents a carry-in bit (C and Z usually labeled as C<sub>out</sub> and C<sub>in</sub>)

```
C = X · Y + X · Z + Y · Z

S = X ⊕ Y ⊕ Z
```

##### Ripple-Carry Binary Adder
Full adder units chained together

#### Subtractors
1.  Take a smaller number, and invert all the digits
2.  Add inverted number to the larger one
3.  Add one to the result

(2's complement)

#### Signed numbers
##### Sign and Magnitude
Sign: A separate bit for the sign, __0 for +, 1 for -__.

Magnitude： Remaining bits store unsigned part of the number.

e.g. 0110 if 6 while 1110 is -6


##### 2's complement
###### 1's complement
(2<sup>n</sup>-1)-x, negate each individual bit.
###### 2's complement is 1's complement + 1
(Adding a -tive umber in 2's complement notation to the same positive number reduces a result of 0)

circuit implementation:
![Subtractor](img/subtractor.png)

##### Addition/Subtraction circuit
![add_sub](img/add_sub.png)
`A XOR 0` is `A`, `A XOR 1` is `!A`. Therefore if `Sub` is high, `1` is `Cin` and input of `Y` is bitwise flipped.

#### Comparators
Bitwise comparison. Most significant bit domintes.

Verilog implementation:

```Verilog
module comparator_4_bit (a_gt_b, a_lt_b, a_eq_b, a, b);

input [3:0] a, b;
output a_gt_b, a_lt_b, a_eq_b;

assign a_gt_b = (a > b);
assign a_lt_b = (a < b);
assign a_eq_b = (a == b);

endmodule
```




### Sequential Circuits
Internal state can change over time, same input value can result different outputs

NAND, NOR gates with feedback have more interesting characteristics, which make them to storage devices.

Feedback circuit example:
![feedback_cir](img/feedback_cir.png)

NAND Behavior

| A                  | Q<sub>T</sub>      | Q<sub>T + 1</sub>  |
| :----------------: | :----------------: | :----------------: |
| 0                  | 0                  | 1                  |
| 0                  | 1                  | 1                  |
| 1                  | 0                  | 1                  |
| 1                  | 1                  | 0                  |

NOR Behavior

| A                  | Q<sub>T</sub>      | Q<sub>T + 1</sub>  |
| :----------------: | :----------------: | :----------------: |
| 0                  | 0                  | 1                  |
| 0                  | 1                  | 0                  |
| 1                  | 0                  | 0                  |
| 1                  | 1                  | 0                  |

Storage could enter unsteady states.

#### Latches
Latches are combination of multiple gates of these types.
![latches](img/latches.png)

##### `S'R'` latch
![S'R'latch](img/S'R'_latch.png)
-   `S'` and `R'` are called "set" and "reset" respectively
-   Circuits "remembers" its signal when going from `10` or `01` to `11`.
-   Going from `00` to `11` produces unstable behavior, depending on which input changes first
-   __00__ is forbidden state

##### `SR` latch
![SRlatch](img/SR_latch.png)
-   `S` and `R` are called "set" and "reset" respectively
-   Circuits "remembers" previous output when going from `10` or `01` to `00`
-   Unstable behavior possible when inputs go from `11` to `00`
-   __11__ is forbidden state

###### Timing diagram
```
S   ▁▁▁▁▔▔▔▔▔▔▔▔▁▁▁▁▁▁▁▁▁▁▁▁
R   ▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▔▔▔▔▔▔▔▔
Q   ▁▁▁▁▁▁▁▁▔▔▔▔▔▔▔▔▔▔▁▁▁▁▁▁
Q'  ▔▔▔▔▔▔▁▁▁▁▁▁▁▁▁▁▁▁▁▁▔▔▔▔
```

Notation:
-   T<sub>PLH</sub>: Propagation time when output goes from Low to High
-   T<sub>PHL</sub>: Propagation time when output goes from High to Low

#### Clock
-   Timing signal to let circuit know when the output may be sampled
-   Periodic regular signal
-   Usually drawn as
```
    ┃ Voltage 5V
    ┃▁▔▁▔▁▔▁▔▁▔▁▔▁▔▁▔▁▔▁▔
    ┃ Voltage 0V
    ┗━━━━━━━━━━━━━━━━━━━━━> time
```

- frequency: How many pulses occur per second, measured in Hertz (Hz)

##### Clocked SR Latch
![clocked_SR_latch](img/clocked_SR_latch.png)

-   By adding another layer of NAND gates to the S'R' latch, we get a SR latch with control input
-   Input C is often connected to a pulse signal that alternates regularly between 0 and 1 (clock)
-   Behavior
    -   Clock need to be high in order for the inputs to take any Effect

Symbol
```
  ┏━━━━━━━┓
━━┫ S     ┣━━ Q
━━┫ C     ┃
━━┫ R     ┣○━ Q'
  ┗━━━━━━━┛
```
The NOT circil is not an extra NOT gate. We got this inversion for free.

##### D latch
![D_latch](img/D_latch.png)
```
  ┏━━━━━━━┓
━━┫ D     ┣━━ Q
━━┫ C     ┃
  ┃       ┣○━ Q'
  ┗━━━━━━━┛
```
-   By making `R` and `S` independent on a signal `D`, avoid the indeterminate state problem
-   Value of `D` sets output `Q` low or high when `C` is high
-   Timing issue
    -   Any changes to its inputs are visible to the output when control signal (clock) is 1
    -   output of a latch ___should not___ be applied directly or through combinational logic to the input of the same or another latch when they all have the same control signal

#### Flip-Flops
Triggered only on a edge of the clock

##### D Flip-Flop
###### Master-slave
A D-latch (master) is connected to an SR-latch (slave) with complementary control signals.
```
Negedge-triggered DFF
      ┏━━━━━━━┓          ┏━━━━━━━┓
D ━━━━┫ D   Q ┣━━━━━━━━━━┫ S1  Q ┣━━ Q
C ━━┳━┫ C     ┃    ┏━━━━━┫ C     ┃
    ┃ ┃     Q'┣○━━━╋━━━━━┫ R1  Q'┣○━ Q'
    ┃ ┗━━━━━━━┛    ┃     ┗━━━━━━━┛
    ┗━━━▷○━━━━━━━━━┛
```

```
Posedge-triggered DFF
       ┏━━━━━━━┓          ┏━━━━━━━┓
D ━━━━━┫ D   Q ┣━━━━━━━━━━┫ S1    ┣━━ Q
C ━▷○┳━┫ C     ┃    ┏━━━━━┫ C     ┃
     ┃ ┃     Q'┣○━━━╋━━━━━┫ R1    ┣○━ Q'
     ┃ ┗━━━━━━━┛    ┃     ┗━━━━━━━┛
     ┗━━━▷○━━━━━━━━━┛
```
Most commonly-used flip-flop

##### Other Flip-Flops
###### T Flip-Flop
Toggles value whenever input T is high
-   If `T` is `1` @ posedge, output `Q` will toggle
-   If `T` is `0` @ negedge, output `Q` holds on prior state
###### JK Flip-Flop
-   If `J` and `K` are  `0`, maintain output
-   If `J` is `0` and `K` is `1`, set output to `0`
-   If `J` is `1` and `K` is `0`, set output to `1`
-   If `J` and `K` are `1`, toggle output value

##### Timing signal restrictions
-   Setup time: input should be stable for some time before active clock edge
-   Hold time: input should be stable for some time immediately after the active clock edge
-   Time period between two active clock edges cannot be shorter than __longest propagation delay__ between any two flip-flops + __setup time__ of the flip-flop

##### Reset inputs
-   A way to initialize flip-flop states
-   reset signal resets FF output to 0 (unrelated to R of SE latch)
-   Synchronous reset: only on posedge
-   Asynchronous reset: output is set to 0 immediately, independent of the clock signal
-   often also called __Clear__

### Sequential Circuit Design
#### Register
Allow storage of information (CPU has many physical registers)
##### Shift registers
![shift_reg](img/shift_reg.png)
A series of DFF can store a multi-bit value (e.g a 16-bit int)
-   Data can be shifted into this register one bit at a time, over 16 clock cycles

##### Load Registers
![load_reg](img/load_reg.png)


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
