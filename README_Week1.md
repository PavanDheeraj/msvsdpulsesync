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
###  Verifiying the Open PDK installation
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

