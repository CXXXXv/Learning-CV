# Learning-CV
近期在学习图像算法，想把学习过程记录下来，用Python实现其中一些算法


# 一、Canny边缘检测
具体流程：
1、使用高斯滤波，创建一个5 * 5大小，sigma = 1的高斯矩阵，对原图像进行滤波；

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


# 二、霍夫变换
基本思想：
1.Each edge point votes for compatible lines.

2.Look for line that get many votes.

得到边缘检测的结果后，每个边缘点都可以有无数条直线经过它，这些直线可以表示为y0 = a * x0 + b

转换到Hough Space中，横纵坐标变为a和b，那么所有经过这个点的直线都可以表示为b = -x0 * a + y0 (x0和y0已知，所以这是一条直线）

所有的边缘点都转换到霍夫空间后，可以得到同样数量的直线，将霍夫空间按a和b分成a * b 个bins后，有一条直线通过一个bin就相当于给这个bin投了一票

获得投票数较多的bin就是实际图像中的边缘直线

但这种直线表达方式会出现斜率无限大的情况，不能有有限个bins。将直线的表达方式变为极坐标就可以解决这个问题。
在霍夫空间内的直线都可以表示为x * cosθ + y * sinθ = d

θ取0到180°，d取[-最大值，最大值]即可

##Basic Hough transform algorithm:

1.Initialize H[d, θ] = 0

2.For each edge point in E(x, y) in the image:
    for θ = 0 to 180:   
      d = x * cosθ + y * sinθ
      H[d, θ] += 1
      
3.Find the value(s) of (d, θ) when H[d, θ] is maximum

4.The detected line in the image is given by d = x * cosθ + y * sinθ

扩展： 可以使用梯度简化算法
