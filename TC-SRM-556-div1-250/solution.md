# XorTravelingSalesman

作者：陈通

关键词：异或 记忆化搜索



## 试题来源

Topcoder SRM 556 Div1 250



## 试题大意

给一个包含$$n$$个点的图，两点间可能没有边或有一条双向边相连，边用数组$$roads[i][j]$$表示。点的编号为$$0$$到$$n-1$$，每个点有一个权值，第$$i$$个点的权值为$$cityValues_i$$。你从$$0$$号点出发开始旅行，可以沿着边走到相邻的点，一个点可以经过多次，你可以在任何一点结束旅行。初始时，你在$$0$$号点且你的收益为$$cityValue_0$$，你每经过一个点，你的收益就会与当前点的权值异或。你的最终收益为你结束旅行时的收益。求最终收益的最大值。

$$1 \leq n \leq 50, 0 \leq cityValue \leq 1023$$



## 算法介绍

此题可以用记忆化搜索求解。定义状态$$f[i][j]$$，表示是否存在某一时刻你处在$$i$$号点，且当前权值为$$j$$。初值$$f[0][cityValue_0] = true$$。对每个为$$true$$的状态进行扩展，若状态$$f[i][j] = true$$，则对于每一个与$$i$$有边相连的点$$x$$，有$$f[x][j \ XOR \ cityValue_x] = true$$。最终答案即为使得$$f[i][j] = true$$成立的最大的$$j$$。

易知状态总数为$$n \times cityValue$$，在搜索的过程中每个状态只会扩展一次，所以总时间复杂度为$$O(n^2cityValue)$$。