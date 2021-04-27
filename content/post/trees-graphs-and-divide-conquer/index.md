---
title: Trees, Graphs and Divide Conquer
subtitle: Read in 20 minutes
date: 2021-04-27T06:24:12.885Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
Two questions

1. How to find the shortest paths when we want to find the target thing, based on item correspondence？
2. Give you two glasses of water without scale, and only know the capacity, we also have infinite water to use, please give us a solution that can get our target water in the glass, e.g. we hope to get 20 water after some operation, and the capacity of the two glasses of water are 90 and 40, please print the process of our operation.

```python
"""import package and define a function for public use"""
from collections import defaultdict
import pickle
def search(start, goal_f, successor_f, sort_f=lambda a: a):
    explored = set()
    paths = [[start]]
    while paths:
        # path = paths.pop(-1)
        # if we expand latest: Depth First Search(DFS)
        path = paths.pop(0)#use BFS to get the oldest path
        # if we expand oldest: Breadth First Search(BFS)
        if goal_f(path): 
            return path
        frontier = path[-1]# get the position that we need to do next step. e.g. path = ['book', '=>', 'chair']
        # and frontier = "chair", then we will continue our next step, based on "chair"
        if frontier in explored: # we don't need to do repeat search, because we will add all search result in every choice
            continue
        for (state, action) in successor_f(frontier).items():# add the next step
            if state in explored: 
                continue
            new_path = path + [action, state]
            paths.append(new_path)

        paths = sorted(paths, key=sort_f)# we set a rule that which solution we want, if we would like to find the
        #shortest path, we can sort the paths by length
        explored.add(frontier)
    return []
```

```python
"""First question and solution"""
def path_goal(goal):# estimate if the path includes the goal, if we acheive the goal, we will stop.
    def _wrap(path):
        return goal in path
    return _wrap

def path_successor(mapping):# list all choice about our next step.
    def _wrap(state):
        return {node: '=>' for node in mapping[state]}
    return _wrap

map = {
    "book": "chair clock bathroom".split(),
    "bathroom": "car taxi".split(),
    "taxi": "beef book".split(),
    "swim": "beef chair".split(),
    "class": "car".split(),
}
bimapping = defaultdict(set)
#create a dict, which will not have the problem that the key not in the dict,because this function give initial value
for rest, links in map.items():
    bimapping[rest] |= set(links)
# |= this symbol means bimapping[rest] | set(links), it will choose intersection between them. If the first is 
#["a","b"] and the second is ["a","c"], the answer will be ["a","b","c"]
    for link in links:
        bimapping[link] |= {rest}
print(bimapping)
# output:defaultdict(<class 'set'>, {'book': {'taxi', 'bathroom', 'clock', 'chair'}, 'chair': {'swim', 'book'}, 
#'clock': {'book'}, 'bathroom': {'taxi', 'car', 'book'}, 'car': {'class', 'bathroom'}, 'taxi': {'book', 'beef', 'bathroom'}, 
#'beef': {'taxi', 'swim'}, 'swim': {'beef', 'chair'}, 'class': {'car'}})


result = search(start='book', goal_f=path_goal('class'), successor_f=path_successor(bimapping), sort_f=lambda a: len(a))
print(result)
# output: ['book', '=>', 'bathroom', '=>', 'car', '=>', 'class']
```

```python
"""Second question and solution"""
def w_successor(A, B):# list all possible operation about the next step
    def _successor(state):
        a, b = state
        return {
            (0, b): '清空a',
            (a, 0): '清空b',
            (A, b): '灌满a',
            (a, B): '灌满b',
            (0, a + b) if a + b <= B else (a + b - B, B): 'a => b',
            (a + b, 0) if a + b <= A else (A, a + b - A): 'b => a'
        }
    return _successor

def reach_capacity(goal):
    def _wrap(path):
        return goal in path[-1]
    return _wrap

solutions = search(start=(0, 0), goal_f=reach_capacity(60),
                       successor_f=w_successor(90, 40), sort_f=lambda p: len(p))
print(solutions)
# output: [(0, 0), '灌满a', (90, 0), 'a => b', (50, 40), '清空b', (50, 0), 'a => b', (10, 40), 
# '清空b', (10, 0), 'a => b', (0, 10), '灌满a', (90, 10), 'a => b', (60, 40)]
```