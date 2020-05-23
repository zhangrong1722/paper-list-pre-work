## motivation

使用Selective Search提取proposals之后，fast r-cnn可以以end-to-end方式实现物体检测。因此，在测试阶段，影响物体检测的瓶颈是proposals的提取，而Selective Search方式是在cpu上运行的，是一种传统方法。faster r-cnn提出一种CNN即RPN（Region Proposal Network），代替Selective Search方法提取图像的proposals。

## method

RPN本质依旧是CNN，输入一张图像，CNN提取得到特征图之后，依次将特征图的每个像素作为Anchor，随后提取不同宽度和比例的窗口作为proposal的特征，随后经过RoI池化层得到proposals的特征，最后送入二分类classifier（是否是proposal）并微调proposal位置。RPN提到到proposals之后，就是一个fast r-cnn了。

到此，faster r-cnn中的所有工作均是由CNN完成。但是，proposals提取和object detection（Detector）这两个阶段内部是end-to-end的，但是两个阶段之间仍旧不是end-to-end。为此，paper中介绍了两种训练方式：1）RPN和Detector交替训练；2）RPN和Detector联合训练。

<img src="../asserts/faster r-cnn.png" style="zoom:30%;" />

<img src="../asserts/faster r-cnn-anchors.png" style="zoom:40%;" />

