
# Chapter_4 Policy Gradient 
# 梯度策略

## 概念
### policy（策略）：
  每一个actor中会有对应的策略，这个策略决定了actor的行为。具体来说，Policy 就是给一个外界的输入，然后它会输出 actor 现在应该要执行的行为。一般地，我们将policy写成π 。
### Return（回报）： 
  一个回合（Episode）或者试验（Trial）所得到的所有的reward的总和，也被人们称为Total reward。一般地，我们用 RR 来表示它。
### Trajectory：
  一个试验中我们将 environment 输出的 ss 跟 actor 输出的行为 aa，把这个 ss 跟 aa 全部串起来形成的集合，我们称为Trajectory，即  Trajectory τ={s1,a1,s2,a2,⋯,st,at}。
### Reward function：
  根据在某一个 state 采取的某一个 action 决定说现在这个行为可以得到多少的分数，它是一个 function。也就是给一个s1，a1，它告诉你得到r1。给它s2，a2，它告诉你得到 r2。把所有的 r 都加起来，我们就得到了R(τ) ，代表某一个 trajectory \tauτ 的 reward。
### Expected reward： 
  Rˉθ=∑ τR(τ)p θ(τ)=E τ∼pθ(τ)	[R(τ)]。
### Reinforce：
  基于策略梯度的强化学习的经典算法，其采用回合更新的模式。

## 问题
###  1. 如果我们想让机器人自己玩video game, 那么强化学习中三个组成（actor、environment、reward function）部分具体分别是什么？
答：actor 做的事情就是去操控游戏的摇杆， 比如说向左、向右、开火等操作；environment 就是游戏的主机， 负责控制游戏的画面负责控制说，怪物要怎么移
动， 你现在要看到什么画面等等；reward function 就是当你做什么事情，发生什么状况的时候，你可以得到多少分数， 比如说杀一只怪兽得到 20 分等等。
###  2. 在一个process中，一个具体的trajectory里s_1,a_1,s_2,a_2出现的概率取决于什么？
答： 1. 一部分是 environment 的行为， environment 的 function 它内部的参数或内部的规则长什么样子。p(s_t+1|s_t,a_t) 这一项代表的是 environment， environment
这一项通常你是无法控制它的，因为那个是人家写好的，你不能控制它。
  2. 另一部分是 agent 的行为，你能控制的是p_θ(a_t|s_t)。给定一个s_t， actor 要采取什么样的a_t会取决于你 actor 的参数θ， 所以这部分是 actor 可以自己
