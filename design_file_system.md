# Design File System

## Problem Statement
You are asked to design a **file system** that allows you to:
- **Create new paths** and associate them with integer values.
- **Retrieve values** associated with existing paths.

### **Rules:**
1. A valid path consists of one or more **concatenated strings** of the form:
   - `"/"` followed by **one or more lowercase English letters**.
   - Example: `"/leetcode"` and `"/leetcode/problems"` are valid paths.
   - Invalid paths: `""` (empty string) and `"/"` (root path alone).
2. **Operations:**
   - `createPath(path: str, value: int) -> bool`
     - Creates a new path with a given value if **the parent path exists**.
     - Returns `True` on success, `False` if the path already exists or its parent path is missing.
   - `get(path: str) -> int`
     - Returns the **value associated** with the path or `-1` if it doesn't exist.

---

### Example Cases
#### Example 1
**Input:**
```python
["FileSystem","createPath","get"]
[[],["/a",1],["/a"]]
```
**Output:**
```python
[null,true,1]
```
**Explanation:**
```python
fs = FileSystem()
fs.createPath("/a", 1)  # Returns True
fs.get("/a")  # Returns 1
```

#### Example 2
**Input:**
```python
["FileSystem","createPath","createPath","get","createPath","get"]
[[],["/leet",1],["/leet/code",2],["/leet/code"],["/c/d",1],["/c"]]
```
**Output:**
```python
[null,true,true,2,false,-1]
```
**Explanation:**
```python
fs = FileSystem()
fs.createPath("/leet", 1)  # Returns True
fs.createPath("/leet/code", 2)  # Returns True
fs.get("/leet/code")  # Returns 2
fs.createPath("/c/d", 1)  # Returns False (parent "/c" doesn't exist)
fs.get("/c")  # Returns -1 (path doesn't exist)
```

### Constraints
- `2 <= path.length <= 100`
- `1 <= value <= 10^9`
- Each path consists of **lowercase English letters** and `'/'`.
- At most **10⁴ calls** to `createPath` and `get`.

---

## Solution Approach
### **Using a HashMap (Dictionary) for Efficient Storage (O(1) Get, O(N) CreatePath)**
We need to efficiently:
- **Store paths and their values**.
- **Check parent existence** before creating a new path.

### **Implementation Using HashMap**
```python
from collections import defaultdict

class FileSystem:
    def __init__(self):
        """
        Initializes the file system using a defaultdict to store paths and values.
        """
        self.path2value = defaultdict(int)
        self.path2value[''] = -1  # Root path initialized

    def createPath(self, path: str, value: int) -> bool:
        """
        Creates a new path with an associated value if its parent exists.
        """
        dirs = path.split('/')
        parent = '/'.join(dirs[:-1])  # Extract the parent path
        if path in self.path2value or parent not in self.path2value:
            return False  # Path already exists or parent doesn't exist
        
        self.path2value[path] = value  # Store path with its value
        return True

    def get(self, path: str) -> int:
        """
        Returns the value associated with the path or -1 if it doesn't exist.
        """
        return self.path2value[path] if path in self.path2value else -1  # Return value or -1 if path not found
```

---

### **Alternative Implementation Using Trie**
Using a **Trie (Prefix Tree)** allows for **efficient hierarchical path storage and retrieval**.

```python
class TrieNode:
    def __init__(self):
        """
        Initializes a trie node with children and value storage.
        """
        self.children = {}  # Dictionary to store child nodes
        self.value = None  # Value associated with the path

class FileSystem:
    def __init__(self):
        """
        Initializes the file system using a Trie data structure.
        """
        self.root = TrieNode()
    
    def createPath(self, path: str, value: int) -> bool:
        """
        Creates a path in the trie with an associated value.
        """
        nodes = path.split('/')
        current = self.root
        
        for i in range(1, len(nodes)):
            part = nodes[i]
            if part not in current.children:
                if i == len(nodes) - 1:  # If it's the last part, create it
                    current.children[part] = TrieNode()
                else:
                    return False  # Parent path does not exist
            current = current.children[part]
        
        if current.value is not None:
            return False  # Path already exists
        
        current.value = value  # Assign the value
        return True
    
    def get(self, path: str) -> int:
        """
        Retrieves the value associated with a path.
        """
        nodes = path.split('/')
        current = self.root
        
        for i in range(1, len(nodes)):
            part = nodes[i]
            if part not in current.children:
                return -1  # Path does not exist
            current = current.children[part]
        
        return current.value if current.value is not None else -1
```

### **Explanation of Trie-Based Approach**
1. **TrieNode Class:**
   - Each node has **children (subdirectories)** and a **value** (if it's a valid path).
2. **`createPath()`**:
   - Traverses the Trie, ensuring the **parent exists before creating a new path**.
   - If the path **already exists**, returns `False`.
   - Otherwise, creates the path and stores its value.
3. **`get()`**:
   - Traverses the Trie to find the **requested path**.
   - If it exists, returns the associated **value**, else returns `-1`.

---

## Complexity Analysis
- **`createPath(path, value)`**:
  - **O(N)** (where `N` is the length of the path) for traversing or creating the trie nodes.
- **`get(path)`**:
  - **O(N)** for traversing the trie.

---

## Interview Tips
- **Common Mistakes:**
  - Forgetting to check for **parent path existence** before creating a new one.
  - Not handling cases where the **path already exists**.
  - Using an inefficient **linear scan approach** instead of a hashmap or Trie.
