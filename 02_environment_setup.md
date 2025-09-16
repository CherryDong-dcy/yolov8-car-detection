# 02\_environment\_setup.md

\# 2. 创建Python环境与安装依赖



**所有命令均在Anaconda Prompt中执行。



\## 2.1 创建并激活Conda环境



我们创建一个独立的环境，名为 `yolo_env`。





\\# 创建一个名为 yolo_env 的新环境，并安装 Python 3.9
输入
conda create -n yolo_env python=3.9



命令解释：



conda create -n：创建一个新环境。



yolo_env：环境名称，可以自定义。



python=3.9：指定环境中Python的版本。



执行命令后，会提示 Proceed ([y]/n)?，直接按 y 回车确认。



# 激活刚刚创建的环境

输入

conda activate yolo_env

命令解释：



conda activate：激活一个环境。激活后，命令行的前缀会从 (base) 变为 (yolo\\\_env)。这意味着你后续安装的所有库都会被隔离在这个环境中。



2.2 安装 PyTorch

安装PyTorch：

官网会生成一条命令，大概率类似于下面这样（但请以官网生成的最新命令为准）：


conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
将这条命令复制到你的 Anaconda Prompt 中运行。

2.3 安装 YOLOv8

\\# 安装 Ultralytics 库，其中包含了 YOLOv8

输入

pip install ultralytics



2.4 验证安装

让我们检查所有库是否安装成功，并确认环境无误。

\\# 进入Python交互界面

输入

python



\\# 在打开的Python环境中，逐行输入以下代码

```
import torch

import ultralytics

print(torch.__version__)

print(ultralytics.__version__)

exit()
```


如果所有命令都没有报错，则说明环境配置成功！


