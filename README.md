# ArduCopter SITL Setup

## Clone the Repository
Run the following commands to clone the official GitHub Repo of ArduPilot.
```
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git submodule update --init --recursive
```
## Install dependencies
Run the Following Commands inside the ardupilot directory
```
cd Tools/environment_install
./install-prereqs-ubuntu.sh
```
Follow the Default Installation Instructions.
Wait till installation to complete.
This step will create a virtual enviornment (venv-ardupilot) in Home Directory where all the dependencies Would be install. 
## Configure the ArduPilot Firmware
:warning: Activate the Virtual Enviornment Before Running These Coommands.
Move to ardupilot Directory before running the following commands
```
./waf configure --board sitl
```
```
./waf copter
```
## Test the SITL
```
./Tools/autotest/sim_vehicle.py -v ArduCopter --console 

```
## Add to path (Optional)
```
sudo nano ./bashrc
```
Add the following lines at the end of file
```
export PATH=$PATH:"Path to autotest folder"

```
Restart the terminal and try running sim_vehicle.py

## Gazebo Setup
ArduCopter SITL Supports Gazebo Harmonic (Outdated as of now but works well)
Run this instructions in the order
:warning: Exit Virtual Enviornment Before this Setup
```
sudo apt-get update
sudo apt-get install curl lsb-release gnupg
```
```
sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt-get update
sudo apt-get install gz-harmonic
```

## Arducopter Gazebo Plugin
```
sudo apt update
sudo apt install libgz-sim8-dev rapidjson-dev
sudo apt install libopencv-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl
```
```
mkdir -p gz_ws/src && cd gz_ws/src
git clone https://github.com/ArduPilot/ardupilot_gazebo
```
```
export GZ_VERSION=harmonic
cd ardupilot_gazebo
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j4
```
```
echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/gz_ws/src/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
echo 'export GZ_SIM_RESOURCE_PATH=$HOME/gz_ws/src/ardupilot_gazebo/models:$HOME/gz_ws/src/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc
```

## Sim test
### Run Gazebo
```
gz sim -v4 -r iris_runway.sdf
```
### RUN SITL
:warning: Activate the Virtual Enviornment Before this step
```
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console
```
### Test Connection
put following commands line by line in SITL Terminal
```
mode guided
arm throttle
takeoff 5
```

