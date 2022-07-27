## Assignment
---
The **assign** statement represents a continuous assignment, this means that whenever the LHS changes RHS also changes. 
<br>  Eg:  ```assign y = x``` 
<br> Points to be noted: 
- The LHS must be a net type variable, usually a wire.
- The RHS can be a net type variable or a register.
- No number restirction in the number of assign statements.
- The "assign" is usually a behavioral design style and are commonly used for modelling combinational circuits.
<br><br>
## Datatypes in Verilog
---

- **Nets:** 
    - Continously driven
    - Cannot store a value
    - Used to model connections between continous assignments and instatiations(Ref: Assign).
    - Very musch similar to a physical wire.
        - That is it actueally  represents connections between hardware elements.
    - Usually the nets are of 1-bit value unless explicitly declared as vectors.
    - There are various types of net data type:
        - wire, wor, wand, tri, supply0, supply1 etc.
    - "wire" === "tri" -> used when there are multiple drivers driving them and are shorted together.
    - "wor" and "wand" -> Stands for wired or and wired and.
    - "supply0" and "supply1" -> model for power supplys. 


- **Register:**
    - Retains the last value assigned to it.
    - Analogous to usual variables in programming languages.
    - Can be used as actual registers(storage elements) too.
- **Integer**
    - General-purpose register used for manupilating quantities. 
    - Mostly used in situations such as loop counting.
    - **Note:**
        - The integer datatype is by default 32 bit datatype in verilog but if seen that it's usecase is below 32 bits then the integer data type will be synthesised for a lower bit length. 
```
Example: 
Wire[15:0] X,Y;
integer C;
C = X +Y;
//Here the integer C only needs 17 bit size hence it's assigned the same instead of 32 bits while synthesis.
```
- **Real**
    - Used to store floating point numbers.
    - When a real value is assigned to an integer the real number is rounded off to the nearest integer.
    - Example:
    ```
    real pi, e;
    initial 
    begin
        pi = 314.159e-2;
        e = 2.718;
    end
    integer x, y;
    initial
        x = pi; // Gets the value 3
        y = e // Yet again gets the value 3
    ```
- **Time**
    - Simulation is carried out wrt. a logical block called simulation time.
    - Time variables are used to store the simulation time 
    - The system function ```$time``` gives the simulation time.
    - Example:
    ```
    time current_time
    initial
    current_time = $time;
     
    ``` 
- **Vectors**
    - The registers and nets can be declared as vector of multiple bit width.
        - By default the size is set to 1-bit.
    - Example:
    ```
    wire x, y, z; // Single bit wires
    wire [15:0] line; // 16 bit wires: MSB = 15, LSB = 0
    reg result[1:15]; // 16 bit reg: MSB:1, LSB: 15
    ```
    - **Vector Slicing:**
        - Using parts of a vector.
        - Example: 
        ```
        reg [31: 0] data;
        reg [15:0] half_one, half_two;
        reg sum[7:0];
        initial 
        begin
           half_one = data[31:16];
           half_two = data[15:0];
           sum = half_one[15:8] + half_two[15:8]
        end
        // Here we define a 32-bit register that contains 2 16-bit registers.
        // And the summ is the summ of first 8 bits of each halves.
        ```
    - **MultiDimensional Arrays and memories**
        - Declaration methods:
            ```
            reg [31:0] register_bank[15:0]
            integer matrix[7:0][15:0]
            // Here we already know integer to be of 32 bits, therefore this represents the integer array of 8X16 bits.
            ```
        - Example: 
            ```
            reg [15:0] memword[0:1023]
            // Here there are 1024 words each of length 16 bits.
            ```
- **Parameters**
    - Similar to ```#define``` in C/C++.
    - You define the parameter then everywhere it's called the value gets replaced.
    - The advantage of using a parameter is basically to change a value throughout the code by just editing a line.
    - Example:
    ```
    parameter Hi = 25;
    parameter LO = 5;
    parameter up = 2b'10, down = 2b'01;
    //Now wherever HI is called it gets replaced by 25, Lo -> 5, up -> 10 etc. 
    ``` 
