# Binary Tree Paths

## Problem Statement
Given the **root** of a binary tree, return all **root-to-leaf paths** in any order.

A **leaf** is a node with no children.

### Example Cases
#### Example 1
**Input:**
```python
root = [1,2,3,null,5]
```
**Output:**
```python
["1->2->5","1->3"]
```

#### Example 2
**Input:**
```python
root = [1]
```
**Output:**
```python
["1"]
```

### Constraints
- The number of nodes in the tree is in the range `[1, 100]`.
- `-100 <= Node.val <= 100`

---

## Solution Approach

This problem can be efficiently solved using **Depth-First Search (DFS)**. We traverse the tree from the root to the leaves while maintaining the path. When a leaf node is reached, we add the current path to the result list.

### **Algorithm Steps:**
1. Use a recursive **DFS** approach to traverse the tree.
2. Maintain a **path string** that records nodes visited from root to the current node.
3. If a **leaf node** is reached, add the constructed path to the result list.
4. If a node has **left or right children**, recursively call DFS on them while appending their values to the path.
5. Return the collected paths once traversal is complete.

---

## Optimized Python Solution
```python
from typing import Optional, List

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        """
        Returns all root-to-leaf paths in a binary tree.
        """
        result = []
        
        def dfs(node, path):
            if not node:
                return
            
            # Add the current node value to the path
            path += str(node.val)
            
            # If it's a leaf node, add the full path to the result
            if not node.left and not node.right:
                result.append(path)
            else:
                # Otherwise, continue traversal
                path += "->"
                dfs(node.left, path)
                dfs(node.right, path)
        
        # Start DFS traversal from the root
        dfs(root, "")
        return result

# Example Usage
solution = Solution()
example_root = TreeNode(1, TreeNode(2, None, TreeNode(5)), TreeNode(3))
print(solution.binaryTreePaths(example_root))  # Output: ["1->2->5","1->3"]
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, where `N` is the number of nodes in the tree, as each node is visited once.
- **Space Complexity:** `O(N)`, due to the recursive call stack in the worst case (a skewed tree).

---

## Interview Tips
- **Edge Cases to Consider:**
  - Single-node trees (`root` with no children).
  - Trees where all nodes only have left children or only right children.
  - A tree with all negative values.

- **Common Mistakes:**
  - Forgetting to check for `None` before accessing node properties.
  - Not handling trees with a single root node correctly.
  - Not correctly appending `->` between nodes in the path.

- **Follow-up Questions:**
  - How would you modify the solution to return paths as lists of node values instead of strings?
  - Can you solve this iteratively using a stack instead of recursion?
  - How would you modify this approach for a very deep tree (e.g., tail recursion or BFS)?
