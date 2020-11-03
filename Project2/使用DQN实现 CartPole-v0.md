# 使用DQN实现CartPole-v0
## 推荐使用Double-DQN去解决，即建立两个初始参数相同的全连接网络target_net和policy_net。

## CartPole-v0
CartPole-v0是OpenAI gym中的一个经典环境，通过向左(action=0)或向右(action=1)推车能够实现平衡，所以动作空间由两个动作组成。每进行一个step就会给一个reward，如果无法保持平衡那么done等于true，本次episode失败。理想状态下，每个episode至少能进行200个step，也就是说每个episode的reward总和至少为200，step数目至少为200。

![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/images/p1.png)

### 训练并绘制reward以及滑动平均后的reward随episode的变化曲线图并记录超参数

### 同时也可以绘制测试(eval)模型时的曲线
