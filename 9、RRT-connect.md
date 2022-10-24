


> Written with [StackEdit中文版](https://stackedit.cn/).

>文章来源：[ 基于双目视觉的六自由度机械臂避障与路径规划研究](https://kns.cnki.net/kcms/detail/detail.aspx?dbcode=CMFD&dbname=CMFDTEMP&filename=1022506528.nh&uniplatform=NZKPT&v=mqHOfXFM9X8f3vzW1IAbxZsmALwHHotTTQ59lZgnkOS2dFG8mZJHsI98nGn43fCH)

# 系统整体框架

![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/9v1Q0IgP5aAlzSZq.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/3iaYIuGzkKPGz0JK.png)
## 上位机

 - **视觉感知**
	 - 障碍物检测
	 - 障碍物空间信息获取
		 - 双目视觉 `先得到障碍物位置坐标，由此得到障碍物高度`
		 - 边缘检测算法 `获取障碍物轮廓信息，将轮廓信息与障碍物高度结合，得到障碍物三维数据`
- **路径规划（决策系统）**
	- 全局路径规划 `改进的RRT-connect算法`
	- 碰撞检测 `包围盒算法进行相交测试`

**涉及理论基础**
- 双目相机标定
- 相机与机械臂的手眼标定
- 图像信息处理
- 机械臂运动学分析
- **碰撞检测**
- **路径规划算法**

## 软硬件平台
### 硬件平台
**达野——MJ05S-600型号机械臂**
- 六自由度
- 重35kg
- 额定负载5kg
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/JE0FLzgmyNrHBjWx.png)

### 软件平台
系统采用 ***“上-下位机”*** 结构

**上位机**
 - Windows操作系统
 - MATLAB/Simulink
 - RT-SimPlus

**下位机**
- Links-Box `实时仿真机，作为机械臂控制器`

**系统硬件连接方式**
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/qr4EezMThXNebVox.png)

# 机械臂运动学分析
**机械臂底座作为机械臂基坐标系**

 1. 正运动学分析
 建立D-H参数表
	![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/u71SXTVelYuvXkR8.png)
S0 - S5 ：机械臂的六个关节

>**D-H模型表示了对机器人连杆和关节进行建模的一种非常简单的方法**
>**D-H详解：**[机器人正向运动学和D-H参数方法](https://blog.csdn.net/hangl_ciom/article/details/102752733)
>**两类D-H参数法**
改进D-H法的坐标系是固连在每个连杆的首端，而标准D-H的坐标系固连在每个连杆的末端。
① 标准D-H
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/54KeBFa9jz9ofzRh.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/vxnFYlR0WVrOEf2u.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/KTpe3GeS72JvzJZH.png)
② 改进D-H
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/QLebyH8679wMdvUY.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/XXTIgmCSimYv8iSX.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/HcnTqXpEq82zpeFL.png)
**D-H参数表**
建立各个连杆的这四个参数：$\alpha_{i-1}, a_{i-1}, \theta_i, d_i$ 的值，称为D-H参数表，建立D-H参数表即可带入以上公式完成机器人的正向解算。
① 确定Z轴
Z轴一定过连杆首端关节的旋转轴（或滑动轴）
② 确定X轴
需要分类讨论
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/VmSeeDsWhFCvgr98.png)
③ 确定Y轴
X轴和Z轴确定后，按右手定则确定Y轴。
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/6EzP8pQXWowxg4mH.png)

2. 逆运动学分析
已知机械臂末端执行器位姿，求解此时机械臂各个关节所转动角度的问题。`a.k.a 机械臂末端执行器在笛卡尔坐标空间与各关节的关节空间的相互转化`

# 双目视觉障碍物检测方法分析
## 相机选型
选用莱娜机器视觉公司：基于AR0135图像传感器的3T16型号双目相机
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/QqjR08PiGoPoHXwJ.png)
## 相机标定
略
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/tVG3iaUf4AUoJtEj.png)
## 手眼标定
将相机坐标系与机器人坐标系进行转换
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/c9XcnVBgUXcLQPOg.png)
问题：为什么可以由3-12得出3-13？
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/4dVi5BoGTLnK7B0q.png)
## 障碍物三维坐标提取
### 障碍物分割
**目的**：将障碍物从含有其他物体的工作空间分离出来，基于图像分割原理来实现。
 - 阈值分割法
 - 区域分割法
 - 边缘分割法

