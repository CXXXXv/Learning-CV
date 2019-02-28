# Learning-CV
近期在学习图像算法，想把学习过程记录下来，用Python实现其中一些算法


首先是Canny边缘检测。具体流程：
1、使用高斯滤波，创建一个5*5大小，sigma=1的高斯矩阵，对原图像进行滤波；
2、求x和y方向的梯度，分别使用了简单求导和使用Sobel算子求梯度；
3、非极大值抑制NMS，根据梯度矩阵和每个位置的梯度角度，使边界宽度变为只有一个像素；
4、根据双阈值连接所有边界：大于高阈值的保留作为边缘；低于低阈值的抑制；在高阈值和低阈值间的像素点在周围寻找是否存在强边界，如果存在强边界则保留作为边缘，否则抑制。

1.Filter image with derivative of Gaussian
2.Find magnitude and orientation of gradient
3.Non-maximum suppresion: Thin multi-pixel wide ridges down to single pixel width
4.Linking and thresholding(hysteresis):
  Define two thresholds: low and high
  Use the high threshold to start edge curves and the low threshold to continue
  
Canny threshold hysteresis:
1.Apply a high threshold to detect strong edge pixels.
2.Link those strong edge pixels to form strong edges.
3.Apply a low threshold to find weak but plausible(possible) edge pixels.
4.Extend the strong edges to follow weak edge pixels.
