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
On the second day of the workshop, the various types of standard cells such as fast, slow and typical were discussed. These cells ensure that Process-Voltage-Temperature (PVT) variations do not have a serious impact on the working of the chip. <br />

In addition, the difference between hierarchical and flat synthesis was also introduced. When trying to synthesize the design described in multiple_modules.v we see an error as shown below: <br />
![alt text](Day2/show_err_mm.png)  <br />

This can be resolved by running the command <br />
`show multiple_modules` <br />
instead of the `show` command we usually run since this represents the hierarchical design. The result of using this is: <br />
![alt text](Day2/show_mm_flat.png)  <br />

The concept of module-level synthesis was also introduced and is preferred when we have multiple instances of the same module. The result of performing the module-level synthesis for sub-module1 in multiple_modules.v is: <br />
![alt text](Day2/show_sub_module1.png)  <br />

We also saw variations of the D flip-flop which are described below along with their simulation and synthesis results:
- Asynchronous reset D flip-flop (dff_asyncres.v): The reset signal of the flip-flop is independent of the clock <br />
![alt text](Day2/async_res.png)  <br />
![alt text](Day2/show_asyncres.png)  <br />

- Asynchronous set D flip-flop (dff_async_set.v): The set signal of the flip-flop is independent of the clock <br />
![alt text](Day2/async_set.png)  <br />

- Synchronous reset D flip-flop (dff_syncres.v): The reset signal of the flip-flop is dependent on the positive edge of the clock <br />
![alt text](Day2/sync_res.png)  <br />
![alt text](Day2/show_syncset.png)  <br />

- Asynchronous + Synchronous reset D flip-flop (dff_asyncres_synncres.v): There are two reset signals for the flip-flop, one which depends on the positive edge of the clock and the other which is independent of the clock <br />
![alt text](Day2/async_sync_res.png)  <br />
![alt text](Day2/show_async_sync.png)  <br />

Finally, we also explored some synthesis optimizations for the designs described in mult_2.v and mult_8.v.

- For mult_2.v
![alt text](Day2/show_mul2.png)  <br />

- For mult_8.v
![alt text](Day2/show_mul8.png)  <br />

## Day 3 - Combinational and sequential optmizations



## Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch