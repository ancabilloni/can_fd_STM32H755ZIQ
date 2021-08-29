# CAN-FD STM32 H755ZIQ

This is an example of how to transmit and receive CAN-FD frame with STM32H7 (NUCLEO H755ZI-Q). 

NUCLEO H755ZI-Q comes with two CAN-FDs. However, we need to connect an external transceiver to be able to communicate. For this project, I used the [SK Pang transceiver breakout board](https://copperhilltech.com/can-bus-can-fd-breakout-board-5vdc/) with CAN-FD capable. 

Wiring
-----

|Transceiver|Nucleo|
|:---: |:---:|
|GND| GND (CN8, Pin 11)|
|5V | 5V (CN8, Pin 9)|
|TX | CAN_TX (CN9, Pin 27)|
|RX | CAN_RX (CN9, Pin 25)|

The transceiver breakout comes with 120 ohm terminating transistor. To use this transistor, add a jumper on JP3.

Testing
----
On the PC side, I use the [PEAK CAN-FD USB](https://www.amazon.com/CAN-USB-FD-Adapter-GC-CAN-USB-FD/dp/B078VQ37YY/ref=sr_1_1?dchild=1&keywords=peak+can+fd&qid=1630224224&sr=8-1) with socketcan on Linux or PCAN Viewer if on Windows. 

To setup socketcan on Linux:

```bash
# Nominal rate 500 Kbits/s, Data rate 2 Mbits/s
sudo ip link set can0 type can tq 25 prop-seg 34 phase-seg1 35 phase-seg2 10 sjw 8 dtq 25 dprop-seg 7 dphase-seg1 8 dphase-seg2 4 dsjw 4 fd on
sudo ip link set up can0
```

In case you notice that I do not use an easier way to setup socketcan such as: 
```bash
sudo ip link set can0 type can bitrate 500000 dbitrate 2000000 fd on
```
Well, I did but this one didn't work for some reason. It may have something to do with the correctness of bit timing setting on the Nucleo board.
