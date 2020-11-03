
# Project 1 使用Q-learning解决悬崖寻路问题
## Problem
悬崖寻路问题（CliffWalking）是指在一个4 x 12的网格中，智能体以网格的左下角位置为起点，以网格的下角位置为终点，目标是移动智能体到达终点位置，智能体每次可以在上、下、左、右这4个方向中移动一步，每移动一步会得到-1单位的奖励。

![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/docs/cliffwalking_1.png)

如图，红色部分表示悬崖，数字代表智能体能够观测到的位置信息，即observation，总共会有0-47等48个不同的值，智能体再移动中会有以下限制：
智能体不能移出网格，如果智能体想执行某个动作移出网格，那么这一步智能体不会移动，但是这个操作依然会得到-1单位的奖励；如果智能体“掉入悬崖” ，会立即回到起点位置，并得到-100单位的奖励；当智能体移动到终点时，该回合结束，该回合总奖励为各步奖励之和。

实际的仿真界面如下:

![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/docs/cliffwalking_2.png)

由于从起点到终点最少需要13步，每步得到-1的reward，因此最佳训练算法下，每个episode下reward总和应该为-13。

## Training & Test Process and Results
通过运行main.py进行500个episodes的训练Q-learning方法得到的rewards如下图：

![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/docs/rewards.png)

采用移动平均法使可视化rewards曲线更加平滑，易于更准确地体现训练过程。

![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/docs/moving_average_rewards.png)

另外，我们还可以看到训练过程中每个episode下走到终点所需steps的变化，可以看到最后能得到最优的13steps。

![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/docs/steps.png)

训练的模型超参数初始设置如下：
### reward 的衰减率：0.9
### e-greedy策略中初始epsilon：0.9
### e-greedy策略中的结束epsilon：0.1
### e-greedy策略中epsilon的衰减率：200
### 学习率：0.1
### 训练的最大episode数目：500
