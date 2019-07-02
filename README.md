大屏图像处理 
====
**1.1 SR(Super Resolution)超分辨率** 

----
1.1.1 传统方案（插值）:bowtie:  
最临近点插值算法、双线性插值、双立方插值  
  
如图双线性插值将附近点的四个像素点像素信息乘以权重得到新图像的像素信息，可以有效的抗锯齿。
![fig1](https://github.com/dhhhe/fieldwork/blob/master/figure/双线性插值.bmp)  
双立方插值考虑了梯度信息，即选取了计算目标点附近最近的16个像素点，实际求解过程中，对求解函数进一步简化。 
![fig2](https://github.com/dhhhe/fieldwork/blob/master/figure/双立方插值2.bmp)  



**参考文献：** 
[1]Cubic Convolution Interpolation for Digital Image Processing, Robert G,Keys    
  
**1.2 基于深度学习的处理方案** 
----
1. SRCNN  
SRCNN是深度学习较早用于超分辨率重建上的算法。其网络结构简单，仅仅用了三个卷积层，如下图所示。 
![fig1](https://github.com/dhhhe/fieldwork/blob/master/figure/SRCNN.bmp)  
SRCNN首先使用双三次(bicubic)插值将低分辨率图像放大成目标尺寸，接着通过三层卷积网络拟合非线性映射，最后输出高分辨率图像结果。本文中，作者将三层卷积的结构解释成三个步骤：图像块的提取和特征表示，特征非线性映射和最终的重建。 
三个卷积层使用的卷积核的大小分为为9x9,，1x1和5x5，前两个的输出特征个数分别为64和32。用Timofte数据集（包含91幅图像）和ImageNet大数据集进行训练。使用均方误差(Mean Squared Error, MSE)作为损失函数，有利于获得较高的PSNR。  
2.FSRCNN
