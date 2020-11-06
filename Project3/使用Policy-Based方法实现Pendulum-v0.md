# 使用Policy-Based方法比如DDPG等实现Pendulum-v0环境

## Pendulum-v0

![]()

钟摆以随机位置开始，目标是将其摆动，使其保持向上直立。动作空间是连续的，值的区间为[-2,2]。每个step给的reward最低为-16.27，最高为0。

环境建立如下：
  env = gym.make('Pendulum-v0') 
  env.seed(1) # 设置env随机种子
  n_states = env.observation_space.shape[0] # 获取总的状态数
  
## 注意
  本次环境action范围在[-2,2]之间，而神经网络中输出的激活函数tanh在[0,1]，可以使用NormalizedActions(gym.ActionWrapper)的方法解决
  由于本次环境为惯性系统，建议增加Ornstein-Uhlenbeck噪声提高探索率，可参考知乎文章
  推荐多次试验保存rewards，然后使用searborn绘制，可参考CSDN
  
## 任务要求
训练并绘制reward以及滑动平均后的reward随episode的变化曲线图并记录超参数写成报告，图示如下：
