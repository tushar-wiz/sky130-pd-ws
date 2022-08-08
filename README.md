# Advanced Physical Design Workshop Using OPENLANE/SKY130

## Day 1
We enter the directory where openlane is installed and run the docker command.
``` 
cd /Desktop/work/tools/openlane_working_dir/openlane
docker
```
After that we run the openlane script.
```
./flow.tcl -interactive
package require openlane 0.9
```
![](https://imgur.com/YHyJgjd.png)

Next we prep the design.

```prep -design picorv32a```
prepares the picorv32a design
![](https://imgur.com/qPDwEhD.png)

```run_synthesis```
We get a cell list like below.  

![](https://imgur.com/vxPRTo8.png)  
![](https://imgur.com/y1APzSV.png)  
Flop Ratio --> 1613/14876 = **0.1084**  
Buffer Ration --> (1656+8)/14876 = **0.1118**

## Day 2 
Tcl files for each step is stored under the config folder
![](https://imgur.com/CTYy7sq.png)

We run the command ```run_floorplan```
![](https://imgur.com/0PW4oM1.png)
After the command we can find the log files at ```/Desktop/work/tools/openlane_working_dir/openlane/```dateOfFolder```/logs/floorplan```
![](https://imgur.com/DxrJu0w.png)

### Magic
We open the created floorplan file in Magic.
![](https://imgur.com/SHo97h1.png)
![](https://imgur.com/Xo0N0D0.png)  

After running ```run_placement``` we open the .def file in magic
![](https://imgur.com/hrIyiHy.png)
![](https://imgur.com/cOWFWiX.png)

### Changing FP_IO_MODE
Setting ```FP_IO_MODE``` 2
![](https://imgur.com/BO9Jee4.png)  
All IO pins at the bottom left corner.
![](https://imgur.com/foAzuiC.png)

## DAY 3
First we start by cloning the repo which contains the standard cell files.
![](https://imgur.com/xi1365k.png)
Copying the necessary files to their required location.
![](https://imgur.com/FWvW4nn.png)
Opening the standard cell in Magic.
![](https://imgur.com/4nHmjXz.png)

### Extracting the SPICE netlist

![](https://imgur.com/q6REiNh.png)

A ```.spice``` file is generated which is edited for our needs.
![](https://imgur.com/dhlCu4z.png)

### NgSPICE
Running the ngspice sim with the command.  
```ngspice sky130_inv.spice```  
and plotting using the command.  
```plot y vs time a```  
> ![](https://imgur.com/I2VOfK4.png) Before Changing the C load

> ![](https://imgur.com/d7gfv4Y.png) After Changing the C load

**RISE TIME**  
![](https://imgur.com/ovlagYa.png)
Rise time comes out to be **0.061ns**  
  

**RISE PROPAGATION DELAY**  
![](https://imgur.com/S9u8Jot.png)  
Rise propagation comes out to be **0.060ns**  

## DAY 4
Open the inv cell in magic and we set a grid.
![](https://imgur.com/alZmdjv.png)
The ```.lef``` file generated from the command ```lef_write```
![](https://imgur.com/MvGR66O.png)

Copying the necessary files to merge the lef file.
![](https://imgur.com/pKGkzwf.png)

### Config.tcl
Adding lines to configure openlane to use ```picorv32a``` design.
![](https://imgur.com/4ofyaYb.png)

```run_synthesis```
![](https://imgur.com/Fd8jgFA.png)

> ![](https://imgur.com/wAcwy6p.png)  
Successful integration of the inverter standard cell.  

### Finding the cell in the Design
![](https://imgur.com/GqYerq0.png)

### STA
Running the command ```sta pre-sta.tcl``` prints a timing report. Where a slack of -36.62 is reported. 
![](https://imgur.com/zWErPS2.png)

Recognizing slower cells.  
![](https://imgur.com/dVbb1bP.png)
![](https://imgur.com/2R0ddmM.png)  
Slower cells are replaced by faster cells with the command ```replace_cell```. 
> ![](https://imgur.com/lUxgN8c.png) Slack is reduced to -34.66 from -36.62

### Clock Tree Synth
```run_cts```
![](https://imgur.com/VpTFY3P.png)

## DAY 5
### PDN
To generate the Power Distribution Network we run the following commands.
```
init_floorplan
place_io
global_placement_or
detailed_placement
tap_decap_or
detailed_placement
gen_pdn
```
`gen_pdn`
![](https://imgur.com/Eqi2VNg.png)  

`run_routing`
![](https://imgur.com/5jXV9Cu.png)  

Opening the routing result in magic.
![](https://imgur.com/cF5OCXk.png)
Checking a Standard Cell
![](https://imgur.com/IITcqCD.png)

*DRC checks are incomplete*