文章使用边缘分割法中的canny算子边缘检测和最大阈值分割进行障碍物的提取。

**canny算子**：基于优化模型的边缘检测算子`信噪比和检测精度相比其他算法有更好表现`

![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/iCmfzw0iVnHzKlv2.png)

**Sobel 算子**：检测图像中的水平、垂直和对角边缘，将原图像转化为梯度图像

**非最大值抑制**：梯度图像的边缘通常是多个像素宽，需要进行进一步细化，保留梯度幅度最大的像素点

**双阈值检测处理**：消除受噪声或光照影响的像素点`对图像中的像素点使用两个大小不同的阈值进行检测`

```mermaid
graph LR
A[左相机采集图像] --> B[灰度化] --> C[Canny算子边缘检测] --> D[最大连通域分割] --> E[目标物检测结果]
F[右相机采集图像] --> B
```

### 障碍物定位
1. 对左右两张图像分割出来的目标物体进行最小外接矩形求解，近似得到物体的尺寸 `文章使用旋转相等角度的方法，旋转结束后，选择所有面中面积最小的外接矩形作为该物体的最小外接矩形`
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/NyRTIf7GmCPe7m4K.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/DORHivWcJ8mZbJVN.png)
2. 找到外接矩形的四个顶点
3. 计算四个顶点在三维世界中的真实坐标 `基于双目视觉定位算法`
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/8oXzriamuwCiS2IB.png)
求$X_W, Y_W, Z_W$ 
4. 计算目标物体在三位环境中的高度
5. 得到目标物体的整个三维坐标信息

### 模块总体

 1. 视频输入模块`Simulink中的image processing blockset`
2. 视频接收模块`Simulink中的video and image processing blockset，截取视频中的工作环境图像`
3.  障碍物提取模块`接受图像，得到障碍物相关信息`
4. 定位模块`得到障碍物相关信息，输出x, y, width, height等`

![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/01/I85oeLcrd1iotznI.png)

# 碰撞检测与路径规划算法
## 基于简化模型的碰撞检测方法设计

**碰撞检测目的**：使机械臂末端在绕过障碍物时，机械臂各个连杆也不与障碍物发生碰撞
**障碍物简化方法**：包围盒法

### 碰撞检测方法分析
**包络法**：建模难度大，耗时长
**包围盒算法**：建模简单，适用于复杂障碍物环境`①使用简单的几何体对待测物进行建模 ②使用树形结构进行存储 ③遍历树形结构进行碰撞检测`
> **包围盒算法检测过程**
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/05/WbNlSZP9Y7aGlZIU.png)
【A、B为进行碰撞测试的两物体]】
A的包围盒：圆
B的包围盒：矩形
**左图情况**：两物体包围盒未发生交集，则两物体不碰撞
**右图情况**：两物体包围盒产生交集，但物体本身无交集，则两物体不碰撞
**碰撞情况**：两物体包围盒产生交集，且物体本身有交集

### 对物体外部包围盒的构建
**构建方法**
 - 包围球
 - 轴向包围盒（AABB）
 - 方向包围盒（OBB）
 - 离散方向多面体包围盒（K-Dop）
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/05/AxlVB6u0VGaCPd07.png)
> **包围球**
> ①能完全包裹整个物体的最小球体
> ②**优点**：构造简单、占用内存小
> ③**缺点**：对物体包裹紧密度较差

> **轴向包围盒（AABB）**
> ①**使用最广泛**
> ②包围盒的边要平行于世界坐标系的坐标轴
> ③**优点**：构造难度低、存储量小、进行相交测试时复杂度较低

> **方向包围盒（OBB）**
> ①与轴向包围盒类似，*区别在于*不对方向包围盒的边做角度要求
> ②**优点**：紧密性好
> ③**缺点**：相交测试难度大

> **离散方向多面体包围盒（K-Dop）**
> ①**优点**：紧密性最好
> ②**缺点**：构造难度以及占用存储量较大

文章采用AABB方法进行包围盒建模，因为在现实工业生产中只要保证机械臂不与障碍物发生碰撞即可，对障碍物包围盒紧密性没有太高要求。
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/05/C6ovNUUYuAWdIDtN.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/05/dPRqVq3NHfdcpx7t.png)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc4MTEzMDYxNl19
-->