---
share: "true"
---
# Intelligent Agent
An Agent is anything that has : Agent就是有传感器和执行器的单元。
- Sensors: to sense information 
- Actuators: to act

Different **Environment**:
![[Pasted image 20230123162221.png|500]]
环境有多种feature: 可观不可观，随机或者决定性的等等。

## Rational and Intelligent
- **rational**:  the result of the action is good evaluated or the reward of the action will be maximized. 只要是正确的，收益最大的都是rational
- **intelligent**:  applying knowledge or using thought and reasoning to find the optimal action 通过已知的知识或者通过思考来进行行动
  比如膝跳反应(knee-jerk reaction)属于反射，is rational but not intelligent.

- **Perfect rationality**: the ability to make good decisions given the information 合理性的完美与否只取决于能否做出最好决策，与给定的信息无关。即使只给了部分的信息，只能要做出当前信息最好的决策，也算作是perfectly rational.
- **Pure reflex agent**: ignore previous percepts, cannot obtain the optimal action in partially observable environment


算法的四个评价维度：
- **Completeness**: Is the algorithm guaranteed to find a solution when there is one, and to correctly report failure when there is not?
- **Cost optimality**: Does it find a solution with the lowest path cost of all solutions?
- **Time complexity**: How long does it take to find a solution? This can be measured in seconds, or more abstractly by the number of states and actions considered.
- **Space complexity**: How much memory is needed to perform the search?


# Uninformed Search and Informed Search
两者本质的区别就是==是否有information==。
Uninformed Search
1. Search **without** information
2. No knowledge
3. Time consuming
4. More complexity
5. Eg. **DFS BFS**

Informed Search
1. Search **with** information
2. use knowledge to find **steps** to solution
3. Quick solution
4. Less complexity(time, space)
5. Eg. **A*, heuristic DFS, Best-First-Search** 

# Uninformed Search

