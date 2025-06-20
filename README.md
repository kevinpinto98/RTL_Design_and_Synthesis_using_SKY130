# RTL Design and Synthesis Using SKY130
This repository documents the work done during the course of the "RTL Design and Synthesis workshop Using sky130" conducted from 11/06/25 to 20/06/25.

## Day 1 - Introduction to Verilog RTL design and Synthesis
The agenda for the first day was to get familiar with the tool flow using iverilog and also with using yosys for synthesizing the design.<br />
The figure below shows the structure of a tesst bench that is used to provide a stimulus to verify the design functionality:<br />
![alt text](Day1/tb.png)

Once we have the design and the testbench reaady we are ready to use iverilog to versify if the design works as intended. To do so, we use the flow as depicted below:<br />
![alt text](Day1/iverilog_flow.png) <br />

When simulating the behaviour of the design using iverilog the following steps need to be followed:
- To launch the iverilog tool you need to provide the design and testbench files as arguments <br />
`iverilog <design.v> <tb_design.v>`
- Following this, a a.out file will be created that you can execute by <br />
`./a.out`
- This open the .vcd file, using which you can observe the signal waveforms using gtkwave <br />
`gtkwave <vcd-file-name>` <br />

The result of the iverilog-based simulation flow for the design good_mux.v using the tb_good_mux.v as the testbench is as follows:
![alt text](Day1/good_mux.png) <br />

After verifying the design,  it's time to use the yosys tool to synthesize the design. The figure below explains the flow using yosys tool: <br />
![alt text](Day1/yosys_flow.png)

The following steps explain the process of using yosys for synthesis:
- Launch the yosys tool <br />
`yosys`
- Once yosys is launched, read the .lib files using <br />
`read_liberty -lib <path-to-lib-file>`
- Next, read the verilog design file(s) using <br />
`read_verilog <.v file>`
- Following this, we synthesize the top module of the design <br />
`synth -top <module-name>`
- Once this completes, we run the following command so that the netlist is created <br />
`abc -liberty <path-to-lib-file>`
- To write the netlist we run the following command <br />
`write_verilog <name.v>` <br />
Note: The name can be anything the user chooses. <br />
For generating a simplified version of the netlist we can also run <br />
`write_verilog -noattr <name.v>`
- To see the synthesized design in a graphical form run <br />
`show` <br />

Once synthesis completes, we need to verify that the synthesized design (i.e. the netlist generated) works as intended, as verified when using the iverilog tool. The flow for doing so is as shown in the figure below: <br />
![alt text](Day1/synth_verif.png)  <br />

The entire synthesis flow can be summarized as follows: <br />
![alt text](Day1/synth_flow.png)  <br />

For Day1, we exercised all the steps described above for the good_mux.v design. The synthesis setps are executed are: <br />
![alt text](Day1/synth_steps_good_mux.png)  <br />

The state of the signals after creating the netlist is:<br />
![alt text](Day1/good_mux_all_signals.png)  <br />

The synthesized design for good_mux.v is as follows: <br />
![alt text](Day1/good_final.png)  <br />

Finally, the netlist generated for good_mux.v is as shown below: <br />
![alt text](Day1/netlist_good_mux.png)  <br />

The simplified version of this netlist is: <br />
![alt text](Day1/netlist_simplified_good_mux.png)  <br />

## Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles




## Day 3 - Combinational and sequential optmizations



## Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch