# 03\_data\_annotation.md

\# 3. 使用LabelImg进行数据标注



\## 3.1 安装LabelImg



在你的 `yolo_env` 环境中安装：

输入

**pip install labelImg**



3.2 启动与使用



安装后，直接输入命令启动

**labelImg**



详细步骤：



准备工作：组织好 images 和 labels 文件夹。



yolo_project  (你的项目根目录，名字可自取)

            ├── datasets

            │   └── cars  (整个数据集文件夹)

            │       ├── images

            │       │   ├── train  (训练集图片)

            │       │   └── val    (验证集图片)

            │       └── labels

            │           ├── train  (训练集标注)

            │           └── val    (验证集标注)

            └── car_dataset.yaml  (数据集配置文件)

📝 第二步：分配你的图片

由于本项目图片极少，我们采用“留一法”进行验证，这在小样本学习中是常见的策略。



选择4张图片及其对应的标注文件（.txt），放入：



.../images/train/ 文件夹



.../labels/train/ 文件夹



选择1张图片及其对应的标注文件，放入：



.../images/val/ 文件夹



.../labels/val/ 文件夹



注意：确保图片文件（.jpg）和标注文件（.txt）文件名必须完全相同（只有后缀不同）。

例如：image_001.jpg 对应的标注文件必须是 image_001.txt。



打开目录：启动LabelImg后，点击 Open Dir 选择包含图片的 images 文件夹。



设置保存目录：点击 Change Save Dir，选择准备好的 labels 文件夹。这一步至关重要！



设置格式：在右侧点击切换格式为 YOLO。



开始标注：



按下键盘快捷键 W，调出标注框。



鼠标拖拽框出目标物体。



在弹出的对话框中输入类别名称（如 car），点击OK。



标注完一张图片后，按 Ctrl + S 保存，这会自动在 labels 文件夹生成同名的 .txt 文件。



按 D 下一张，A 上一张。

