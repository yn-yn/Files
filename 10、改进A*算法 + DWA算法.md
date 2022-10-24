


> Written with [StackEdit中文版](https://stackedit.cn/).

> cited from [融合改进A*算法与DWA算法的机器人实时路径规划](https://kns.cnki.net/kcms/detail/detail.aspx?dbcode=CAPJ&dbname=CAPJLAST&filename=WXDG2022090700A&uniplatform=NZKPT&v=rLTbnxKi-e6VrsxSA4OvVcdWXTusUpjyQmz3ny0ycyTKrlevg9wsl7xIlitYW-TT)

**传统A^*^（A-star）算法**：搜索效率低下、节点冗余且不平滑及转弯易靠近障碍物等缺点

**改进**：改进A*算法与动态窗口法（Dynamic Window Approach，DWA）相融合的实时路径规划方法

**该算法的主要创新点有：**
1. 优化A*算法评价函数，加入周围环境及父节点的影响，以指数加权的方式提高寻路效率
2. 增加节点安全扩展策略保证路径安全性；
3. 使用关键点提取策略剔除冗余节点；
4. 将提取的关键点信息加入DWA 算法的评价函数中，实现算法融合。

==**DWA 算法具有良好的局部避障能力**==（dynamic window approach）

# 使用改进A*算法与DWA算法对一直状况进行路径规划
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/sdrnH68zwIGIhKGr.png)

## 改进A*算法仿真对比实验
① 20X20 随机栅格地图
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/xNFGZChQb1c7xBDc.png)

② 40X40 随机栅格地图
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/guBGTv8LRGggxfpn.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/yVjfmZoC4IsjG3hv.png)
由上述实验结果可知，在20X20的简单栅格环境地图中，本文所改进的A^*^ 的寻路时间相比于传统A^*^ 算法降低了**81%**，相比于文献[15]改进A^*^ 算法下降了**84%**；同时，本文所改进的A^*^ 的**安全性**相比于传统A^*^ 算法和文献[14]改进A^*^ 算法有大幅提高，使机器人在转弯时与障碍物保持安全距离；而且本文所改进的A^*^算法在保证路径较短的同时，最大程度减小了转折角度和次数，使**规划路线更加平滑**。
在40X40的复杂随机栅格环境地图中，由于地图环境较复杂且同时要考虑路径的安全性，导致本文改进的A^*^ 算法较传统A^*^ 算法的路径长度及转折角度略有上升，但本文改进的A^*^ 算法规划的路径距离障碍物的平均距离更远，也**不会出现斜穿障碍物的情况**，而且相比于传统A^*^ 算法的寻路时间降低了**89%**，从而保证机器人可以安全、快速地避开环境内的已知障碍物。

## 融合导航算法仿真实验
融合算法的路径仿真结果如下图所示，在图中以灰色栅格表示突然出现的障碍物，`*`代表由改进A^*^ 算法所得到全局关键点，
蓝色虚线表示由改进A^*^ 算法得到的路径，红色实线表示由融合算法得到的路径。
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/A2VQ32hxsKK2jIGa.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/uHMZHDGfCewQ5Czd.png)

由图7可知，当环境中不存在未知障碍物时，改进A^*^ 算法和融合算法均能顺着关键点的指引到达目标点，具体如图7(a)所示，此时在机器人的行进路线上分别添加1~3 个未知障碍物，分别如图7(b)～(d)所示，可以看出改进A^*^ 算法所规划的路径并没有规避未知障碍物，而融合导航算法在全局路径的引导下对未知障碍物进行合理规避，并与障碍物之间始终保持安全距离。由上述仿真结果可知，融合导航算法能在保证全局最优的基础上，安全快速地实现动态避障功能。

## 实验验证
① 无未知障碍物
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/moEXUukZt8hRILPa.png)
从实验结果可以看出，改进A^*^ 算法规划的路线较原始A^*^ 算法规划的路线转折更小、平滑性更好，并且规划用时也大幅减小，其中原始A^*^ 算法耗时约8.7 s，改进A^*^ 算法耗时约3.6 s。

② 含未知障碍物
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/19/xVZHhXwWoybTU719.png)
从实验结果可以看出，虽然融合算法在遇到未知障碍物时转折角度会变大，但成功地避开了环境中的2 个未知障碍物。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIyNzkzMjM2OF19
-->