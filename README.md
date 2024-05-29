# irobotEC [WIP]

[![build](https://github.com/IRobot-EC-2024/irobotEC/actions/workflows/ci_build.yml/badge.svg)](https://github.com/IRobot-EC-2024/irobotEC/actions/workflows/ci_build.yml)
[![clang-format Check](https://github.com/IRobot-EC-2024/irobotEC/actions/workflows/style_check.yml/badge.svg)](https://github.com/IRobot-EC-2024/irobotEC/actions/workflows/style_check.yml)
[![docs](https://github.com/IRobot-EC-2024/irobotEC/actions/workflows/doxygen-gh-pages.yml/badge.svg)](https://github.com/IRobot-EC-2024/irobotEC/actions/workflows/doxygen-gh-pages.yml)
[![CodeFactor](https://www.codefactor.io/repository/github/lunarifish/irobotec/badge)](https://www.codefactor.io/repository/github/lunarifish/irobotec)

用于统一底层代码和少量通用业务代码的电控库。 项目基于C++20/C11标准，遵守[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)[[中文](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents.html)]
。

**NOTE: 本项目使用GNU ARMGCC工具链开发，用其他工具链编译可能不通过**

## 文档

文档可以自行使用Doxygen构建，也可以在[这里](https://irobot-ec-2024.github.io/irobotEC/)查看。

```shell
doxygen ./Doxyfile
```

正确构建后，文档会被放在`docs/`文件夹下。

## 使用方法

1. Clone仓库（包括子模块），在CMakeLists.txt里添加为子目录并且把`irobotEC`静态库链接到主目标：

    ```shell
    git clone --recursive https://github.com/IRobot-EC-2024/irobotEC.git
    ```

    ```cmake
    add_subdirectory(<仓库路径>)
    target_link_libraries(<目标> PUBLIC irobotEC)
    ```

2. 在CMakeLists.txt里添加对应芯片型号的预处理宏定义（注意检查是否已经自动生成过）

    ```cmake
    add_definitions(-DSTM32F407xx)
    # add_definitions(-DSTM32H723xx)
    ```

   可选项：
    - [x] STM32F407xx
    - [x] STM32H723xx

3. 启用UART、CANFD、CAN、I2C、SPI的Register Callback功能，在CubeMX中的配置项位置如下图：

   ![](https://img2.imgtp.com/2024/04/10/AUnAPRby.png)

4. 包含头文件，按需使用

    ```cpp
    #include "device/motor/dji_motor.hpp"
    // #include ...
    ```

## 项目结构/开发进度

未打钩的复选框表示还在开发中。

- `core/`: 支撑库运行的核心功能

- `examples/`：例程

- `libs/`：第三方库

- `src/`

    - `device/`：设备驱动和封装
        - `actuator/`：作动器，如电机和舵机等
            - [ ] `unitree_motor`：宇树电机
        - `remote/`：遥控器/接收机
        - `sensor/`：传感器
            - [ ] `icm42688p/`：[ICM42688P IMU](https://product.tdk.com.cn/system/files/dam/doc/product/sensor/mortion-inertial/imu/data_sheet/ds-000347-icm-42688-p-v1.6.pdf)
        - `supercap/`：超级电容
        - [ ] `referee/`：裁判系统

    - `hal/`：基于STM32 HAL库封装的C++类库
      - [ ] `fdcan`
      - [ ] `i2c`
      - [ ] `spi`

    - `modules/`：软件模块
        - `algorithm/`：常用算法

## 开发环境

- STM32CubeMX `6.10.0`
    - STM32Cube MCU Package for STM32F4 series `1.28.0`
    - STM32Cube MCU Package for STM32H7 series `1.11.1`
- [GNU Arm Embedded Toolchain, AArch32 bare-metal target (arm-none-eabi)](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads) `13.2.Rel1`
