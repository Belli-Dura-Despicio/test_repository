# ＃常见问题
## 能否审查三人麻将日志？
不，它不能。三人麻将是一个完全不同的游戏。

## ＃＃在麻将中的引擎有多好？
我们不知道，因为我们没有一个好的、坚实的、有效的、合法的、可靠的手段来评估它在人类标准中的强度。

##什么是pt？
简单地说，它们是对局结束时最终排名的加权版本。90,45,0,-135 "是一个七段棋手在天后汉宫的pt分布。

## ＃＃＃他们相互之间的比较有多好？
在重复的麻将中，1个AKOCHAN对3个Mortal的设置和`90,45,0,-135`的比例来衡量，Mortal以平均等级2.479和平均比例1.677胜过`90,45,0,-135`的AKOCHAN，并以平均等级2.482和平均比例1.961胜过`90,30,-30,-90`的AKOCHAN。

有关的细节将在未来的Mortal的文件中注明。

## ＃＃＃（凡人）记号是什么意思？
$P_k^p$是一个向量，由玩家$p$实现4个相应位置的可能性值组成，在这个游戏的kyoku $k$开始时估计。

$Phi_k$是pt EV，在这个游戏的kyoku $k$开始时估计的。

$hat Q^\pi(s_k^i, a_k^i)$是由模型评估的[Q值](https://en.wikipedia.org/wiki/Q-learning)。
在这个游戏中，kyoku $k$的第i$种状态。
与$pi$代表模型的政策。

Mortal的Q值优化目标是$\Phi_{k+1}。- Phi_k$。
所以理论上$hat Q^\pi(s_k^i, a_k^i) + `Phi_k$是对pt EV的估计。

## ＃＃＃（凡人）为什么除了最好的以外，所有的行动有时都有明显低于最好的Q值？
如上所述，$hat Q^\pi(s_k^i, a_k^i) + \Phi_k$是对pt EV的估计。然而，对这个值的评估是**<ins>手段而不是目标</ins>**。明确地说，Mortal作为麻将AI的真正基本目标是在麻将游戏中取得最好的成绩，而不是为所有的行动计算出准确的分数。因此，除了最好的行动之外，所有行动的评估值都可能是不准确的；它们只是作为一种手段来确定其在训练中的探索偏好。

这是一个利用与探索的两难问题。首先，Mortal是[无模型](https://en.wikipedia.org/wiki/Model-free_(reinforcement_learning))，这意味着它不能获得优化的目标，即实际的Q值$Q^\pi(s_k^i, a_k^i)$，而没有实际评估行动$a$。
因此，如果我们打算使行动的Q值更加准确，模型将不得不更多地探索那些不太可能的行动，这可能会导致高估一些不好的行动，使它表现得更差。在像麻将这样有很多随机性的游戏中，这种高估是非常有可能发生的，因为方差非常大。为了避免这样的性能退步，模型需要利用更多的东西，导致不太准确的预测Q值$hat Q^\pi(s_k^i, a_k^i)$。

## (kochan) 如何配置pt分布？
在`tactics.json`中，改变`jun_pt`值。注意，每个元素都有一个硬编码的边界$[-200, 200]$。

∮∮(akochan)为什么akochan有时表现得如此古怪？
Akochan不擅长侃大山。Akochan在极端情况下也有数值稳定性问题。

Akochan对其唯一的目标非常积极--"最终 "的pt EV，而不是仅仅赢得这一轮。

## ＃＃＃评级是如何计算的？
$$
100 次 (
    \frac{1}{k}\displaystyle\sum_{k=1}^k
    \Frac{1}{i}的情况。\displaystyle\sum_{i=1}^n
    \frac
    {hat Q^\pi(s_k^i, a_k^i)-\displaystyle \min_a \hat Q^\pi(s_k^i, a_k^i)}。
    {displaystyle \max_a \hat Q^\pi(s_k^i, a_k^i) - \displaystyle \min_a \hat Q^\pi(s_k^i, a_k^i) }
)^ 2
$$

这种计算方法非常基本，而且不是一个可靠的措施。
