**提示：文档部分由 ChatGPT 生成。**

# Process Source Tracer

[English](./README.md) | 简体中文

该项目是一个用于在 Windows 递归查询进程创建者的实用工具，类似 Linux 的 `ps` CLI。通过追踪进程的父进程链，它可以帮助你确定一个进程的创建者及其上层创建者，以便判断进程是否友善并符合你的期望。

## 用途

这个工具可以用于以下情况：

- 查找一个进程的起源，了解它是由谁创建的。
- 追踪进程的上层创建者，了解进程的整个创建链。
- 判断进程是否友善，是否符合你的预期。

## 如何使用

在 `cmd` 中使用该工具的步骤如下：

1. 克隆本项目到本地或下载项目的压缩包。
2. 打开 `cmd`，切换到项目所在的目录。
3. 运行 `tracepid.cmd` 命令并传递目标进程的 PID 作为参数。例如：`tracepid.cmd <PID>`。
4. 工具将会递归查询目标进程的创建者，并显示创建链上的进程信息。
5. 根据输出信息，判断进程是否友善并符合你的期望。

## 用例

```shell
# 显示进程列表与详细信息
tasklist /V

# 对怀有疑问的 PID 进行查询
tracepid 10101
```

或者完成某些操作

```shell
http-server -p 8081
# Error: listen EADDRINUSE: address already in use 0.0.0.0:8081

netstat -ano | findstr 8081
# TCP    0.0.0.0:8081           0.0.0.0:0              LISTENING       38920

tasklist /V | findstr 38920
# node.exe    38920 Console    1    29,608 K    Unknown    DESKTOP-86L4P1F\Administrator    0:00:00    暂缺

tracepid 38920
# Parent Process ID: 26484
# Executable Path:   C:\Program Files\nodejs\node.exe
# Parent Process ID: 5220
# Executable Path:   C:\windows\system32\cmd.exe
# Parent Process ID: 3724
# Executable Path:   C:\Program Files\nodejs\node.exe
# 没有可用实例。
# 
# Parent Process ID: 3724
# Executable Path:   C:\Program Files\nodejs\node.exe
# Reached the root process.
# End of the recursion.

taskkill /f /t /pid 38920
# 成功: 已终止 PID 38920 (属于 PID 26484 子进程)的进程。
```

请注意，该工具使用了批处理脚本编写，可能会受到一些限制。在某些情况下，可能需要使用更高级的系统工具和编程语言来实现更详细和准确的分析。
