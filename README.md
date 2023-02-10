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
![mag_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/Check_magic_plus_commands.png)<br /><br />
Checking if xschem works
![xschem_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/xschem_check.JPG)<br /><br />
Checking if netgen works
![netgen_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/netgen_run_command.JPG)<br /><br />
Checking if ngspice works
![spice_test](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/ngspice_run_command.JPG)<br /><br />


### Creating inverter schematic using xschem
An initial schematic is made by placing components from the open_pdk library<br />
The required changes to the properties of the device can be made here and will automatically reflect in the layout
![xshem_inv](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/inverter_sch.JPG)<br /><br />
Convert the schematic to a symbol
![xshem_sym](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/inverter_sym.JPG)<br /><br />
Using the symbol, we can create an independent test bench to simulate the circuit
![xshem_tb](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/inverter_tb_sh.JPG)<br /><br />

The circuit can be simulated in ngspice. *make sure to disable .subckt in the simulation tab for the netlist generated for the sim*
![xschem_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/inverter_with_pwl_input.JPG)

![xschem_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/magic_nfet_parameter_change.JPG)


![xschem_sim](https://github.com/Nirav-Mange/msvsdmod10synccnt/blob/main/layout_vs_schematic_netgen_result.JPG)




































