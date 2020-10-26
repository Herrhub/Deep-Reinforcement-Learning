
# Chapter_5 Proximal Policy Optimization

##  1. 基于on-policy的policy gradient有什么可改进之处？或者说其效率较低的原因在于？
答：
  a. 经典policy gradient的大部分时间花在sample data处，即当我们的agent与环境做了交互后，我们就要进行policy model的更新。但是对于一个回合我们仅能更新
policy model一次，更新完后我们就要花时间去重新collect data，然后才能再次进行如上的更新。
  b. 所以我们的可以自然而然地想到，使用off-policy方法使用另一个不同的policy和actor，与环境进行互动并用collect data进行原先的policy的更新。这样等价于使
用同一组data，在同一个回合，我们对于整个的policy model更新了多次，这样会更加有效率。

##  2. 使用important sampling时需要注意的问题有哪些。
答：我们可以在important sampling中将p替换为任意的q，但是本质上需要要求两者的分布不能差的太多，即使我们补偿了不同数据分布的权重p(x)/q(x) 。

E x-p [f(x)] = E x-q [f(x)*p(x)/q(x)]

当我们对于两者的采样次数都比较多时，最终的结果时一样的，没有影响的。但是通常我们不会取理想的数量的data，所以如果两者
的分布相差较大，最后结果的variance差距将会很大。

##  3. 基于off-policy的importance sampling中的 data 是从θ' sample 出来的，从θ换成θ'有什么优势？
答：使用off-policy的importance sampling后，我们不用θ去跟环境做互动，假设有另外一个 policy θ'，它就是另外一个actor。它的工作是他要去做
demonstration，θ'的工作是要去示范给θ看。它去跟环境做互动，告诉θ说，它跟环境做互动会发生什么事。然后，借此来训练θ。我们要训练的是θ，
 θ'只是负责做 demo，负责跟环境做互动，所以 sample 出来的东西跟θ本身是没有关系的。所以你就可以让θ'做互动 sample 一大堆的data，θ可以update
参数很多次。然后一直到θ train 到一定的程度，update 很多次以后，θ'再重新去做 sample，这就是 on-policy 换成 off-policy 的妙用。

##  4. 在本节中PPO中的KL divergence指的是什么？
答：本质来说，KL divergence是一个function，其度量的是两个action （对应的参数分别为θ和θ'）间的行为上的差距，而不是参数上的差距。这里行为上的差
距（behavior distance）可以理解为在相同的state的情况下，输出的action的差距（他们的概率分布上的差距），这里的概率分布即为KL divergence。
