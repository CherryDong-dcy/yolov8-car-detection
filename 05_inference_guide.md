# 05\_inference\_guide.md

常详细和直观的结果报告。



📁 结果在哪里？

所有训练结果都保存在 runs/detect/ 目录下。每次训练都会创建一个新的子文件夹（例如 train, train2, train3，数字会递增）。

地址会在Anaconda Prompt中输出



🔍 如何查看和理解结果（最重要的部分）

打开上面的文件夹，你会看到很多文件和文件夹。以下是需要重点关注的内容：



1\. 📊 训练指标图表 (results.png 或 results.csv)

这是最核心的分析工具，它展示了训练过程中各项指标的变化趋势。



results.png：一张综合了所有指标的可视化图表。



关注左上角 Box Loss：如果这条曲线稳步下降然后趋于平稳，说明模型正在有效地学习如何定位物体。这是训练成功的最重要标志！



cls\_loss：分类损失，同样应该下降。



val\_loss：验证损失，理想情况下也应该下降。如果它开始上升而训练损失下降，说明模型过拟合了（但对于你只有5张图的情况，这很正常）。



results.csv：包含相同数据的表格文件，可以用Excel打开进行更详细的分析。



2\. 🖼️ 验证结果样本 (val\_batch\*\_labels.jpg 和 val\_batch\*\_pred.jpg)

这些图片让你直观地看到模型在验证集上的表现。



val\_batch\*\_labels.jpg：显示的是真实的标注框（Ground Truth）。



val\_batch\*\_pred.jpg：显示的是模型预测的框。



对比这两张图，你可以看出模型预测的框和真实的框是否吻合。



3\. 🤖 训练好的模型权重 (weights/ 文件夹)

这是你最重要的成果！



best.pt：性能最好的模型权重。根据在验证集上的表现自动选择。



last.pt：训练结束时的最后一个模型权重。



你后续要用自己的图片进行测试时，使用的就是 best.pt 文件。



4\. 📋 模型配置文件 (args.yaml)

保存了本次训练所有的超参数（学习率、批量大小等），用于复现实验结果。



🚀 如何用你的模型进行预测（真正展示结果）

训练的最终目的是用模型识别新的图片。使用以下命令：





**# 基本预测命令**

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source='你的图片路径.jpg' conf=0.5**



**# 例子：预测单张图片**

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source='你的图片路径.jpg' conf=0.5**



**# 例子：预测一个文件夹下的所有图片**

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source='文件夹地址/\*.jpg'**

参数解释：



conf=0.5：置信度阈值。只显示置信度高于50%的预测结果。你可以调高（如0.7）来减少误报，或调低（如0.3）来检测更多目标。



预测结果在哪里？



预测生成的图片会保存在 runs/detect/predict/ 目录（或 predict2, predict3...）下。打开这些图片，你就能看到模型画出的识别框了！



✅ 你的检查清单

找到 runs/detect/train4/ 文件夹。



打开 results.png，查看损失曲线是否下降。



对比 val\_batch\*\_labels.jpg 和 val\_batch\*\_pred.jpg。



找到 weights/best.pt 文件。



使用 best.pt 对一张新图片进行预测，验证最终效果。



如果预测结果中没有出现任何识别框，这是一个非常常见的问题，尤其是使用极小的自制数据集时。别灰心，我们来系统地排查和解决它。



首先，不要怀疑自己！ 对于只有5张图片的数据集，这是一个正常的挑战。我们的目标是找到原因并学会如何解决。



🔍 排查步骤（从最简单到最复杂）

请按照以下顺序一步步检查：



1\. ✅ 确认预测命令是否正确

确保你的预测命令格式正确，并且指向了训练得到的最佳模型 (best.pt)。



\# 确保你的命令和这个类似，source替换成你的图片路径

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source="你的测试图片路径.jpg" conf=0.25**

关键参数：尝试降低置信度阈值 conf=0.25（甚至 conf=0.1）。模型可能不太自信，较高的阈值会过滤掉所有预测。



2\. ✅ 检查预测结果保存位置

预测结果默认保存在 runs/detect/predict/（或 predict2, predict3...）文件夹下。请确认你查看的是新生成的预测结果图片，而不是原始的测试图片。



3\. 🔬 检查训练日志和指标（最重要的一步！）

这是诊断问题的核心。打开你的训练结果文件夹 runs/detect/train4/，重点看两个文件：



results.png：



查看 box\_loss 和 cls\_loss 曲线。如果它们没有呈现明显的下降趋势，而是保持平坦或剧烈波动，说明模型根本没有学习到有效的特征。这是最可能的原因。



val\_batch0\_pred.jpg：



这张图显示了模型在验证集上的预测效果。如果这张图里就有框，说明模型在训练时是能看到一些东西的。如果这张图也完全没有框，那问题就出在训练过程中。



4\. 📊 分析可能的原因及解决方案

根据上面的检查，大概率是以下两种原因之一：



❌ 可能性一：模型训练失败（损失曲线没有下降）

原因：数据量太少（5张图），模型无法从中学习到规律。



解决方案：



增广数据（推荐）：人工制造更多训练数据。



对现有的5张图片进行旋转、翻转、调整亮度、添加噪声等操作，轻松地将数据量扩充到20-30张。



可以使用一些在线工具或简单的Python脚本（如OpenCV）来完成。



大幅增加训练轮数：将 epochs 从100增加到500甚至1000。给小模型更多的时间去“记忆”这几张图片。



**yolo task=detect mode=train model=yolov8n.pt data=car\_dataset.yaml epochs=1000 imgsz=640 batch=1 patience=50 device=cpu**

简化任务：确保你的图片中的物体（小车）非常突出、背景简单，并且标注框非常精确。



❌ 可能性二：置信度阈值过高

原因：模型其实有预测，但因为置信度太低（比如只有0.1），而你的阈值设置（如默认的0.25）太高，导致所有预测都被过滤掉了。



解决方案：



大幅降低预测时的置信度阈值，直到能看见框为止：



bash

\# 使用非常低的置信度阈值，看看是否有任何输出

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source="test.jpg" conf=0.01**

如果设置 conf=0.01 后出现框了，就说明模型确实学到了东西，只是不够自信。然后你可以慢慢提高阈值，找到一个平衡点。



🧪 最终测试：对训练图片本身进行预测

为了最终确定是模型问题还是推理设置问题，用训练集里的图片进行预测：

输入

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source="训练集图片文件夹地址/\*.jpg" conf=0.01**

如果这样能画出框：说明模型“记住”了训练集，但泛化能力很差（对新图片无效）。你需要更多数据。



如果这样也画不出框：说明模型训练完全失败，需要回到上一步，尝试增加数据或训练轮数。



总结：首先检查 results.png 中的损失曲线。这是告诉你模型是否在学习的关键信号。 根据它的表现，选择对应的解决方案。对于超小数据集，这是一个需要反复调试的过程，非常正常！





结果保存位置：

预测完成后，结果会保存在 runs/detect/predict/ 文件夹（或 predict2, predict3 等递增的文件夹）中。生成的图片文件名会与原始图片相同（如 6.jpg），但会添加预测框。



🔧 额外功能：

如果你想实时查看预测结果（而不是只保存），可以添加 show=True 参数：

**yolo task=detect mode=predict model=runs/detect/train4/weights/best.pt source="测试图片地址" conf=0.25 save=True show=True**

运行这个命令后，系统会弹出一个窗口显示带有预测框的图片，同时也会保存结果文件。



请运行这个命令，然后检查 runs/detect/predict/ 文件夹中的结果！

