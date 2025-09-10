# 04\_training\_guide.md

\# 4. 模型训练命令全解析



在激活的 `yolo\\\_env` 环境下，进入你的项目目录。



输入

**conda activate yolo\_env**

**cd "你的项目目录地址"(例如cd D:\\yolo\_project)**



4.1 基础训练命令

**yolo task=detect mode=train model=yolov8n.pt data=car\_dataset.yaml epochs=100 imgsz=640**

参数逐行解释：



yolo: 调用 Ultralytics YOLO 库。



task=detect: 设置任务为目标检测。



mode=train: 设置模式为训练。



model=yolov8n.pt: 指定使用的模型。n 代表 Nano（最小、最快），其他选项有 s(small), m(medium), l(large), x(xlarge)，模型越大通常精度越高但速度越慢。



data=car\_dataset.yaml: 指定数据集配置文件的路径。



epochs=100: 训练轮数。整个数据集被完整遍历一次称为一个epoch。



imgsz=640: 输入图像的尺寸。图像会被缩放和填充至 640x640 像素。增大尺寸可能会提升精度但会显著增加显存消耗和训练时间。



4.2 小样本过拟合策略

当数据量极少时（如5张），我们需要调整策略，让模型“记住”而非“学习”。



输入

**yolo task=detect mode=train model=yolov8n.pt data=car\_dataset.yaml epochs=500 imgsz=1280 batch=1 device=cpu patience=500 lr0=0.01**

新增参数解释：



batch=1: 批量大小设置为1。即每看一张图片就更新一次模型权重，更容易“记住”单张图片。



device=cpu: 指定使用CPU进行训练，避免GPU兼容性问题。



patience=500: 早停机制的耐心值。设置一个非常大的值（等于epochs），实质上是禁用早停，强制模型训练满500轮。



lr0=0.01: 初始学习率。适当提高学习率，让模型调整权重的步伐更大，更快地拟合训练数据。



（imgsz=1280在测试阶段，发现待测图片中的目标物体尺度远小于训练样本。针对此‘小目标检测’挑战，本项目尝试了多种方案，最终发现通过增大模型推理时的输入分辨率（从640px提升至1280px），可显著提升小目标的识别率。）

