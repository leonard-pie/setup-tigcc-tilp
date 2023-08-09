# Setup TIGCC compiler and TILP on Debian 11 Bullseye
This is a small guide on how to set up [TIGCC](http://tigcc.ticalc.org/) (compiler) and [TILP](http://lpg.ticalc.org/prj_tilp/) (TI Linking Program) on Debian 11 Bullseye.

**TIGCC** (from "TI" and "GCC") is a software development environment which allows developers to program and compile A68K assembly, GNU assembly, and C code for the Motorola 68000 series Texas Instruments graphing calculators (TI-89 (Titanium), TI-92 Plus and Voyage 200, as well as experimental support for the TI-92 with the Fargo shell). TIGCC is licensed under the GNU General Public License. 

**TILP** (formerly GtkTiLink) can transfer data between Texas Instruments graphing calculators and a computer. It works with all link cables (parallel, serial, Black/Gray/Silver/Direct Link) and it supports the TI-Z80 series (73..86), the TI-eZ80 series (83PCE, 84+CE), the TI-68k series (89, 92, 92+, V200, 89T) and the Nspire series (Nspire Clickpad / Touchpad / CX, both CAS and non-CAS).


### Step 1: install essential packages
```bash
sudo apt install gcc g++ make build-essential git autoconf automake autopoint libtool libtool-bin libglib2.0-dev zlib1g-dev libusb-1.0-0-dev libgtk2.0-dev libglade2-dev gettext bison flex groff texinfo xdg-utils libarchive-dev intltool
```


### Step 2: download and launch gcc4ti installation script
```bash
mkdir /tmp/gcc4ti
cd /tmp/gcc4ti/
git clone https://github.com/debrouxl/gcc4ti
cd gcc4ti/trunk/tigcc-linux/scripts/
sudo ./updatesrc
cd ../gcc4ti-0.96b11
sudo scripts/Install
```
Leave the default by pressing enter when asked, should be 7 times.


### Step 3: Set enviroment variables
```bash
echo "export PATH=$PATH:/usr/local/share/gcc4ti/bin" >> ~/.bashrc
echo "export TIGCC=/usr/local/share/gcc4ti" >> ~/.bashrc
```
At this point you can restart your bash or execute this command to reload .bashrc variables
```bash
source ~/.bashrc
```


### Step 4: Download and install tilp
```bash
mkdir /tmp/tilp
cd /tmp/tilp/
curl https://www.ticalc.org/pub/unix/tilp.tar.gz -O
curl http://lpg.ticalc.org/prj_tilp/download/install_tilp.sh -O
sudo chmod 755 install_tilp.sh
sudo ./install_tilp.sh
```
Leave the default by pressing enter.
At this point everything should be set up.


## Example compiling and transferring a C file on a TI-89
Now let's try to create a Hello World file, compile it and execute it on a TI-89 calculator.

### Creating a C file
```bash
nano hello_world.c
```


### Hello World C File
```c
#define USE_TI89
#include <tigcclib.h>

void _main(void) {
    // clear the screen
    ClrScr();

    // print the string at the top left corner of the display
    DrawStr(0, 0, "Helloo, World!", A_NORMAL);

    // wait for a key press before the program exits
    ngetchx();
}
```


### Compiling
```bash
tigcc hello_world.c
```
you should now see a hello_world.89z compiled file.


### Using TILP to transfert the file to your TI calc
connect your calc
```bash
sudo tilp
```
From the GUI:

File -> Change Device -> Click search icon -> Double click on calc -> OK
Press refresh button
Drag your hello_world.89z from your local disk (right) to the calculator (left)


### Launching the compiled file on calculator
Press button "2nd" and then button "-" to activate "var-link" menu.
Select hello_world -> ENTER -> close bracket -> ENTER
You should now see the output of your c file 




