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
![magic_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/magic_nfet_parameter_change.JPG)
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

### ALIGN Post-layout characterization
The procedure is same as what we did after magic we need to take the .spice file generated from ALIGN layout paste it in our testbench spice file

After successfully merging the file run below command
````
ngspice (yourfilename).spice
````
You will get the following results
![ALIGN_tool](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/post_align_layout_characterisation.JPG)
![ALIGN_tool](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Week0/post_align_layout_transient.JPG)

---->![netgen_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/layout_vs_schematic_netgen_result.JPG)




































