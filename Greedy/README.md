## Greedy

贪心算法是一种在**每一步选择**中都采取**在当前状态下最好或最优**（即最有利）的选择，从而希望导致结果是最好或最优的。是**用局部最优解决定全局最优解**的算法

> The idea of greedy algorithm is to pick the *locally* optimal move at each step, that will lead to the *globally* optimal solution.

贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。

贪心算法可以解决一些最优化问题，典型问题为[最小生成树问题](Minimum Spanning Tree/README.md)

