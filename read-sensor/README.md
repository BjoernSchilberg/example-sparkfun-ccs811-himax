# `read-sensor` firmware

This directory contains the source code to read values from the SparkFun
*Environmental Combo Breakout - CCS811/BME280 (Qwiic) board* and print values
to the serial port.

To flash the firmware:

```shell
himax-flash-tool --firmware-path image_gen_linux/out.img
```

Data can then be forwarded to the Edge Impulse project using the [Data forwarder](https://docs.edgeimpulse.com/docs/cli-data-forwarder), ie:

```shell
edge-impulse-data-forwarder --frequency 2
```

You can also have an overview of the raw data by opening a serial connection
(`baudrate=115200`), before using the [Data
Forwarder](https://docs.edgeimpulse.com/docs/cli-data-forwarder). For example under GNU/Linux:
```shell
stty -f /dev/ttyUSB0 | cat /dev/ttyUSB0
```


If you need to modify the firmware, check out the [main.cc](main.cc), and rebuild/compile with:

```shell
docker build -t read-sensor -f Dockerfile.gnu .
mkdir -p build-gnu
docker run --rm -it -v $PWD:/app read-sensor /bin/bash -c "cd build-gnu && cmake -DCMAKE_TOOLCHAIN_FILE=toolchain.gnu.cmake .."
docker run --rm -it -v $PWD:/app:delegated read-sensor /bin/bash -c "cd build-gnu && make -j && sh ../make-image.sh GNU"
```