## Specifying Constant values in Verilog

- Can be specified in both sized and unsized forms.
- Syntax: ```<size>'<base><value> ```
- Examples:
    ```
    4'b0111 // 4 bit binary number 0111
    8'hAB   // 8 bit hexadecimal number AB or 1010 1011
    12'h8xf // 12 bit hexadecima with don't cares, 1000 xxxx 1111
    25     // Unsized number, default 32 bits = 25 
    ```
- **Note:** Usually integer constants are specified in unsized form.

---




## Supported Data Values and Signal Strength.
---
### Data values

Value level | Represents 
----------- | ----------
0           | Logic 0 state
1           | Logic 1 state
x           | Unknown logic state
z           | High impedence state


**Note:** 
- All unconnected nets are set to "z"
- All register variables are set to "x"

### Signal Strength

Strength | Type 
-------- | ----------
Supply   | Driving
Strong   | Driving
Pull     | Driving
Large    | Storage
Weak     | Driving
Medium   | Storage
Small    | Storage
highz    | High Impedence

**Note:** 
- If two signals of unequal strengths prevail in a wire then the stronger one prevails.
- Particularluy used for mos level circuts. Eg: Dynamic MOS.

---
## Predefined Logic Gates in Verilog


- NOT
    - Eg: not G1(out, in)
- AND 
    - Eg: and G2(out, in1, in2)
- OR
    - Eg: or G3(out, in1, in2)
- XOR
    - Eg: xor G4(out, in1, in2)
- NAND
    - Eg: nand G5(out, in1, in2)
- NOR
    - Eg: nor G6(out, in1, in2)
- XNOR
    - Eg: xnor G9(out, in1, in2)
- BUFIF1
    - Output will be equal to input if control input is 1 else output will be in "z" state
    - Eg: bufif1 G7(out, in, ctrl)
- BUFIF1
    - Output will be equal to input if control input is 0 else output will be in "z" state
    - Eg: bufif0 G8(out, in, ctrl)
- NOTIF0
    - Output will be inverse input if control input is 0 else output will be in "z" state
    - Eg: notif0 G9(out, in, ctrl)   
- NOTIF1
    - Output will be inverse input if control input is 1 else output will be in "z" state
    - Eg: notif1 G3(out, in, ctrl)

- **Restrictions in instantating primitive gates**

    - The output port must always be connected to a net varible like wire.
    - The input can either be a net or a register. 
    - They have single output but multiple inputs.
        - Example: angd gat(y, a, b)
        - y --> o/p, a,b --> i/p
    - When instantating a gate an optional delay may be specified as it is useful while simulation
        - Logic synthesis usually ignores time delays.
        - Eg: and #5 G1(y, a,b)
        - #5 --> delay
        - The delay will not create any changes in synthesis but is useful for simulation
### **Delays and "timescale" directive :**
- While defining delays in components within the a module the unit of time in which the delay occurs needs to be specified. This is done by the timescale directive.
- **Syntax :**
    - ``` timescale <reference time unit> / <time precision>```
- Valid timescale units are:
    - s -> seconds, ms -> milliseconds, us -> microsecond, ps -> picosecond, fs -> femtosecond.

- **Note :** The time precision specifies the precision to which the delays are rounded off during a simulation.
- Example:
```
timescale 10ns/1ns
module perxor(y, a, b)
    input a, b;
    output y;
    wire t1, t2, t3;
    nand #5 g1(t1, a, b);
    and #5 g2 (t2, a, t1);
    and #5 g3(t3, b, t1);
    nor #5 g4(y, t2, t3);
endmodule
// here the timescale says that each unit is of 10ns [ie. #5 --> 50ns] and simulation will be done witha precision of 1ns.
```
---
# Inbetween stuff - written in notebook

---
