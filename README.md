大屏图像处理 
====
#**1.1 SR(Super Resolution)超分辨率**  
----
1.1.1 传统方案（插值）:bowtie:  
最临近点插值算法、双线性插值、双立方插值  
  
如图双线性插值将附近点的四个像素点像素信息乘以权重得到新图像的像素信息，可以有效的抗锯齿。
![fig1](https://github.com/dhhhe/fieldwork/blob/master/figure/双线性插值.bmp)  
双立方插值考虑了梯度信息，即选取了计算目标点附近最近的16个像素点，实际求解过程中，对求解函数进一步简化。 
![fig2](https://github.com/dhhhe/fieldwork/blob/master/figure/双立方插值2.bmp)  



**参考文献：** 
[1]Cubic Convolution Interpolation for Digital Image Processing, Robert G,Keys
