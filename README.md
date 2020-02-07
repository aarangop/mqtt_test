# mqtt_test
Sample project to get the c++ paho mqtt library running

# Instructions

The following instructions detail how to install the paho.mqtt.c and paho.mqtt.cpp libraries inside a custom project that uses the CMakeLists.txt provided in this repo.

## Installing the paho.mqtt.c library

The Eclipse Paho MQTT C++ Client Library will be installed in the lib directory inside the project.
The Eclipse Paho MQTT **C** Client is a dependency of the MQTT C++ Client, so it has to be installed as well.

First, to install the MQTT C Client, clone the paho.mqtt.c repository somewhere:

`
cd somewhere
git clone https://github.com/eclipse/paho.mqtt.c.git
`

Then, the library can be configured and generated using CMake.
Note that the C Client will be installed in the lib directory of the project.
The path to the lib directory will be for this example: `/usr/arang/repos/mqtt_test/lib`
The following options are used to generate the cmake build files.

```
CMAKE_INSTALL_PREFIX=/usr/arang/repos/mqtt_test/lib/paho.mqtt.c
```

Execute:

```
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arang/repos/mqtt_test/lib/paho.mqtt.c
```

This will generate the required build files. In order to compile and install paho.mqtt.c, execute:

```
cmake --build . --target install
```

This will install the paho.mqtt.c library in the specified location `/usr/arang/repos/mqtt_test/lib/paho.mqtt.c`

## Installing the paho.mqtt.cpp library

First, clone the paho.mqtt.cpp repo and navigate to the `/build` directory.

```
cd somewhere
git clone https://github.com/eclipse/paho.mqtt.cpp.git
cd paho.mqtt.cpp/build
```

It is necessary to consider some options before installing the library. 

First, locate the directory where paho.mqtt.cpp will be installed, using the `CMAKE_INSTALL_PREFIX` option and setting it to the corresponding location inside the project.

```CMAKE_INSTALL_PREFIX=/usr/arang/repos/mqtt_test/lib/paho.mqtt.cpp/```

Since the paho.mqtt.cpp library relies on paho.mqtt.c, it is necessary for CMake to know the location of the paho.mqtt.c installation. For this we need to specify the location of the include and lib directories of paho.mqtt.c. Remember that paho.mqtt.c was installed in the lib directory of this project. The relevant CMake options are:

```
PAHO_MQTT_C_INCLUDE_DIRS=/usr/arang/repos/mqtt_test/paho.mqtt.c/include
PAHO_MQTT_C_LIBRARIES=/usr/arang/repos/mqtt_test/paho.mqtt.c/lib/<paho-mqtt-lib-flavor>
PAHO_BUILD_SHARED=ON
```

Since the paho.mqtt.c library comes in a variety of flavors, it is up to the user to select the corresponding flavor among:
- paho-mqtt3a   --> asynchronous
- paho-mqtt3as  --> asynchronous, using SSL
- paho-mqtt3c   --> classic
- paho-mqtt3cs  --> classic, using SSL

This example does use the asynchronous version without SSL. Therefore the correct `PAHO_MQTT_C_LIBRARIES` path for this case would be: `/usr/arang/repos/mqtt_test/paho.mqtt.c/lib/paho-mqtt3a.lib`

Once the project is set up, it is necessary to copy a version of both the paho.mqtt.c and paho.mqtt.cpp dll into the executable's folder, therefore the option `PAHO_BUILD_SHARED` is set to `ON`.

With these options in mind, we can generate the build files using:

```
cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arang/repos/mqtt_test/lib/paho.mqtt.cpp/ -DPAHO_MQTT_C_INCLUDE_DIRS=/usr/arang/repos/mqtt_test/paho.mqtt.c/include -DPAHO_MQTT_C_LIBRARIES=/usr/arang/repos/mqtt_test/paho.mqtt.c/lib/paho-mqtt3a.lib -DPAHO_BUILD_SHARED=ON
```

Then we can compile and install using:

```
cmake --build . --target install
```

## Building the project

Building the project is now very straightforward with the provided CMakeLists.txt.

If not present, make a build directory
```
mkdir build
cd build
```

Then generate the build files and then build the project.
```
cmake ..
cmake --build . --target test
```

Before running the project don't forget to copy the paho mqtt dlls to the executable's location:
```
cp ../lib/paho.mqtt.c/bin/paho-mqtt3a.dll Debug/
cp ../lib/paho.mqtt.cpp/bin/paho-mqttpp3.dll Debug/
```
