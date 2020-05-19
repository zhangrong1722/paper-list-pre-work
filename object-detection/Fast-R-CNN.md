## motivation

R-CNN的问题在于速度太慢，虽然SPPNet将其速度大大加快，但是却存在性能比不上R-CNN的问题。Fast R-CNN结合了两者的优势，既加快了运行速度，又提升了性能。

## method

网络示意图如下图所示：

<img src="../asserts/fast rcnn.png" alt="fast rcnn" style="zoom:50%;" />

Fast R-CNN采用了SPPNet的作用，引入RoI Pooling层，实际上就是SPP层，这使得Fast R-CNN能像SPPNet一样，CNN只需要对输入图像提取一次特征，这大大加快了模型训练速度，对于不同尺寸region proposal的特征，Rol Pooling层输出固定长度的特征。

对于SPPNet和R-CNN，提取到region proposal的特征之后，需要设置多个SVM分类器和回归器分别来分类region proposals并fine-tuning bounding box位置，故SPPNet和R-CNN均是multi-stage的，在R-CNN中，为了更新CNN（SPPNet中不更新CNN），R-CNN使用提取到的region proposal（IoU>0.5为正例，IoU<0.5为负例）fine-tuning CNN。而Fast R-CNN采用multi-task思想，设置两个分支分别完成region proposal分类和位置回归，将这两个任务合并到single stage，也就不需要存储region proposal的特征到磁盘。在更新CNN方面，Fast R-CNN使用从少量图像中提取较多region proposal。paper举了一个例子，Fast R-CNN从2张图像中分别提取64个Rol（即region proposal），而R-CNN是从128个图像中取一个Rol。显然，Fast R-CNN中的策略更为高效。总之，Fast R-CNN有如下优点：

1. 更高的mAP
2. 不需要额外磁盘存储region proposal的特征（Rol输出）
3. 一个阶段（多任务）
4. 可更新所有网络参数（主要与R-CNN相比）

