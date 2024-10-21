![image](https://github.com/user-attachments/assets/3a21c561-748b-47cf-b49f-2cf1dd52af53)# Installation of Required tools
Use the following commands to install yosys, iverilog and gtkwave

```
   $ git clone https://github.com/YosysHQ/yosys.git
    $ cd yosys
    $ sudo apt install make (If make is not installed) 
    $ sudo apt-get install build-essential clang bison flex \
        libreadline-dev gawk tcl-dev libffi-dev git \
        graphviz xdot pkg-config python3 libboost-system-dev \
        libboost-python-dev libboost-filesystem-dev zlib1g-dev
    $ make config-gcc
    $ make 
    $ sudo make install
```

![Screenshot from 2024-10-21 19-29-07](https://github.com/user-attachments/assets/8360cb64-327b-4c0d-a6fe-efb4f022a1f7)
 ### To install iverilog:
 ` sudo apt-get install iverilog `

 ### To install gtkwave:
    ``` 
    sudo apt update
    sudo apt install gtkwave 
    ```

![Screenshot from 2024-10-21 19-31-10](https://github.com/user-attachments/assets/9e206e18-d0b1-46eb-bcae-824f434e2e46)

# Day 1
### Simulator:
-Simulator is tool to verify the RTL design by using testbech. iverilog is one of the simulation tool.

-Design is the actual verilog code which has the intended functionality to meet with the required specifications.
-Testbench is the setup to apply stimulus.
iverilog based simualtion flow:
The iverilog simualator takes the design file and the testbench as input and generates a VCD(Value chae dump format) file. The simulaton output can be viewed using gtkwave analyser.
![image](https://github.com/user-attachments/assets/8fc95a48-dcf5-4ed8-8ce0-870e9b37f659)
## VSD Flow

```
mkdie VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
```

To load a design file:
```
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
![image](https://github.com/user-attachments/assets/4444f832-84f0-493c-bf5c-3b1334f5656e)
To see the file structure :
` gvim tb_good_mux.v -o good_mux.v `
![image](https://github.com/user-attachments/assets/a7e20d02-42cc-441d-9c7a-742fa6bcc0a6)

## Yosys
Synthesizer is the tool for converting RTL design to netlist and YOSYS is the tool we are using as for synthesis.
Netlist is the representation of the design in the form of cells present in the design.

![image](https://github.com/user-attachments/assets/900a9cb6-6b9d-4653-aa40-4184c3f67907)

To verify the synthesis output: 
Give the netlist as input the iverilog simulator along with the same testbench and view the output VCD file using gtkwave analyser. This output should be same as the output obtained from RTL design file.
The set of primary inputs and outputs remain same for both RTL design and netlist simulation. Hence the same testbench is used in both cases.
![image](https://github.com/user-attachments/assets/7e5b85e5-f6d8-4350-aa99-2f1ebb639382)

## Logic Synthesis
### RTL Design 
It is the behavioural representaion of the requiered specification.
RTL to gate level transition is called synthesis.
the design is converted into gates and connections are made.
Netlist is the output of the synthesis.
### .lib:
It is the collection of logic modules.
It includes basic logic gates like AND, OR, NOT, etc.
It also includes different flavours of same gate, like 2 input AND gate, 3 input AND gate.
## Labs using Yosys

List of Commands in yosys:
<br>

| Task  | Command |
| :-----:	| :-----: | 
| To invoke Yosys | yosys |
| To read librery | read_liberty -lib /home/bhavana/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib |
| To read design | read_verilog good_mux.v |
| To synthesis | synth top good_mux.v |
| To generate netlist | abc -liberty ../path |
| To see realized logic | show |
| To write netlist | write_verilog -noattr good_mux_netlist.v <br> !gvim good_mux_netlist.v |

<br>

![image](https://github.com/user-attachments/assets/3fa82c63-7513-43fe-90dd-97a184d9cf14)

<br>

![image](https://github.com/user-attachments/assets/aa65991f-490d-44d4-8e6c-150e1de454ed)


Writing netlist:
<br> 
![image](https://github.com/user-attachments/assets/338a9ea1-d384-4318-b16f-24276ccd382d)

# Day 2

Open lib File
` gvim /home/bhavana/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib `

<br>

Details in the lib file:
<br>

![image](https://github.com/user-attachments/assets/2873a7ba-f6e3-4452-bb89-6d7e55e6f9a4)

<br>
Different features of each cell:

<br>

![image](https://github.com/user-attachments/assets/cffca201-60db-410a-8e63-61337cf7c3c5)

<br>

Comparing details of same cell but different size:

<br>

Smaller cell has smaller area but high delay compared to larger cell which are faster and consume more area.
![image](https://github.com/user-attachments/assets/c0ecaf2a-0bae-4b42-92c8-73935782bbb6)

<br>

## Heirarchical and Flat Synthesis

### Heirarchical Design

1. Read the multiple_modules.v file
2. ` synth -top multiple_modules `
3. ` show multiple_modules `
![image](https://github.com/user-attachments/assets/0ca6fb1f-8790-4362-8a80-ff47180bfafd)

![image](https://github.com/user-attachments/assets/0334dee4-cecb-4357-9644-f2910afc2cc6)

<br> 
Here it can be seen that heirarchies are reserved. The design containes two submodules.
![image](https://github.com/user-attachments/assets/47372e48-f239-442d-af6a-1f05ebc29387)

### Flat Synthesis
` flatten ` is the command to flat out the netlist.

### To synthesis a submodule in a multiple_modules file
```
read_verilog multiple_modules.v
synth -top sub_module_1
abc -liberty ../path_of_.lib
show
```
![image](https://github.com/user-attachments/assets/a83643e6-4fd2-4bf0-9d9e-6c6e676e113c)
<br>
Module leve synthesis is preferred when
- we need multiple instances of same module.
- Divide and Conquer - need for synthesising part by part.

## Flops
Flops are needed to avoid propogation of gliches.

### Types of coding Flops:
1. Asynchronous Flops: There is no clock dependency. Asynchronous flip-flops can change their output state immediately in response to input changes. They do not have setup or hold time requirements related to a clock, leading to potential timing issues. They are often used in situations where immediate response to input changes is necessary, such as in certain control circuits.
2. Synchronous Flops: Synchronous with clock signal. The output state only at specific times determined by a clock signal (e.g., on the rising or falling edge of the clock). Inputs must be stable for a certain period before and after the clock edge (setup and hold times).