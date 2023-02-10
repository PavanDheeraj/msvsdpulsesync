# msvsdpulsesync

## Installation of Oracle Virtual Box with Ubuntu 22.04
Go to https://www.virtualbox.org/wiki/Downloads
Under VirtualBox 7.0.6 platform packages, click on Windows hosts save the .exe file in Opensourcetoolinstall folder.
Install the VirtualBox.
Download Ubuntu 22.04.1 LTS from https://ubuntu.com/download/desktop
Install Ubuntu 22.04.1 LTS in the virtualbox.

## Installation of Tools and SKY130 PDKs
### Magic
Magic is an open-source VLSI layout tool. Install magic using the following commands
```
$  git clone git://opencircuitdesign.com/magic
$  cd magic
$	 ./configure
$  make
$  sudo make install
```

### Netgen
Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic". Install netgen using the following commands.
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install
```

### Ngspice
Ngspice is an open-source mixed-level/mixed-signal electronic circuit simulator. Download ngspice-39 tarball ngspice-39.tar.gzfrom https://ngspice.sourceforge.io/download.html into the work directory. Install ngspice using the following commands.
```
 $ tar -zxvf ngspice-39.tar.gz
 $ cd ngspice-39
 $ mkdir release
 $ cd release
 $ ../configure  --with-x --with-readline=yes --disable-debug
 $ make
 $ sudo make install
```

### Xschem
Xschem is a schematic capture program. Install xschem using the following commands
```
$  git clone https://github.com/StefanSchippers/xschem.git xschem_git
$	./configure
$  make
$  sudo make install
```

### Open PDK
Open_PDKs is distributed with files that support the Google/SkyWater sky130 open process description https://github.com/google/skywater-pdk. Open_PDKs will set up an environment for using the SkyWater sky130 process with open-source EDA tools and tool flows such as magic, qflow, openlane, netgen, klayout, etc. Install Open PDK using the following commands
```
$  git clone git://opencircuitdesign.com/open_pdks
$  open_pdks
$	./configure --enable-sky130-pdk
$  make
$  sudo make install
```
### Align 
ALIGN is an open source automatic layout generator for analog circuits. Install ALIGN using the following commands.
```
export CC=/usr/bin/gcc
export CXX=/usr/bin/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public

#Create a Python virtualenv
python -m venv general
source general/bin/activate
python -m pip install pip --upgrade

# Install ALIGN as a USER
pip install -v .

# Install ALIGN as a DEVELOPER
pip install -e .

pip install setuptools wheel pybind11 scikit-build cmake ninja
pip install -v -e .[test] --no-build-isolation
pip install -v --no-build-isolation -e . --no-deps --install-option='-DBUILD_TESTING=ON'

$git clone https://github.com/ALIGN-analoglayout/ALIGN-pdk-sky130
cp -r ALIGN-pdk-sky130 ALIGN-public/pdks/
```
###  Verify the Open PDK installation
An initial working directory can be made by copying the required files as follows:
```
$ mkdir trial1
$ cd trial1
$ mkdir magic
$ mkdir netgen
$ mkdir xschem
$ cd xschem
$ cp /usr/local/share/pdk/sky130A/libs.tech/xschem/xschemrc .
$ cp /usr/local/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
$ cd ../magic
$ cp /usr/local/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
$ cd ../netgen
$ cp /usr/local/share/pdk/sky130A/libs.tech/netgen//sky130A_setup.tcl .
```
## Simulation of Inverter using Xschem and Ngspice

### Pre-layout Simulation using Xschem and Ngspice
#### DC Analaysis of CMOS inverter

Create the schematic for inverter in xschem. It is made by placing components from the Open PDK library.
DC analysis is done by using the .dc command . This information must be entered by the user in code_shown.sym from components. Also, this symbol contains the library file.

![Screenshot (19)](https://user-images.githubusercontent.com/58168687/218138987-c4463a6e-c961-415c-93a2-523456a115d6.png)

Inverter Schematic

![inverter](https://user-images.githubusercontent.com/58168687/218140120-f64ac384-bd95-4d35-ac17-e14783db6aa1.PNG)

Click on Netlist option available on the top right corner of the window to generate a spice file for the schematic created. Click on Simulate to run the simulation and obtain the voltage-transfer characteristic(VTC) for the inverter.

![inverter_wave](https://user-images.githubusercontent.com/58168687/218139408-c80aa535-f71c-4acf-9eed-a2f4f0f772c2.PNG)

#### Transient Analaysis of CMOS inverter
Transient analysis is done by using the .tran command . This information must be entered by the user in code_shown.sym from components. Also, this symbol contains the library file.

![xsch_inv_img](https://user-images.githubusercontent.com/58168687/218140208-e6525092-df58-4d91-a32d-b9e4c90a0688.PNG)

The graph shows the operation of the inverter.

![xsch_inv_tran](https://user-images.githubusercontent.com/58168687/218140761-4ec2f475-6a2d-4a5c-a113-2174cf79d895.PNG)

### Simulation using Magic and Ngspice
The layout 'inverter.mag' was drawn in Magic as shown below.

![mag_inv_lay](https://user-images.githubusercontent.com/58168687/218141392-60864dbd-7d30-424e-a27f-0857970db138.PNG)

The spice netlist without parasitics is extracted using the following commands:
```
$ extract all
$ ext2spice lvs
$ ext2spice -d 
```

This spice netlist is again simulated using Ngspice. 

![mag_inv_tran](https://user-images.githubusercontent.com/58168687/218142957-088a643b-f6b9-4f04-97d9-42d5dcd0df16.PNG)

### LVS Report
The layout vs schematic compares the pre-layout netlist with the netlist extracted from the layout(in our case, without the parasitics). The mismatch is due to voltage sources used in the pre-layout simulation.

The report is shown below:

![lvs](https://user-images.githubusercontent.com/58168687/218144359-a9001a69-49b7-4b3f-9053-b47758eb17f2.PNG)

### Comparison of results
We can note that the graph of out vs time for both pre-layout simulation and post layout simulation(without parasitics) are similar.
