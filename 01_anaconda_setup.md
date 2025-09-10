# 01\_anaconda\_setup.md

\# 1. Anaconda 安装与配置详解



\## 1.1 Anaconda 是什么？

它是什么？ 一个“软件集装箱”。它可以把你的YOLO项目所需要的所有软件包（Python, PyTorch等）打包装在一个独立的“集装箱”里，与电脑上其他Python环境隔离开。这样就不会因为版本冲突而一团糟。



\## 1.2 下载与安装步骤



1\.  \*\*下载\*\*：

    -   打开 \[Anaconda 官网](https://www.anaconda.com/download)

    -   选择适用于 Windows 的 \*\*Python 3.9\*\* 版本安装包。为什么是3.9？因为这是YOLOv8等深度学习库兼容性最好的版本之一，稳定大于新奇。



2\.  \*\*安装\*\*：

    -   运行下载好的安装程序 (`.exe` 文件)。

    -   \*\*安装选项配置（非常重要！）\*\*：

        -   `\[✓] Create shortcuts(suppor ted packages only).\*\*勾选\*\*（创建快捷方式）。

        -   `\\\[ ] Add Anaconda3 to my PATH environment variable`：\*\*不勾选\*\*（避免与系统其他Python冲突）。

        -   `\\\[✓] Register Anaconda3 as my default Python 3.9`：\*\*勾选\*\*（方便VSCode等IDE自动识别）。

        -   `\\\[✓] Clear the package cache upon completion`：\*\*勾选\*\*（清理临时文件，节省空间）。

    -   点击Install，等待安装完成。



3\.  \*\*验证安装\*\*：

    -   安装完成后，在Windows开始菜单中找到并打开 \*\*"Anaconda Prompt (anaconda3)"\*\*。

    -   \*\*注意\*\*：请不要使用普通的CMD或PowerShell，后续所有操作都在Anaconda Prompt中进行。

    -   在打开的窗口中输入 `\*\*conda --version\*\*`，如果显示版本号（如 `conda 24.5.0`），则说明安装成功。

