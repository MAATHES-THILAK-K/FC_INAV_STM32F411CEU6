# **Custom STM32F411CEU6 Flight Controller (INAV 6.1 Compatible)**

A fully custom, hand-built flight-controller based on the **STM32F411CEU6 (WeAct BlackPill)**, designed to run **INAV 6.1** with support for quadcopter/plane builds, custom SBUS transmitter, and modular sensor expansion.

This project documents the **hardware design**, **pinouts**, **INAV configuration**, and the **custom SBUS TX** created using ESP32.

> ⚠️ **Reference & Credit:**
> This project is heavily inspired by and based on the original INAV firmware adaptations by **rizacelik**, available here:
> **[https://github.com/rizacelik/STM32F411CEU6_INAV_Firmware](https://github.com/rizacelik/STM32F411CEU6_INAV_Firmware)**
> Massive respect to the author for enabling INAV support on custom F411 boards.

---

## **📸 Hardware Overview**

Your flight controller is built using commonly available modules, combined into a compact FC board:

### **🧠 Microcontroller**

* **STM32F411CEU6** (WeAct BlackPill board)
* ARM Cortex-M4 @ 100 MHz
* 512 KB Flash, 128 KB RAM
* Onboard USB for flashing/debug

### **🌀 Sensors**

* **MPU6050** — Gyroscope + Accelerometer (I²C)
* **BMP180** — Barometer (I²C)

### **⚡ Power System**

* Integrated buck converter
* Accepts **up to 24V** (battery input)
* Regulated **5V output** for MCU + peripherals

### **🛫 Outputs**

* **5× Motor PWM outputs**
* Supports Quad/Hex (expandable via future versions)
* **Dedicated UART/SPI/I²C pads** for:

  * GPS
  * Magnetometer (HMC5883, QMC5883, IST8310)
  * Airspeed sensor
  * Blackbox logging
  * Optional SPI IMU upgrade (future)

### **📡 Communication**

* Custom **ESP32 SBUS Transmitter**
* 16-channels over a single wire
* Fully compatible with INAV receiver setup
* (TX Code will be released soon)

---

## **🌐 Firmware**

This FC runs **INAV 6.1**, tested using:

* **INAV Configurator 6.1**
* **Firmware Base:**
  [https://github.com/rizacelik/STM32F411CEU6_INAV_Firmware](https://github.com/rizacelik/STM32F411CEU6_INAV_Firmware)

---

## **📂 Repository Structure**

```
Custom-STM32F411-FC/
│
├── Firmware/
│   ├── prebuilt_firmware.bin
│
├── Images/
│   ├── fc_front.jpg 
│   ├── Pinout_Diagram.png
|
└── README.md
```

---

## **📌 Pinout Summary**

### **I²C Bus (IMU & Barometer)**

| Sensor | Pin      | MCU Pin |
| ------ | -------- | ------- |
| SDA    | I2C1_SDA | PB9     |
| SCL    | I2C1_SCL | PB8     |

### **Motor Outputs**

(Example mapping, adapt based on your routing)

| Motor | MCU Pin | Timer    |
| ----- | ------- | -------- |
| M1    | PA0     | TIM2_CH1 |
| M2    | PA1     | TIM2_CH2 |
| M3    | PA2     | TIM2_CH3 |
| M4    | PA3     | TIM2_CH4 |
| M5    | PB0     | TIM3_CH3 |

### **UARTs**

| Purpose    | TX  | RX   |
| ---------- | --- | ---- |
| GPS        | PA9 | PA10 |
| SBUS Input | —   | PA15 |

### **SPI (Future Expansion)**

| Pin  | MCU           |
| ---- | ------------- |
| SCK  | PB3           |
| MISO | PB4           |
| MOSI | PB5           |
| CS   | any free GPIO |

---

## **🛠️ Building the Firmware**

1. Download INAV source
2. Add/modify the `"STM32F411CEU6"` target
3. Configure:

   * IMU = MPU6050
   * BARO = BMP180
   * TARGET_MCU = F411
4. Compile using:

```
make TARGET=STM32F411CEU6
```

---

## **📡 ESP32 SBUS Transmitter (Custom TX)**

A dedicated ESP32-based transmitter sending **16 SBUS channels** using a single wire.

Features:

* Supports stick, switch, pot inputs
* SBUS output at 100Kbps inverted
* Works directly with INAV
* Will be added soon to this repo

---

## **📦 Included Files**

* ✔️ Precompiled INAV `.bin`
* ✔️ Wiring diagram
* ✔️ Board images (front)
* ✔️ SBUS TX source (coming soon)

---

## **🚀 Future Add-Ons**

* SPI IMU (ICM42605/ICM20602)
* OSD integration
* Logging via SPI Flash or SD card
* Full custom INAV target upstream PR
* Custom PCB design

---

## **🙏 Credits**

Special thanks to **rizacelik** for the original STM32F411 INAV firmware adaptation:
[https://github.com/rizacelik/STM32F411CEU6_INAV_Firmware](https://github.com/rizacelik/STM32F411CEU6_INAV_Firmware)

This project extends and re-implements the concept using custom hardware and additional peripherals.

