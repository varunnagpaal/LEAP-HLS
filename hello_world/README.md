Hello World
=============

This program is the running example in the LEAP HLS tutorial session. It reads 256 values from scratchpad0 and writes these values squared back to scratchpad1. Scratchpad0 is initialized with a *.dat file.

Steps to build it:
-------------

### Generating HLS kernel:

1) Create a Vivado HLS project in `VivadoHLS/hello_world/.` using the *.cpp and *.h sources in `VivadoHLS/.` In the synthesis settings, make sure to enable the generation of active low, synchronous reset inputs.

2) Change into `generated_verilog/.` and run `./reload_source_files.sh`.

3) Change into `testbench/.` and run `make` in order to test whether the HLS design can be imported into a *BVI wrapper* and simulated using *iverilog*. When the HLS core interface or the wrapper files are modified, it is recommended to use the testbench to check quickly that everything works as expected before building the actual LEAP simulation model (which may take up to 15 minutes).

### Building a hardware simulation model

1) Run `./build_sim.sh hello_world_sim.apm` to build the LEAP simulation model including the HLS core. The [.apm file](https://github.com/AWB-Tools/awb/wiki//Apm-file) contains several user-defined parameters, such as the L1 and L2 cache sizes and hardware implementations.

2) Run `./run_sim.sh hello_world_sim.apm` to setup the LEAP benchmark and start the iverilog simulation of the model.

3) The core output of the Vivado HLS C simulation and LEAP iverilog simulation should be identical.

### Hardware implementation on an FPGA board

1) Run `./build_synth.sh hello_world_vc707_synth_synplify.apm` or `./build_synth.sh hello_world_vc707_synth_vivado.apm` to build the FPGA implementation. The [.apm file](https://github.com/AWB-Tools/awb/wiki//Apm-file) contains several user-defined parameters, such as the L1 and L2 cache sizes and the traget clock rate. The .apm files build for a Xilinx VC707 board (Virtex 7, XC7VX485T-2). They invoke either Synopsys Synplify or Vivado for netlist synthesis and Vivado for placement and routing.

2) Run `./run_synth.sh hello_world_vc707_synth.apm` to setup the LEAP benchmark, program the FPGA and run the application. The build (`pm`) and benchmark (`bm`) directories are located in the `build/default` folder of your LEAP workspace directory. The number of input samples (64 by default) is configurable through a [dynamic parameter](https://github.com/LEAP-FPGA/leap-documentation/wiki/Dynamic-parameters) in LEAP which can be set as a command line argument: `<LEAP workspace dir>/build/default/bm/null/run --param N_SAMPLES=XX`

3) The core output of the Vivado HLS C simulation and the STDIO output of the application should be identical.

