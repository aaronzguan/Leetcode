## Course Schedule - Topological Sort  判断是否存在拓扑排序

现在你总共有 n 门课需要选，记为 `0` 到 `n - 1`. 一些课程在修之前需要先修另外的一些课程，比如要学习课程 0 你需要先学习课程 1 ，表示为[0,1]，给定n门课以及他们的先决条件，判断是否可能完成所有课程？

```
输入: n = 2, prerequisites = [[1,0]] 
输出: true 

输入: n = 2, prerequisites = [[1,0],[0,1]] 
输出: false
```

Topological Sort:

**Time Complexity:** O(|E|+|V|) where |V| is the number of courses, and |E| is the number of dependencies. 

**Space Complexity:** O(|E| + |V|), with the same denotation as in the above time complexity.

```python
from collections import deque
class Solution:
    """
    @param: numCourses: a total of n courses
    @param: prerequisites: a list of prerequisite pairs
    @return: the course order
    """
    def findOrder(self, numCourses, prerequisites):
        # 1. 建立图 graph: dict {prerequisite:course}
        graph = collections.defaultdict(list)
        # 2. 计算入度
        course_to_indegree = {x: 0 for x in range(numCourses)}
        for pre in prerequisites:
            graph[pre[1]].append(pre[0]) # prerequisite 作为key, course作为value加入graph中
            course_to_indegree[pre[0]] += 1 # 对有pre要求的课，入度加1
        # 3. 得到入度为0的课程，则该课程没有prerequisite的要求，可以先上
        start_course = [c for c in range(numCourses) if course_to_indegree[c] == 0]
        
        topo_order = []
        if start_course == []:
            return topo_order # return False
        # 4. 将每个入度为0的点放入队列（Queue）中作为起始节点
        queue = deque(start_course)
        nums_course = 0
        while queue:
            # 5. 从队列中拿出最前的点
            course = queue.popleft()
            nums_course += 1 
            topo_order.append(course)
            if nums_course == numCourses:
                return topo_order # return True
            # 6. 去掉这个点的所有连边，即将该点的neighbor的相应入度 -1
            for c in graph[course]:
                course_to_indegree[c] -= 1 
                # 7. 发现新的入度为0的点，丢回队列中
                if course_to_indegree[c] == 0:
                    queue.append(c)
        return [] # return False
```