## Breadth first search
1. [Code](https://www.geeksforgeeks.org/breadth-first-search-or-bfs-for-a-graph/)
2. [Video](https://youtu.be/qul0f79gxGs)

![Untitled](00%20Computer%20Science/00%20Grundlagen%20der%20Künstlichen%20Intelligenz/Images/Untitled%201.png)
搜索过程中Queue的变化:
![[Pasted image 20230123220837.png|400]]

Performance Analyse:
- **completeness**: ==前提是只要宽度b是有限的==，总是能找到目标
- **optimality**：==如果所有的成本是相同的==，那么宽度优先是最优的
- **time complexity**: $1+b+b^{2}+b^{3}+\cdots+b^{d}=O\left(b^{d}\right)$ 假设每层都有b个子节点，在第d层得到最优解。如果每层的子节点不一致，取最大的那一个。
- **space complexity**: BFS需要存储所有节点的数据，因此为 $O\left(b^{d}\right)$
- **FIFO Queue**

Breadth first algorithm本质还是一个NP算法，时间/空间的复杂度较高。

## Dijkstra’s algorithm (uniform-cost search)
[Video](https://youtu.be/dRMvK76xQJI)
该算法本质是利用best-first algorithm，含有各种路径cost。与宽度优先搜索不同的是，BFS如果搜索到goal算法会直接停止；但是Dijkstra会继续搜索，因为考虑到路径的cost，很有可能之后出现的goal的cost会比上一次出现的更低。==注意该算法检测目标是在拓展该节点而不是生成该节点。==
Explanation in book:
> Note that if we had checked for a goal upon generating a node rather than when expanding the lowest-cost node, then we would have returned a higher-cost path 

Breath first algorithm is optimal if cost is uniform. But **if actions have different costs**, we will use best-first algorithm with Path-Cost as evaluation function, which names Dijkstra’s algorithm

Performance Analyse:
- **Completeness**: 可以找到goal
- **optimal**: 如果所有的cost为正值，是最优的
- **time complexity**: $O\left(b^{1+\left[\frac{C^{*}}{\varepsilon}\right]}\right)$, where $C^{*}$-the cost of optimal solutions, $\varepsilon$ - the small cost
- **space complexity**:$O\left(b^{1+\left[\frac{C^{*}}{\varepsilon}\right]}\right)$ 如果cost都相同，复杂度为$O\left(b^{d+1}\right)$与BFS算法类似, 但复杂度要比BFS高
-  **Priority Queue**

![Untitled|600](00%20Computer%20Science/00%20Grundlagen%20der%20Künstlichen%20Intelligenz/Images/Untitled%203.png)


## Depth-first Search
从这个图可以清楚的看到DFS的搜索过程，以及为什么空间复杂度是$O(b\cdot m)$，空间复杂度考虑的是最复杂的情况，即goal在最大深度为m的结点。需要储存每一层一个节点的所有子节点，可以看到紫色的表示reached数组，绿色的表示frontier数组，两个数组加在一起就是占用的空间，因此从第四张图可以看到，在branch factor=2, max depth=3,空间复杂度为$2^3=8$ (虽然是实际为7？)

> 深度优先搜索只需要存储一条从根结点到叶结点的路径,以及该路径上每个结点的所有未被扩展的兄弟结点即可。其空间复杂度要远远小于BFS。


![[Pasted image 20230124144222.png|Pasted image 20230124144222.png]]

Performance Analyse
- **Completeness**: : Not so good. 对于图搜索，如果是有限的，DFS则是完备的；而对于树搜索，容易陷入循环中，则不完备。
- **Optimal**: No. DFS会优先搜索左侧最深的目标，如果目标在右侧较浅的层级，依然会返回更深的目标。所以DFS不是最优的。
- **Time Complexity**: $O\left(b^{d}\right)$, where m - 节点的最大深度
- **Space Complexity**: $O(b\cdot m)$
- **LIFO stack**

### Depth-limited Search

**Idea**: 为了改善DFS的完备性和最优性而提出的。类似于DL的dropout，人为限定一个最大深度$l$ 。

- **Complete**:only if limit $l≥d$
- **Optimal**:only guaranteed if limit $l=d$ 最优的条件非常苛刻，必须是两者相等。
- **Time Complexity**: $O\left(b^{l}\right)$, where $l$ - 自定义最大深度
- **Space Complexity**: $O(b\cdot l)$

But it also has some problems:

- for most problems we will not know a good depth limit until we have solved the problem.
- If branching factor is infinite at the second level, also problematic

### Iterative Deepening Depth-first Search

**Idea**: perform depth-limited search repeatedly with increasing depth limit $l$. 就是重复的增加深度限制$l$，直到发现目标goal。结合了深度优先和宽度优先的优点。

具体算法的搜索过程：
![Untitled](00%20Computer%20Science/00%20Grundlagen%20der%20Künstlichen%20Intelligenz/Images/Untitled%204.png)

- Complete: if b is finite 
- Optimal: if all costs are uniform
- time complexity:$N(\operatorname{IDS})=(d) b^{1}+(d-1) b^{2}+(d-2) b^{3} \cdots+b^{d}$ b-nodes d-depth
- Space Complexity: $O(b\cdot m)$

可能会疑问如果这样重复的搜索，会不会增加搜索时间，有更大的时间复杂度？实际并不是，因为重复多次的都是上层的结点，而真正数量大的结点在下次，但重复的次数低，最后一层的重复次数仅为1。

通过下面的计算便可知道，两者时间复杂度相差无几，但是空间复杂度IDDFS还是要更胜一筹。
![[00 Computer Science/00 Grundlagen der Künstlichen Intelligenz/Images/Untitled 5.png|500]]


## Bidirectional Search

**Idea**: perform two parallel searches from start and goal state 同时从起点和目标处进行搜索，两者相交时就证明找到了最优解。

- Complete:if b is finite
- Optimal:if costs are uniform
- Runtime Complexity:$O\left(b^{d/2}\right)$
- Space Complexity: $O\left(b^{d/2}\right)$

## Overview of Uninformed Search Algorithms

![Untitled](00%20Computer%20Science/00%20Grundlagen%20der%20Künstlichen%20Intelligenz/Images/Untitled%206.png)

PS：

- a -Superscripts a complete if b is finite
- b - complete if all step costs are greater than some $\varepsilon$ >0 .
- c - optimal if all costs are uniform

# Informed Search

What is informed search? - **Informed search methods** take problem-specific information into account in order to guide the search by estimating „how far away from the goal“ the agent is.

## Heuristic function
**Heuristic Function**: 可以保证找到好的解决方法，但是不能保证是最优的。
本质就是一个量化当前状态到目标的差距。比如地图问题，就采用了当前点到目的点的欧式距离（直线距离）作为Heuristic function。每到一个点，当前的heuristic value都不一样，用这个value就可以衡量现在离goal有多远，而不是采用全局搜索把所有的情况都遍历一遍。[参考视频](https://youtu.be/5F9YzkpnaRw)

Uninformed algorithms were guided by the **path cost function** (calculate the cost t to reach node n **from initial node**), but informed algorithm uses **heuristic function**(calculate the cost from node n **to goal node**) 
## Best-First Search(Greedy Search)

![Untitled|500](00%20Computer%20Science/00%20Grundlagen%20der%20Künstlichen%20Intelligenz/Images/Untitled.png)

Choose node with **minimum** value for some evaluation function. It needs to use priority queue.
**贪心算法用的是Priority queue**。

- Completeness: Graph-search version is complete in finite spaces总能找到解
- Optimality: Greedy best-first search does not guarantee optimality**不能保证最优解**
- Time complexity and space complexity: The worst-case time and space complexity is $O(|V|)$. With a good heuristic function, however, the complexity can be reduced substantially, on certain problems reaching  $O(bm)$.




## A* search algorithm
[Video](https://youtu.be/6TsL96NAZCo)
- A\*一定能找到最优解，证明过程不需要了解
- A\* 也是用的priority queue, 需要算出当前结点下的heuris function的值，然后再进行大小排列，选择最小的拓展。
Evaluation function: **path cost function + heuristic function**
与Dijkstra算法不同的是，A\*考虑的是整体的cost，到当前结点需要花去的费用以及通过heuristic fucntion预估到达目的地的费用，两者相加便是A\*的cost。但是Dijkstra只考虑了path cost function。而Greedy best-first 只考虑了 heuristic function。

Greedy best-first: heuristic function
Dijkstra: path cost function
A\*: path cost function + heuristic function

Example for A\* search:

![Example for A* search](00%20Computer%20Science/00%20Grundlagen%20der%20Künstlichen%20Intelligenz/Images/Untitled%207.png)



- Completeness and optimality: A\*search is complete and optimal whenever
    - step cost is always greater than some e>0
    - h is **admissible**:it never overestimates the actual cost to a goal node
	    - $0\leq h \leq cost$ 只要不超出cost都是admissble。可能会考如何判断admissble.

在部分情况下A\*是不能保证完备性和最优性的：
Condition1: Vanishing costs in infinite graphs
![[00 Computer Science/00 Grundlagen der Künstlichen Intelligenz/Images/Untitled 8.png#inlL|Vanishing costs in infinite graphs|300]]可以看到每一个结点预估的成本都是实际的1/2，虽然低估(underestimate)但依然是可接受的(admissible)，但由于cost不断减小，导致最后evaluate value依然小于最优值，而无法达到最后的goal，因此在这种情况下不能保证completition.

出现的原因是：
step cost is not always greater than some e>0，应该总是大于1/2






Condition 2: Overestimation
![[00 Computer Science/00 Grundlagen der Künstlichen Intelligenz/Images/Untitled 9.png#inlL|Overestimation|300]]在这种情况下，由于heuristic value是高估的，导致不能保证optimal。
Reason: h is not admissible.