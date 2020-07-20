## Links
[[README](../README.md)]
[[EWF win7 step by step](<../doc/ewf-win7-step-by-step.md>)]

# EWF win7 step by step

本文件只适用 Windows 7（非 Embedded 版本），进行开启 EWF 保护。

EWF 参考

> [https://docs.microsoft.com/zh-cn/previous-versions/windows/embedded/ms912906(v=winembedded.5)](https://docs.microsoft.com/zh-cn/previous-versions/windows/embedded/ms912906(v=winembedded.5))

## 启用 EWF 的一般步骤

1. 安装相应系统类型的 EWF，32位和64位的 Windows 7，只选择其中一个进行安装。
2. 启用 EWF。
3. 查看 EWF 状态，确认已经开启。
4. 对被 EWF 保护的磁盘进行测试，写入文件，并重启系统，确认没有问题。

## 详细说明

EWF for Windows 7

```
*************************************
*           EWF配置工具             *
*         v1.0 by EECTRL            *
*************************************
*                                   *
*  [1]: 安装windows 7 x86 EWF       *
*  [2]: 安装windows 7 x64 EWF       *
*  [3]: 启用EWF                     *
*  [4]: 提交并停用EWF               *
*  [5]: 提交写入内容                *
*  [6]: 查看EWF保护状态             *
*  [7]: 重启系统                    *
*  [8]: 退出                        *
*                                   *
*************************************
请选择:
```

### [1] 安装windows 7 x86 EWF

在 Windows 7（**32位**）操作系统下安装 EWF。非 Windows 7 嵌入式版本的 win 7 要启用 EWF，首先需要安装 EWF。

### [2] 安装windows 7 x64 EWF

在 Windows 7（**64位**）操作系统下安装 EWF。非 Windows 7 嵌入式版本的 win 7 要启用 EWF，首先需要安装 EWF。

*P.S. [[1]](<#1-安装windows-7-x86-ewf>) 与 [[2]](<#2-安装windows-7-x64-ewf>) 的主要区别在于，一个是32位，另一个是64位的 Windows 7，只要选择期中之一安装即可*

### [3] 启用EWF

为指定盘符，或者所有磁盘，启用 EWF 保护。

启用后
- 对所保护的磁盘进行的写操作、或者删除文件操作，在系统重启之后，全部将被重置，即，重启之后，全部数据恢复为保护后的节点上。
- 如：
    1. 用户在 C: 盘上安装了 Windows 7，并把驱动、和使用环境都配置好；
    2. 为了防止误操作，或者意外断电，使得该 Windows 7 不能正常使用，该用户为 C: 盘启用了EWF；
    3. 用户对 C: 盘做了一些野蛮操作，如删除系统文件、随意把系统文件移到别的目录，甚至直接给电脑断电（提前是断电没有对磁盘设备进行破坏）；
    4. 当用户重新启动该 Windows 7 时，系统一如保护后的状态，C: 盘没有文件被删除，没有文件被移动。

*P.S. 如果是刚安装 EWF 的，在启用 EWF 前，需要先重启 Windows 7，才能正常启用 EWF。启用后，也需要重启 Windows 7，才能正常保护。如果启用 EWF 后没重启 Windows 7，此时对磁盘的所有写操作，都是生效的，EWF 此时并没有保护磁盘。*

### [4] 提交并停用EWF

为指定盘符，或者所有磁盘，停用 EWF 保护，在停用前，会先把磁盘的一些写操作[提交](<#5-提交写入内容>)了。

*P.S. 此操作包括了把现有的磁盘写操作，进行提交，也就是说，如果之前进行了一些有可能对系统产生破坏的写操作的话，建议先重启 Windows 7，让 EWF 恢复之前的文件状态，再进行停用 EWF 操作。*

### [5] 提交写入内容

为指定盘符，或者所有磁盘，提交所有写操作。操作的结果是，EWF 将会对现有的磁盘数据进行保护。

*P.S. 如果之前进行了一些有可能对系统产生破坏的写操作的话，不建议进行此操作。*

### [6] 查看EWF保护状态

查看 EWF 当前的状态

```
Protected Volume Configuration
  Type            RAM (REG)
  State           DISABLED
  Boot Command    NO_CMD
    Param1        0
    Param2        0
  Volume ID       19 03 B5 10 00 00 50 06 00 00 00 00 00 00 00 00
  Volume Name     "\\?\GLOBALROOT\Device\HarddiskVolume2" [C:]
  Max Levels      1
  Clump Size      512
  Current Level   N/A

  Memory used for data 0 bytes
  Memory used for mapping 0 bytes
```

- **State** - 当前状态
    - **DISABLED** - 未保护状态
    - **ENABLED** - 已保护状态，即系统重启后，所有数据将被恢复
- **Boot Command** - 重启后将采取的命令
    - **NO_CMD** - 没有命令
    - **ENABLED** - EWF 将在系统重启后被启用
    - **DISABLED** - EWF 将在系统重启后被停用
- **Volume Name** - 当前磁盘的名称（包括了盘符，如 C:），如果有多个磁盘，这里会有多条信息

