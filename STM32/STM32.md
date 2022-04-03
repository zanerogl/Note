#  STM32

### 简介

- STM32系列是由意法半导体公司推出的ARM Cortex-M内核单片机，从字面上来看，ST为意法半导体公司的缩写，M是Microcontrollers即单片机的缩写，32代表32位

-----

### 芯片系列

- ![Chip_series0](./STM32.assets/Chip_series0.png)
- ![Chip_series1](./STM32.assets/Chip_series1.png)

----

### 命名规则

- ![Suffix_model_description](./STM32.assets/Suffix_model_description.png)

-----

### 使用的芯片

***STM32F103C8T6***

- 系列：主流系列STM32 F1
- 内核：ARM Cortex-M3
- 主频：72 MHz
- RAM（随机存取存储器，运行内存）：20 K (SRAM)
- ROM（程序存储器）：64 K (Flash)
- 供电：2.0-3.6V(标准3.3V)
- 封装：LQFP48 （指的是芯片有48个引脚）

----

### 外设资源

- ![Peripherals](./STM32.assets/Peripherals.png)

----

### 启动配置

- ![BOOT](./STM32.assets/BOOT.png)

----



























































### GPIO库函数

- **初始化**

  ```c
  void GPIO_Init (GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct);
  ```