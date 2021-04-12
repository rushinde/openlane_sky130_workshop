# Introduction

This is a tutorial based on the VSD Advanced Physical Design using OpenLANE/Sky130 Workshop. The workshop took place over a 5-day period in April 2021. 

OpenLANE is a RTL to GDSII flow which includes several open source tools including OpenROAD, Yosys, Magic and Fault. Sky130 is an open source PDK created by Google and SkyWater Technology Foundry for 130nm packages. A PDK, or process design kit, is the interface between fabricators and designers. It consists of files containing information about the design flow, such as process design rules (DRC, LVS and Physical Extraction), device models, digital standard cell libraries, I/O libraries and more. RTL IPs and EDA tools are required along with PDKs for a complete ASIC RTL to GDSII design flow. RTL designs can be found on librecores.org, opencores.org and github. Open source EDA tools include Qflow and OpenROAD.

# Day 1

## Set up of OpenLANE and synthesis of design

Working directory

![](/images/1.png)

Set up OpenLANE interactive session using following commands and initialize package

```sh
./flow.tcl  -interactive  
```

```sh
package require openlane 0.9
```

![](/images/2.png)

Prepare sample design and run synthesis

```sh
prep -design picorv32a 
```

```sh
run_synthesis
```

![](/images/5.png)

![](/images/6.png)

Check synthesis reports and results

![](/images/7.png)

![](/images/8.png)

![](/images/9.png)

# Day 2

## Floorplanning and placement

Get examples of configuration variables

![](/images/11.png)

Run floorplan using following commands and check logs. Floorplan should be aligned with configuration variable values defined in .tcl files (Priority sky130.tcl > config.tcl > floorplan.tcl). def and lef files are created.

```sh
run_floorplan
```

Success

![](/images/12.png)

Result of floorplan

![](/images/13.png)

Log

![](/images/14.png)

Check floorplan in Magic using following command (adjusted for your system and current directory). I ran this from my current run's floorplan results folder.

```sh
magic -T /home/rushikesh_shinde/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def
```

Magic layout:

![](/images/15.png)

![](/images/16.png)

Run placement and check if legality passed

```sh
run_placement
```

![](/images/17.png)

Result of placement in Magic:

![](/images/18.png)

# Day 3

## Standard cell design and libraries 

Characterization of standard cell CMOS inverter and subsequent post layout simulation using ngspice. 

Clone [this](https://github.com/nickson-jose/vsdstdcelldesign) repository using the following command to get sample CMOS inverter inverter designed	based on Sky130 pdks. This cell will be later plugged into a pre-built [picorv32a](https://github.com/efabless/openlane/tree/master/designs/picorv32a) core. 

After cloning, inside the cloned directory open the design using Magic:

```sh
magic -T sky130A.tech sky130_inv.mag &
```

CMOS inverter in Magic:

![](/images/19.png)

You can hover over a via or layer and press 's' to select. Then enter `what` in the tkcon window to get information on that layer.	

![](/images/20.png)

In the tkcon window, enter the following commands to extract spice file:

```sh
extract all
```

This will create a spice extraction file for the selected layout. To convert the extraction file to spice file enter the following:

```sh
ext2spice cthresh 0 rthresh 0
ext2spice
```

We will get the spice file. For simulation make changes to spice file as shown here:

![](/images/21.png)

To invoke ngspice: 

```sh
ngspice sky130_inv.spice
```

![](/images/22.png)

To characterize the cell, we need to calculate parameters such as output rise transition, output fall transition, fall cell delay and rise cell delay. We need a plot of output vs time while sleeping input. Then we can find out time values at 20%, 80% etc. to calculate the required parameters. The co-ordinates are printed when we click on a point on the plot.

```sh
plot y vs time a
```

![](/images/23.png)

![](/images/24.png)

![](/images/25.png)

# Day 4


# Day 5

# Acknowledgements

- Kunal Ghosh, Co-Founder (VSD Corp. Pvt. Ltd)















