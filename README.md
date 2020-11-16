# EasyFlash

[中文页](README_ZH.md) | English

[![GitHub release](https://img.shields.io/github/release/armink/EasyFlash.svg)](https://github.com/armink/EasyFlash/releases/latest) [![GitHub commits](https://img.shields.io/github/commits-since/armink/EasyFlash/4.1.0.svg)](https://github.com/armink/EasyFlash/compare/4.1.0...master) [![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/armink/EasyFlash/master/LICENSE)

## 1. Introduction

[EasyFlash](https://github.com/armink/EasyFlash) is an open source, lightweight embedded Flash memory library, which facilitates developers to easily develop common applications based on Flash memory. It is very suitable for products that require power-off storage functions such as smart homes, wearables, industrial control, medical, Internet of Things, etc., with extremely low resource consumption and support for various MCU on-chip memories. The library mainly includes **three useful functions**:

- **ENV** quickly save product parameters, support **write balance (wear balance)** and **power failure protection** functions

EasyFlash can not only realize the power-down saving function of the product's **set parameters** or **run log** information, but also encapsulates the concise **add, delete, modify and query** methods, which reduces development The difficulty of processing product parameters also ensures that the product has better scalability during later upgrades. Turn Flash into a small key-value storage database of the NoSQL (non-relational database) model.

- **IAP** Online upgrade is no longer difficult

The library encapsulates the commonly used interface of IAP (In-Application Programming) function, supports CRC32 check, and supports the upgrade of Bootloader and Application.

- **Log** No file system required, logs can be stored directly on Flash

It is very suitable for application in small products without file system, which is convenient for developers to quickly locate and find the cause of system crash or crash. At the same time with [EasyLogger](https://github.com/armink/EasyLogger) (my open source ultra-lightweight, high-performance C log library, it provides a seamless interface with EasyFlash), easy to achieve C log Flash storage function.

### 1.1, V4.0 NG mode

Since the Spring Festival in 2019, EasyFlash has been iterating for more than 4 years, combining the needs and suggestions of many developers, and finally released the V4.0 version. The ENV function in this version is named **NG** (Next Generation) mode , This is a completely refactored new version with the following new features:

- Smaller resource footprint, memory footprint **almost 0**; (Versions before V4.0 will use additional RAM space for cache)
- The value type of ENV supports **any type**, any length, which is equivalent to direct memcpy variable to flash; (only supports storing strings before V4.0)
- ENV operation efficiency is higher than the previous mode, the remaining free area is fully utilized, and the number of erasing and operation time are significantly reduced;
- **Native support** wear leveling, power-down protection function (extra Flash sector is required before V4.0);
- ENV supports **incremental upgrade**, ENV also supports upgrade after firmware upgrade;
- Support large data storage mode, **length unlimited**, data can be stored sequentially on multiple Flash sectors. Resources such as script programs and audio that occupy more than 1 sector of Flash can also be stored in ENV (supported in V4.2 soon);
- Support **data encryption**, improve storage security, a must-have function in the Internet of Things era (coming soon in V4.3);
- Support **data compression** to reduce Flash usage (coming soon in V4.4);

V4.0 design and internal principles, V4.0 migration guide and more, please continue to read the following [document chapter](#2. Documentation)

### 1.2, resource occupation

```
Minimum requirements: ROM: 6K bytes RAM: 0.1K bytes
```

## 2. Documentation

- Porting instructions based on RT-Thread: [`ports/README.md`](ports/README.md)
- API documentation: [`docs/zh/api.md`](docs/zh/api.md)
- General porting document: [`docs/zh/port.md`](docs/zh/port.md)
- V4.0 Migration Guide: [`\docs\zh\v4_migrate.md`](/docs/zh/v4_migrate.md)
- V4.0 ENV function design and implementation: [`\docs\zh\design.md`](/docs/zh/design.md)

Be sure to **read the document** before porting.

## 3. Configuration instructions

```shell
[*] ENV: Environment variables
[*] Auto update ENV to latest default when current ENV version number is changed.
(0) Setting current ENV version number
[*] LOG: Save logs on flash
(262144) Saved log area size. MUST be aligned by erase minimum granularity
[*] IAP: In Application Programming
(4096) Erase minimum granularity
      Write minimum granularity (1bit such as Nor Flash) --->
(0) Start addr on flash or partition
[*] Enable debug log output
```

- `ENV: Environment variables`: Whether to enable the environment variable function
  - `Auto update ENV to latest default when current ENV version number is changed.`: Whether to enable automatic update of environment variables. After starting this function, the environment variable will be automatically updated when its version number changes.
    - `Setting current ENV version number`: current environment variable version number
- `LOG: Save logs on flash`: log function, you can save the log sequence to Flash. It can also cooperate with EasyLogger to complete the power-down storage of product logs.
- `IAP: In Application Programming`: IAP online upgrade function, after opening it will provide some commonly used APIs in IAP functions.
- `Erase minimum granularity`: The minimum granularity of erasing, the general SPI Flash is usually 4KB, and the STM32F4 on-chip Flash is usually 128KB.
- `Write minimum granularity`: The minimum granularity of writing data. Generally, SPI Flash is usually 1bit, and STM32F4 on-chip Flash is usually 8bit. Please refer to the specific options for details.
- `Start addr on flash or partition`: The offset address of the entire storage area of ​​EasyFlash relative to Flash or partition, depending on the transplant code.
- `Enable debug log output`: Whether to enable debug log output. After opening, you will see more debug log information.

## 4. Support

![support](./docs/zh/images/wechat_support.png)

If EasyFlash solves your problem, you might as well scan the QR code above and ask me **a cup of coffee**~