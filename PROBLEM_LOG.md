# PROBLEM\_LOG.md

\# 项目问题排查日志



本项目是初学者搭建YOLOv8环境的真实记录，涵盖了从零开始遇到的大部分典型问题及其解决方案。



---



\## 🚧 问题 1: Anaconda 安装环境冲突



\-   \*\*时间\*\*: 2025年9月7日

\-   \*\*环境\*\*: Windows 11

\-   \*\*现象\*\*:

    安装Anaconda时，弹窗提示：`A Python installation has been found on your system at C:\\\\\\\\fakeD\\\\\\\\python\\\\\\\\python3\\\\\\\\...`

\-   \*\*原因分析\*\*:

    系统已存在一个非Anaconda管理的Python环境，可能与新安装的Anaconda产生冲突。

\-   \*\*解决方案\*\*:

    1.  点击 \*\*Cancel\*\* 取消当前安装。

    2.  进入`设置` -> `应用` -> `应用和功能`，找到并卸载旧的Python。

    3.  如果找不到，直接去文件资源管理器删除提示中的目录（`C:\\\\\\\\fakeD\\\\\\\\python\\\\\\\\`）。

    4.  重新运行Anaconda安装程序，顺利通过。

\-   \*\*根本原因\*\*: 系统环境变量混乱。

\-   \*\*预防措施\*\*: 一台电脑最好只通过一个工具（如Anaconda）管理Python环境。



\## 🚧 问题 2: PyTorch 与 NVIDIA 新显卡兼容性问题



（如果你通过Pytorch官网下载PyTorch,下载界面官网为您生成的命令是：

**pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu126**

如果你通过这个指令下载PyTorvh,那么你可能会遇到这个问题）



\-   \*\*时间\*\*: 2025年9月8日

\-   \*\*环境\*\*: Conda `yolo\\\\\\\_env`, RTX 5060

\-   \*\*错误信息\*\*:

    `torch.AcceleratorError: CUDA error: no kernel image is available for execution on the device`

\-   \*\*现象\*\*:

    `torch.cuda.is\\\\\\\_available()` 返回 `True`，但一开始训练就报错。

\-   \*\*原因分析\*\*:

    RTX 5060基于最新的Ada Lovelace架构（计算能力sm\_89+），而我安装的稳定版PyTorch 2.8.0仅预编译支持到sm\_90（Ampere架构）。

\-   \*\*解决方案\*\*:

    \*\*临时方案（采用）\*\*：在训练命令后添加 `device=cpu` 参数，使用CPU进行训练。对于极小的数据集，速度可以接受。

    输入

    **yolo task=detect mode=train ... device=cpu**

    ```

    \*\*永久方案\*\*：卸载稳定版PyTorch，安装PyTorch Nightly预览版（支持新架构）。

    输入

    **pip3 install --pre torch torchvision --index-url https://download.pytorch.org/whl/nightly/cu121**

\-   \*\*总结\*\*: 硬件更新速度有时会快于软件生态的适配速度。



**以下问题解决方法较简单或前文有所提及，不再展开叙述**

| \*\*3. 训练时字体下载失败\*\* | `ConnectionError: Download failure for https://ultralytics.com/assets/Arial.ttf` | 网络无法访问Ultralytics海外服务器 | 在运行训练命令前，在终端执行 `\\\*\\\*set ULTRALYTICS\\\\\\\_FONT=arial.ttf\\\*\\\*`，强制使用系统字体。 |

| \*\*4. 小样本过拟合\*\* | 模型在训练集上损失下降，但在新图片上无法预测出任何结果 | 训练数据量极少（仅5张），模型无法学习泛化特征 | \*\*调整策略\*\*：承认泛化不现实，转而追求“过拟合”。<br>\*\*方案\*\*：大幅增加训练轮数(`epochs=500`)，减小批量大小(`batch=1`)，关闭早停(`patience=500`)，让模型强行记住训练样本。 |

| \*\*5. 小目标检测失败\*\* | 对于远处的小车，模型无法检测 | 目标在图像中占比太小，分辨率不足，特征不明显 | \*\*方案\*\*：预测时使用高分辨率推理，添加 `imgsz=1280` 参数，将图片放大后再进行预测。 |

6.不小心关闭了Anaconda prompt

✅ 解决方案

重新打开 Anaconda Prompt



在Windows开始菜单里找到并点击 “Anaconda Prompt”。



重新激活你的工作环境





输入以下命令，即可回到你之前的 yolo\_env 环境：



**conda activate yolo\_env**

成功之后，命令行的前缀会从 (base) 变回 (yolo\_env)。



🔍 验证一下（可选）

为了确保万无一失，你可以验证一下PyTorch和GPU是否依然可用：



**python -c "import torch; print('环境:', 'yolo\_env'); print('GPU可用:', torch.cuda.is\_available())"**

💡 核心要点记住

(base)： 是Anaconda的默认基础环境，我们一般不直接在里面干活。



(yolo\_env)： 是你为自己项目创建的“独立工作间”，所有工具（PyTorch, YOLOv8）都安装在这里。以后每次开工前，都需要先执行 conda activate yolo\_env 进入这个工作间。

