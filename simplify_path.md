# Simplify Path

## Problem Statement
Given an **absolute Unix-style path**, simplify it to its **canonical form** by following these rules:

1. A **single period (`.`)** refers to the **current directory** and should be ignored.
2. A **double period (`..`)** refers to the **parent directory** and should remove the previous directory.
3. Multiple consecutive **slashes (`/`)** should be treated as a **single slash (`/`)**.
4. Any sequence of periods **not exactly `.` or `..`** is treated as a valid directory or file name (e.g., `...` is a valid name).
5. The **simplified path must:**
   - Start with a single `/`.
   - Have directories separated by exactly one `/`.
   - Not end with `/`, unless the path is the root directory.
   - Not contain `.` or `..`.

---

### Example Cases
#### Example 1
**Input:**
```python
path = "/home/"
```
**Output:**
```python
"/home"
```
**Explanation:** The trailing slash should be removed.

#### Example 2
**Input:**
```python
path = "/home//foo/"
```
**Output:**
```python
"/home/foo"
```
**Explanation:** Multiple consecutive slashes are replaced by a single one.

#### Example 3
**Input:**
```python
path = "/home/user/Documents/../Pictures"
```
**Output:**
```python
"/home/user/Pictures"
```
**Explanation:** `..` moves up one level from `Documents` to `user`.

#### Example 4
**Input:**
```python
path = "/../"
```
**Output:**
```python
"/"
```
**Explanation:** Going up one level from the root directory is not possible.

#### Example 5
**Input:**
```python
path = "/.../a/../b/c/../d/./"
```
**Output:**
```python
"/.../b/d"
```
**Explanation:** `...` is treated as a valid directory name.

### Constraints
- `1 <= path.length <= 3000`
- `path` consists of **English letters, digits, periods (`.`), slashes (`/`), or underscores (`_`).`
- `path` is a **valid absolute Unix path**.

---

## Solution Approach
The best way to solve this problem is to use a **stack**:
1. **Split** the path by `/` to get directory components.
2. **Iterate over components:**
   - Ignore `.` (current directory).
   - If `..` (parent directory), **pop from stack** (if not empty).
   - Otherwise, **push valid directory names onto the stack**.
3. **Construct the simplified path** by joining stack elements with `/`.
4. Ensure the **result starts with `/`** and contains **no trailing slash** (unless it's root).

This approach runs in **O(N) time** and **O(N) space**.

---

## Optimized Python Solution
```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        """
        Simplifies a Unix file path to its canonical form.
        """
        stack = []  # Stack to store valid directory names
        components = path.split("/")  # Split path into parts
        
        for component in components:
            if component == "" or component == ".":
                # Ignore empty parts (from consecutive slashes) and current dir references
                continue
            elif component == "..":
                # Go up one directory if possible
                if stack:
                    stack.pop()
            else:
                # Valid directory name, add to stack
                stack.append(component)
        
        # Join stack to create simplified path
        return "/" + "/".join(stack)
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, since we iterate over the `path` once and process each component efficiently.
- **Space Complexity:** `O(N)`, due to storing directory names in a stack.

---

## Interview Tips
- **Edge Cases to Consider:**
  - **Root directory only (`/`)** should remain `/`.
  - **Multiple slashes (`////home///foo/`)** should be converted to `/home/foo`.
  - **Paths with multiple `..` (`/a/b/c/../../d`)** should properly move up the hierarchy.
  - **Edge case of `/a/../..` should return `/` (can't go above root).**

- **Common Mistakes:**
  - Forgetting to remove multiple consecutive slashes.
  - Incorrectly handling `..` when already at the root (`/../` should return `/`).
  - Using `.split("/")` without filtering out empty parts correctly.

- **Follow-up Questions:**
  - How would you modify this function to handle **relative paths** instead of absolute paths?
  - Can you adapt this function to **Windows-style paths (`C:\Users\Documents\..\Pictures`)**?
  - What if we need to **simplify paths dynamically** (streaming input)?