控制的。随着 actor 的行为不同，每个同样的 trajectory， 它就会有不同的出现的概率。
###  3. 当我们在计算 maximize expected reward时，应该使用什么方法？
答： gradient ascent（梯度上升），因为要让它越大越好，所以是 gradient ascent。Gradient ascent 在 update 参数的时候要加。要进行 gradient ascent，我们先要
计算 expected reward R的 gradient 。我们对R取一个 gradient，这里面只有p_θ(τ)是跟θ有关，所以 gradient 就放在p_θ(τ)这个地方。
###  4. 我们应该如何理解梯度策略的公式呢？
  答：
  
  ![](https://github.com/Herrhub/Deep-Reinforcement-Learning/blob/main/images/4.7.png)
  
  如上式，p_θ(τ)里 面 有 两 项 ， p_θ(s_t+1|s_t,a_t)来 自 于 environment， p_θ(a_t|s_t)是 来 自 于 agent。 p_θ(s_t+1|s_t,a_t)由 环 境 决 定 从 而 与 θ 无 关。
  因 此∇log p(s_t+1|s_t,a_t) = 0。 因 此∇p_θ(τ) = log p_θ(a^n_t|s^n_t) 。
  具体来说：
    a. 假设你在s_t执行a_t，最后发现τ的 reward 是正的， 那你就要增加这一项的概率，你就要增加在s_t执行a_t的概率。
    b. 反之，在s_t执行a_t会导致τ的 reward 变成负的， 你就要减少这一项的概率。
###  5. 我们可以使用哪些方法来进行gradient ascent的计算？
答：用 gradient ascent 来 update 参数，对于原来的参数θ，可以将原始的θ加上更新的 gradient 这一项，再乘以一个 learning rate，learning rate 其实也是要调
的，和神经网络一样，我们可以使用 Adam、RMSProp 等优化器对其进行调整。
###  6. 我们进行基于梯度策略的优化时的小技巧有哪些？
答：
  1. Add a baseline：#为了防止所有的reward都大于0，从而导致每一个stage和action的变换，会使得每一项的概率都会上升。所以通常为了解决这个问题，我们把
reward 减掉一项叫做 b，这项 b 叫做 baseline。你减掉这项 b 以后，就可以让 R(τ^n)-b 这一项， 有正有负。 所以如果得到的 total reward R(τ^n) 大于 b 的
话，就让它的概率上升。如果这个 total reward 小于 b，就算它是正的，正的很小也是不好的，你就要让这一项的概率下降。 如果 R(τ^n) < b， 你就要让这
个 state 采取这个 action 的分数下降 。这样也符合常理。
  2. Assign suitable credit： 首先第一层，本来的 weight 是整场游戏的 reward 的总和。那现在改成从某个时间t开始，假设这个 action 是在 t 这个时间点所执
行的，从t这个时间点，一直到游戏结束所有 reward 的总和，才真的代表这个 action 是好的还是不好的；接下来我们再进一步，我们把未来的reward做一个
discount，这里我们称由此得到的reward的和为Discounted Return(折扣回报) 。
  3. 综合以上两种tip，我们将其统称为Advantage function， 用 A 来代表 advantage function。Advantage function 是 dependent on s and a，我们就是要计算的是在
某一个 state s 采取某一个 action a 的时候，advantage function 有多大。
  4. Advantage function 的意义就是，假设我们在某一个 state s_t执行某一个 action a_t，相较于其他可能的 action，它有多好。它在意的不是一个绝对的好，而是
相对的好，即相对优势(relative advantage)。因为会减掉一个 b，减掉一个 baseline， 所以这个东西是相对的好，不是绝对的好。A^θ(s_t,a_t)通常可以是由一个
network estimate 出来的，这个 network 叫做 critic。
###  7. 对于梯度策略的两种方法，蒙特卡洛（MC）强化学习和时序差分（TD）强化学习两个方法有什么联系和区别？
答：
  1. 两者的更新频率不同，蒙特卡洛强化学习方法是每一个episode更新一次，即需要经历完整的状态序列后再更新，而对于时序差分强化学习方法是每一个
step就更新一次。相对来说，时序差分强化学习方法比蒙特卡洛强化学习方法更新的频率更快。
  2. 时序差分强化学习能够在知道一个小step后就进行学习，相比于蒙特卡洛强化学习，其更加快速、灵活。
  3. 具体举例来说：假如我们要优化开车去公司的通勤时间。对于此问题，每一次通勤，我们将会到达不同的路口。对于时序差分（TD）强化学习，其会对于每
一个经过的路口都会计算时间，例如在路口 A 就开始更新预计到达路口 B、路口 C , 以及到达公司的时间；而对于蒙特卡洛（MC）强化学习，其不会
每经过一个路口就更新时间，而是到达最终的目的地后，再修改每一个路口和公司对应的时间。
###  8. 请详细描述REINFORCE的计算过程。
答：首先我们需要根据一个确定好的policy model来输出每一个可能的action的概率，对于所有的action的概率，我们使用sample方法（或者是随机的方法）去选择
一个action与环境进行交互，同时环境就会给我们反馈一整个episode数据。对于此episode数据输入到learn函数中，并根据episode数据进行loss function的构造，通过
adam等优化器的优化，再来更新我们的policy model。
