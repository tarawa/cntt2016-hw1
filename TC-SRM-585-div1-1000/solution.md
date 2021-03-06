# EnclosingTriangle
作者：叶芃

关键词：计算几何 单调性 计数 前缀和
## 题目简述

给定由$$(0,0),(1,0),...,(m,0),(m,1),...(m,m),(m-1,m),...(0,m),(0,m-1),...(0,1)$$

这$$4m$$个点围成的正方形。正方形内部有若干个点(不超过20个，不在边界上，点的坐标都是整数)，要求在正方形上选取三个点并且给定的点都在这三个点围成的三角形内部或边上，求选取这三个点的方案数。($$2\le m \le 58585$$)


## 算法

先考虑如何判断一个点在$$\Delta ABC$$内部或边上。设该点为$$ P $$，则根据叉积的知识，只需判断$$\overrightarrow{AB}\times \overrightarrow{AP}\ge0$$且$$\overrightarrow{BC}\times \overrightarrow{BP}\ge0$$且$$\overrightarrow{CA}\times \overrightarrow{CP}\ge0$$即可，也就是$$P$$在$$\overrightarrow{AB},\overrightarrow{BC},\overrightarrow{CA}$$的左侧。

朴素的做法便是先将点按逆时针编号，并按编号从小到大枚举$$A,B,C$$，保证三个点的编号递增，并判断所有点是否在这个三角形内。

记$$n=4m$$，设给定的点为$$\{P_1,P_2,...,P_k\}$$，对于一个点$$A$$，满足$$\forall i \in[1,k],\overrightarrow{AB}\times \overrightarrow{AP_i}\ge0$$的点$$B$$的编号显然是一段区间，因为我们是逆时针编号，设满足条件的最大编号的点为$$B'$$，则对于任意编号介于$$A$$和$$B'$$之间的点$$B_0$$，在$$\overrightarrow{AB}$$左侧的点也会在$$\overrightarrow{AB_0}$$左侧，同理，满足$$\forall i \in[1,k],\overrightarrow{CA}\times \overrightarrow{CP_i}\ge0$$的点$$C$$的编号也是一段区间。设对于第$$i$$号点上述两个区间分别为$$[i+1,f_i]$$和$$[g_i,n]$$，可以发现$$f_i$$和$$g_i$$均是单调不减的，同样因为我们是逆时针枚举点，以$$f_i$$为例，设$$A$$之后的点为$$A_0$$，那么在$$\overrightarrow{AB}$$左侧的点也会在$$\overrightarrow{A_0B}$$左侧，$$g_i$$同理。于是我们就可以利用单调性在$$O(n)$$的时间内求出$$f_i$$和$$g_i$$($$f_i$$是一定存在的，对于$$g_i$$，若不存在，为方便起见，令$$g_i=n+1$$)。

假设我们已知两个点$$a$$和$$b$$，那么必然有$$b\in [a+1,f_a]$$，对于$$c$$，首先可以知道$$c\in[g_a,n]$$，而换个角度对$$b$$考虑，得到$$c\in[b+1,f_b]$$。这样一来所有条件都满足了。而由于$$f_b\le n$$，于是$$c\in [max(b+1,g_a),f_b]$$。我们要求的答案即

$$\sum_{a=1}^{n-1}\sum_{b=a+1}^{f_a}max(f_b-max(g_a,b+1)+1,0)$$

注意到在上式中，有$$g_a\ge f_a \ge b$$，我们对于$$b=f_a$$的情况单独计算，则只需求

$$\sum_{a=1}^{n-1}\sum_{b=a+1}^{f_a-1}max(f_b-g_a+1,0)$$

由于$$f_i$$与$$g_i$$是单调不减的，我们可以在枚举$$a$$的时候维护一个变量$$t$$，表示最小的满足$$f_t>g_a-1$$的编号。那么对于$$\sum_{b=a+1}^{f_a-1}max(f_b-g_a+1,0)$$，若$$t>f_a-1$$则该式为0，否则为$$\sum_{b=max(t,a+1)}^{f_a-1}f_b-(g_a-1)\times(f_a-max(t,a+1))$$，这样一来，我们只需要再预处理出$$f_i$$的前缀和就可以$$O(1)$$计算，时间复杂度降为$$O(n\times20)$$，已经可以通过所有数据。

事实上，若给定点集的数量再扩大(例如扩大到$$10^5$$)，也是可以做的。这时只需求出给定点集的凸包，在求$$f_i$$和$$g_i$$时利用凸包的单调性维护到两条直线的最近点即可，时间复杂度为$$O(m+|S|)$$，其中$$|S|$$表示给定点集的大小。
