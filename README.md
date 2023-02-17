# msvsdmod10synccnt
#VSD research Program

The aim is to learn the mixed signal IC design flow from scratch

# Week0

In week0 we did the installation of the tool and created the inverter ckt in xschem, did spice simulation and used magic layout tool. We compared the netlist using the netgen of Layout vs schematic. We also installed ALIGN tool

#### Steps to install Sky130 PDK
1. Clone the repository  
```git clone "git://opencircuitdesign.com/open_pdks"```  
2. Enter the open pdks folder  
```cd open_pdks```  
3. Tell open_pdks to compile and install the Sky130 PDK from the Google Skywater sources  
```configure --enable-sky130-pdk```  
4. Run make to automatically grab the SkyWater and a few other third-party repositories and submodules from github  
```make```
5. Build libraries from the various individual files and repositories. This may take a **LONG TIME** but only happens once, so be patient  
```sudo make install```

### Magic
Magic is an open-source VLSI layout tool.<br /><br />
Install steps:
```
$  git clone git://opencircuitdesign.com/magic
$  cd magic
$	 ./configure
$  make
$  sudo make install

### Netgen
Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic" <br /><br />
Install steps:
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install

### Xschem
Xschem is a schematic capture program <br /><br />
Install steps:
```
$  git clone https://github.com/StefanSchippers/xschem.git xschem_git
$	./configure
$  make
$  sudo make install
```

### Ngspice
```
 $ tar -zxvf ngspice-37.tar.gz
 $ cd ngspice-37
 $ mkdir release
 $ cd release
 $ ../configure  --with-x --with-readline=yes --disable-debug
 $ make
 $ sudo make install
```


Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.
```
$ sudo apt-get update
$ sudo apt-get install xterm
```
### Verifiying the open_pdk installation
An initial working directory can be made by copying the required files as follows:
```
$ mkdir Lab1_and
$ cd Lab1_and
$ mkdir mag
$ mkdir netgen
$ mkdir xschem
$ cd xschem
$ cp /usr/local/share/pdk/sky130A/libs.tech/xschem/xschemrc .
$ cp /usr/local/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
$ cd ../mag
$ cp /usr/local/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
$ cd ../netgen
$ cp /usr/local/share/pdk/sky130A/libs.tech/netgen//sky130A_setup.tcl .
```
Checking if magic works

![mag_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/Check_magic_plus_commands.png)<br /><br />

Checking if xschem works

![xschem_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/xschem_check.JPG)<br /><br />

Checking if netgen works

![netgen_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/netgen_run_command.JPG)<br /><br />

Checking if ngspice works

![spice_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/ngspice_run_command.JPG)<br /><br />


### Creating inverter schematic using xschem
An initial schematic is made by placing components from the open_pdk library<br />
The required changes to the properties of the device can be made here and will automatically reflect in the layout
![xshem_inv](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/inverter_sch.JPG)<br /><br />
Convert the schematic to a symbol
![xshem_sym](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/inverter_sym.JPG)<br /><br />
Using the symbol, we can create an independent test bench to simulate the circuit
![xshem_tb](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/inverter_tb_sch.JPG)<br /><br />

The circuit can be simulated in ngspice. 
![xschem_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/inverter_with_pwl_input.JPG)
![ngspice_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/prelayout_simulation.JPG)
![ngspice_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/prelayout_transient_analysis_plot.JPG)
![ngspice_sim]()
### Creating layout in magic
When creating a new directory for layout using magic make sure you have .magicrc and sky130 related files in that folder you can use the below command to copy the files
```
cp /usr/local/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
```
![magic_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/magic_nfet_parameter_change.JPG)

![magic_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/Layout%20using%20magic.JPG)

After making the layout you need to save this file so Now, go to File --> save and select autowrite. Go to the command window and type the following commands:
```
extract do local
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
This command will create the spice file from the layout which we will use for postlayout simulation. Close magic for now.

### Post layout(magic) ngspice simulation
Now check the directory by typing ls command in terminal. Here you will see the list of different files generated by magic. We are particularly interested in .spice netlist file of the inverter as this file contains the post layout parasitics which were absent in pre-layout netlist.

We need to combine the postlayout .spice file with inverter testbench .spice file which you can find in .xschemrc folder in home directory. Make sure you make a copy of testbench file and edit that file by copying the postlayout netlist into it. 

Run the spice file using ngspice command you should get plot like below


![post_layout simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/post_layout_characterization.JPG)

### Layout using AlIGN tool
To make the layout using align tool you need to make a work directory in ALIGN-public folder. Make sure you have the inverter.spice file in that folder by copying that file and rename it to .sp as ALIGN only reads .sp files

Once you are ready you need to use the following command to make it work
```
python3 ../bin/schematic2layout.py (your .spice file name here) -p ../pdks/SKY130_PDK/
```
After running the command successfully you will see that a GDS file is created your terminal window would look like this 
![ALIGN_tool](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/run_align_tool.JPG)

# Open .gds file
To open this .gds file you need to run magic first using the magic commands as above 
-> Next go to file menu
-> Click on read gds
-> Select your .gds file

