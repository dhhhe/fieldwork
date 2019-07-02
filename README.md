大屏图像处理 
====
**1.1 SR(Super Resolution)超分辨率** 

----
**1.1.1 传统方案（插值）:bowtie:**  
----
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
![fig3](https://github.com/dhhhe/fieldwork/blob/master/figure/SRCNN.bmp)  
SRCNN首先使用双三次(bicubic)插值将低分辨率图像放大成目标尺寸，接着通过三层卷积网络拟合非线性映射，最后输出高分辨率图像结果。本文中，作者将三层卷积的结构解释成三个步骤：图像块的提取和特征表示，特征非线性映射和最终的重建。 
三个卷积层使用的卷积核的大小分为为9x9,，1x1和5x5，前两个的输出特征个数分别为64和32。用Timofte数据集（包含91幅图像）和ImageNet大数据集进行训练。使用均方误差(Mean Squared Error, MSE)作为损失函数，有利于获得较高的PSNR。  

2.FSRCNN  
FSRCNN是对之前SRCNN的改进，主要在三个方面：一是在最后使用了一个反卷积层放大尺寸，因此可以直接将原始的低分辨率图像输入到网络中，而不是像之前SRCNN那样需要先通过bicubic方法放大尺寸。二是改变特征维数，使用更小的卷积核和使用更多的映射层。三是可以共享其中的映射层，如果需要训练不同上采样倍率的模型，只需要fine-tuning最后的反卷积层。由于FSRCNN不需要在网络外部进行放大图片尺寸的操作，同时通过添加收缩层和扩张层，将一个大层用一些小层来代替，因此FSRCNN与SRCNN相比有较大的速度提升。FSRCNN在训练时也可以只fine-tuning最后的反卷积层，因此训练速度也更快。FSRCNN与SCRNN的结构对比如下图所示。 
![fig4](https://github.com/dhhhe/fieldwork/blob/master/figure/FSRCNN.bmp)  
FSRCNN可以分为五个部分。特征提取：SRCNN中针对的是插值后的低分辨率图像，选取的核大小为9×9，这里直接是对原始的低分辨率图像进行操作，因此可以选小一点，设置为5×5。收缩：通过应用1×1的卷积核进行降维，减少网络的参数，降低计算复杂度。非线性映射：感受野大，能够表现的更好。SRCNN中，采用的是5×5的卷积核，但是5×5的卷积核计算量会比较大。用两个串联的3×3的卷积核可以替代一个5×5的卷积核，同时两个串联的小卷积核需要的参数3×3×2=18比一个大卷积核5×5=25的参数要小。FSRCNN网络中通过m个核大小为3×3的卷积层进行串联。扩张：作者发现低维度的特征带来的重建效果不是太好，因此应用1×1的卷积核进行扩维，相当于收缩的逆过程。反卷积层：可以堪称是卷积层的逆操作，如果步长为n，那么尺寸放大n倍，实现了上采样的操作。  
  
  以上两篇是以深度学习为基础的超分辨率经典算法，我在welink上也看到很多工程师认为上述方案，特别是FSRCNN的落地可能性更大。 

  
