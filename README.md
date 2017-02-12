# EnergyPlus Simulator for Raspberry Pi

EnergyPlus is a whole building energy simulation program that engineers,
architects, and researchers use to model both energy consumption—for heating,
cooling, ventilation, lighting, and plug and process loads—and water use in
buildings. The instructions to prepare EnergyPlus binaries for the Raspberry Pi
ARM platform are provided below. You can download an already compiled binary
package from [here](energyplus-8.5.0-armhf.deb).

1. Install the following packages:
```
sudo apt-get install cmake cmake-curses-gui checkinstall
```

2. Download the latest EnergyPlus (V8.5.0)
```
cd /home/pi &&
wget https://github.com/NREL/EnergyPlus/archive/v8.5.0.tar.gz &&
tar -xzvf v8.5.0.tar.gz
```
Alternatively, you can clone the [EnergyPlus GitHub Repository](https://
github.com/NREL/EnergyPlus) if you want to get the latest
update:
```
git clone https://github.com/NREL/EnergyPlus.git EnergyPlus-8.5.0
```
3. Navigate in the terminal to the root of the source tree (i.e. cloned
repository). You will launch the cmake GUI from this location.
```
cd EnergyPlus-8.5.0
ccmake ../EnergyPlus-8.5.0
```
4. A new screen Configure the build in ccmake, then generate the make files.
* Set `CMAKE_BUILD_TYPE` to `MinSizeRel`
* Set `CMAKE_INSTALL_PREFIX` to `/usr/local/EnergyPlus-8.5.0`
* Empty the value of `CMAKE_VERSION_BUILD`
* Press [t] to display advance settings. See if any option is set to "YES",
 change it to "NO" because we are compiling for RaspberryPi and make it as light
 as possible.
* Press [g] to generate MakeFile and exit

5. Since you are already in the build folder, just run `make -j N`,
where N is the number of cores you'd like to employ to build the project.
```
make -j2 # use 2 cores from a total of 4 cores on RPi3
```
6. The make process might take up to one hour to complete. Once completed, run
the following to install EnergyPlus and generate `.deb` package file. The .deb
file can be found in the root of the source directory.
```
sudo checkinstall
```
Save the `.deb` file so that in future you can directly install using the
`.deb` file

7. Add symbolic links
```
sudo ln -s /usr/local/EnergyPlus-8.5.0/Energy+.idd /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/energyplus /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/energyplus /usr/local/bin/EnergyPlus
sudo ln -s /usr/local/EnergyPlus-8.5.0/PreProcess/IDFVersionUpdater /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/PreProcess/FMUParser/parser /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/runenergyplus /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/runepmacro /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/runreadvars /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/PreProcess/IDFVersionUpdater/V8-2-0-Energy+.idd /usr/local/bin
sudo ln -s /usr/local/EnergyPlus-8.5.0/PreProcess/IDFVersionUpdater/V8-3-0-Energy+.idd /usr/local/bin
```