Now you may only get box with black boundaries do not panic just press S and then X your layout will be displayed like below

![ALIGN_tool](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/align_tool_layout.JPG)

Next we need to run post layout spice simulation, so go to tkcon window and type the following command
```
extract do local
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

## ALIGN Post-layout characterization
The procedure is same as what we did after magic we need to take the .spice file generated from ALIGN layout paste it in our testbench spice file

After successfully merging the file run below command
````
ngspice (yourfilename).spice
````
You will get the following results
![ALIGN_tool](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/post_align_layout_characterisation.JPG)
![ALIGN_tool](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/post_align_layout_transient.JPG)

# Week 1
First few days of Week1 I completed the task from Week0 such as doing Magic post-layout simulation and ALIGN post-layout simulation

In Week1 there were notably two tasks as shown below

1. Enroll in VSD custom layout course
2. do the pre layout and post layout simulation of Fn Function

$$ F_n = \overline{(B + D) \cdot (A + C) + (E \cdot F)}  $$

![Fn_equation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_diagram.JPG)
(* *The above image was taken from VSD custom Layout course Section 6 Video 25* *)
 ### Creating Fn circuit in xschem
Above circuit as a guide, you need to build this circuit in xschem using * *SKY130 pdks* * as shown in the below figure

![Fn_circuit](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_pre_layout_schematic2.JPG)

After creating the circuit we need to create the symbol put it in the testbench, the symbol is created similar to how we did it in Week0 for inverter

Steps to create Symbol:

1. Once finished with schematic go to Symbol Menu.
2. Click on * * make symbol from schematic * *

Next to is to create test bench with symbol as shown in the below figure.

![fn_circuit_testbench](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_testbench.JPG)
### Prelayout simulation of the ckt
Once the testbench is created we need to run the prelayout simulation, for that you need to generate the netlist by clicking on the * *Netlist* * button, saving the netlist and running the simulation you will obtain a plot as shown below.

![fn_circuit_prelayout_simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_pre_layout_simulation.JPG)
![fn_circuit_prelayout_simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_pre_layout_simulation2.JPG)

Once you get the above plot go to simulation menu and click on 'LVS netlsit: Top level is a .subckt' as we require this netlist for the next step

### Creation of layout using magic

For creation of layout you need to open magic using below command

````
magic -d XR
````

You need to import the .spice file of the circuit, once imported you will be able to see the devices and input and output pins(those who cannot see only outlines of the cell please press 'i' and then 'x')

The following image shows the layout using the magic tool

![fn_circuit_magic layout](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_MAGIC_layout.JPG)

Once the layout is complete we need to extract the spice by running the following commands in the tkcon window

```
extract do local
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

Once the spice file generated we need to copy the netlist into testbench netlist

* * Common errors seen while doing the above task
1. too many .subckt instantiated (Sol: Please only keep one subckt in the netlist)
2. conflicting end statement(Sol: we need to have only one .end statement at the end of the netlist)

### Post Layout simulation using Magic
Once the netlist is copied we need to run the ngspice simulation using the following command
````
ngspice (yourfilename).spice
````
Successful run will result in following plot

![fn_circuit_post_layout_simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_post_layout_simulation1.JPG)
![fn_circuit_post_layout_simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_post_layout_simulation2.JPG)

If we compare the second image with prelayout simulation we see the difference in timings

### Creating layout using ALIGN tool

We need to create layout using ALIGN tool as it automates the routing process which comparede to magic is easy. So to run ALIGN you need to go ALIGN-public folder and go to work directory and copy the prelayout netlist of the Fn function there(Note rename the file from .spice to .sp).

Before going to work directory you need to initiate the python virtual environment by using following commands
````
python3 -m venv general
source general/bin/activate
````
once the virtual environment is initiated we need to run the following command to start routing of the layout

```
python3 ../bin/schematic2layout.py (your .spice file name here) -p ../pdks/SKY130_PDK/

```

You might encounter error as shown below

#### Error 1

![Error1](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Error1.png)

#### Error 2

![Error2](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Error2.png)

#### Solution

Solution to error1 is that SKY130 fets we get in the netlist have extra parameters just reduce it to the given parameters as shown in the solution netlist

Solution to error2, if you are facing this error there might exist a space after the nf=2 in the netlist

![Solution](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/solution.png)

After successfully running the ALIGN tool we get the .gds file which we read in MAGIC tool by going to file menu--> click on 'read .gds file ' and select the appropriate file.

The following layout will appear

![ALIGN_layout](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_ALIGN_layout.JPG)

Once we can see the layout, again we go through steps to generate the spice file

```
extract do local
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
### Post layout (generated using ALIGN) simulation

The generated spice netlist we need to copy into the testbench and run the ngspice simulation. 

Successful run will result in following plot:

![ALIGN_layout_simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_post_ALIGN_layout_simulation1.JPG)
![ALIGN_layout_simulation](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week1/Fn_circuit_post_ALIGN_layout_simulation2.JPG)












































