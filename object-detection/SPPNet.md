## motivation

AlextNet在2012年提出来的，而SPPNet是在14年被ECCV Accepted，是一篇比较旧的paper了。CNN中由于全连接层的存在，导致CNN输入尺寸是固定的，分类任务中输入尺寸一般为（224，224），这也是PyTorch中pretrained模型的输入尺寸。因而对于不同尺寸的输入图像，必须crop/wrap至指定尺寸，这会导致图像发生几何形变，模型性能也会发生一定程度的性能下降。

## method

卷积层允许任何尺寸图像输入，全连接层要求输入是固定维度的，paper的做法是在最后一层卷积层和全连接层之间加入SSP层，SSP层的目的是在输入特征图是任何尺寸的情况下，都输出固定维度的特征图。示意图如下图所示：

<img src="/Users/estelle/Documents/DL/Code/paper-list-pre-work/asserts/sppnet.png" alt="sppnet" style="zoom:40%;" />

对于任意尺寸的特征图，paper的做法是将特征图都分成n\*n个子图，再对每个子图执行全局均值/最大池化操作，最终得到的维度是n\*n，设输入特征图的channel数是K，则SSP层输出的特征维度是（n\*n\*K），paper还采用了多尺度，来加强特征提取能力。paper使用三组尺度：6\*6、3\*3和1\*1，1\*1实际上就相当于直接对特征图执行全局池化。

## experiments

需要固定输入尺寸是CNN的共同特点，故SSP层对于所有CNN的性能均有提升，paper主要针对classification和object detection。classification很容易理解，可利用多种不同输入尺寸训练网络。

object detection的话，借助于SPP层，可以避免CNN对Selective Search方法提取到的每个region proposal都提取特征（RCNN中的做法），而只需要对每张图提取一次特征，然后将region proposal在原图中的相对位置映射到CNN提取到的特征图中，从而得到region proposal的特征图。SPPNet最大的优势在于大大加快了网络训练过程，paper report 在Pascal VOC 2007数据集上SPPNet比RCNN快了24-102倍。

