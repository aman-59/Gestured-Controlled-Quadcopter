# Basics of Aerodynamics

# Quadcopter Movements

# Quadcopter calculation

# Gesture Recognisation 

# Command Scripting






























# SITL Setup
Install Requirements: 
## Ground Control Station
 Mission Planner: https://ardupilot.org/planner/docs/mission-planner-installation.html#windows-installation 
## Ubuntu 22.04.5 LTS
https://releases.ubuntu.com/jammy/
## Python 3.10.11

```
sudo apt install -y software-properties-common build-essential zlib1g-dev \
libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev \
libffi-dev libsqlite3-dev wget libbz2-dev
```
```
cd /tmp    
wget https://www.python.org/ftp/python/3.10.11/Python- 3.10.11.tgz
```

```
tar -xvf Python-3.10.11.tgz
cd Python-3.10.11
```

```
 ./configure --enable-optimizations
```

```
make -j$(nproc)
sudo make altinstall
```

## ArduPilot 
Run the following commands to clone the official GitHub Repo of ArduPilot.
```
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git submodule update --init --recursive
```
Run the Following Commands inside the ardupilot directory
```
cd Tools/environment_install
./install-prereqs-ubuntu.sh
```
Follow the Default Installation Instructions.
Wait till installation to complete.
This step will create a virtual enviornment (venv-ardupilot) in Home Directory where all the dependencies Would be install.

## Configure ArduPilot Firmware
:warning: Activate the Virtual Enviornment Before Running These Coommands.
Move to ardupilot Directory before running the following commands
```
./waf configure --board sitl
```
```
./waf copter
```

Test Run:

```
./Tools/autotest/sim_vehicle.py -v ArduCopter --console 

```
Restart the terminal and try running sim_vehicle.py

# Gazeebo Setup 
## ArduCopter 
SITL Supports Gazebo Harmonic (Outdated as of now but works well)
Run this instructions in the order
 Exit Virtual Enviornment Before this Setup
 
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

## ArduCopter Gazebo Plugin
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


# Simulation Run

Run all the applications in order all in new terminal. 
1. Gazebo 
2. ArduCopter
3. Command Script

```
gz sim -v4 -r iris_runway.sdf
```
```
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON
```

```
git clone https://github.com/aman-59/Gestured-Controlled-Quadcopter.git
cd Gestured-Controlled-Quadcopter
python3 quad.py
```



# Test Flight.
<table>
  <tr>
    <td align="center">
      <h4>Forward</h4>
      <img src="media/forward.gif" alt="Forward Gesture" width="100%">
    </td>
    <td align="center">
      <h4>Backward</h4>
      <img src="media/backward.gif" alt="Backward Gesture" width="100%">
    </td>
    <td align="center">
      <h4>Left</h4>
      <img src="media/left.gif" alt="Left Turn Gesture" width="100%">
    </td>
  </tr>
  <tr>
    <td align="center">
      <h4>Right</h4>
      <img src="media/right.gif" alt="Right Turn Gesture" width="100%">
    </td>
    <td align="center">
      <h4>LAND</h4>
      <img src="media/land.gif" alt="Land Gesture" width="100%">
    </td>
    <td width="33%">
      </td>
  </tr>
</table>
