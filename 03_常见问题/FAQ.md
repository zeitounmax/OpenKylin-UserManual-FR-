# FAQ
> 这个页面是由Docs SIG和FAQ SIG共同维护的问答列表，广泛收集大家的问题以及问题解决方案，欢迎贡献！

## 安装apk包
- Q：可以安装自己下载的apk包么？
- A：可以，将apk下载到本地后，右键点击，打开方式选择安装器。

## 如何升级openKylin

- Q：如何升级openKylin？
- A：可以执行以下命令

```
sudo apt update
sudo apt full-upgrade
```

## 快速检查电脑是否配备USB 3.0

- Q：越来越多的电脑开始配备USB 3.0端口，但是如何知道您的电脑是否带有3.0端口以及哪个端口是3.0呢？
- A：打开终端，使用以下命令：

```
lsusb
```

该命令显示系统中的USB相关信息，检查结果，如果发现如下显示“3.0 root hub”字样，表明系统带有USB 3.0。

```
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```


- Q：如何识别哪个端口为USB 3.0呢？
- A: 通常USB 3.0端口带有SS标记（Super Speed的缩写）。如果硬件厂商没有标记SS或USB 3，则检查端口内部的颜色，USB 3.0通常为蓝色。


在Linux系统中，查看硬件信息是一项常见的任务，可以帮助你了解系统的配置，进行故障诊断或性能优化。以下是一些常用的Linux命令，用于查看各种硬件信息：

## 如何查看CPU信息

- Q：如何查看电脑的CPU信息？
- A：通常可以在设置-关于界面查看CPU型号信息，如果需要查看CPU详细信息，可以通过`lscpu`命令或者`cat /proc/cpuinfo`命令来查看CPU架构（如x86_64）、型号、核心数、线程数、每个核心的CPU频率、缓存、支持的指令集等信息。

## 如何查看内存信息

- Q：如何查看电脑的内存信息？

- A：可以使用`cat /proc/meminfo`命令来查看内存信息，包括总内存、可用内存、缓存使用情况等，其中MemTotal就是内存的总容量。如果需要查看更多信息可以通过`sudo lshw -class memory`命令来查看详细信息，以下是一个输出示例。

```bash
  *-memory
       description: System Memory
       physical id: 21
       slot: System board or motherboard
       size: 32GiB
     *-bank:0
          description: SODIMM DDR4 Synchronous Unbuffered (Unregistered) 3200 MHz (0.3 ns)
          product: CT16G4SFRA32A.C8FF
          vendor: Unknown
          physical id: 0
          serial: E68AC1FD
          slot: DIMM 0
          size: 16GiB
          width: 64 bits
          clock: 3200MHz (0.3ns)
     *-bank:1
          description: SODIMM DDR4 Synchronous Unbuffered (Unregistered) 3200 MHz (0.3 ns)
          product: CT16G4SFRA32A.C8FF
          vendor: Unknown
          physical id: 1
          serial: E68AC2D5
          slot: DIMM 0
          size: 16GiB
          width: 64 bits
          clock: 3200MHz (0.3ns)

```
## 如何查看 Linux 硬盘信息
- Q：如何查看 Linux 硬盘大小、类型和硬件信息
- A：可以通过`sudo lshw -class disk`命令，即可输出硬盘的详细信息，例如描述、产品类型、供应商、总线信息、版本和大小。

     可以通过`lsblk`即可在Linux中查看文件系统类型。

     可以通过`sudo fdisk -l`即可查看磁盘类型和大小、磁盘机型、扇区大小和其他附加信息。

     可以通过`hwinfo --disk`即可在Linux系统中查看硬碟硬件信息。
