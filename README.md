# Coding Interview Preparation

This repository contains solutions and reference materials for various coding interview problems. Each problem is documented in its own Markdown file, providing a quick reference to the problem description, solution, and implementation details.

## Problems to Solve

Below is the list of problems we are working on, with links to corresponding Markdown files. Those marked with **[TODO]** still need documentation.

1. [Count Primes](count_primes.md)  
   - Counts the number of prime numbers less than `n` using the Sieve of Eratosthenes for optimal efficiency.
2. [Text Justification](text_justification.md)  
   - Formats a list of words into fully justified text, distributing spaces evenly and left-justifying the last line.
3. [Rotating the Box](rotate_box.md)  
   - Simulates gravity on falling stones before rotating the box 90 degrees clockwise.
4. [Spiral Matrix](spiral_matrix.md)  
   - Extracts elements from a matrix in a spiral order by iteratively shrinking the boundaries.
5. [Meeting Scheduler](meeting_scheduler.md)  
   - Finds the earliest available time slot for a meeting using a two-pointer or heap-based approach.
6. [Two Sum Less Than K](two_sum_less_than_k.md)  
   - Finds the maximum sum of two numbers that is less than `k` using a sorted two-pointer approach.
7. [Candy Crush](candy_crush.md)  
   - Implements a simulation of Candy Crush, repeatedly crushing adjacent candies and applying gravity until the board stabilizes.
8. [Binary Tree Paths](binary_tree_paths.md)  
   - Finds all root-to-leaf paths in a binary tree using Depth-First Search (DFS).
9. [Longest Substring Without Repeating Characters](longest_substring_no_repeats.md)  
   - A sliding window approach to find the longest substring with unique characters efficiently.  
10. [Simple Bank System](simple_bank_system.md)  
   - Implements a banking system with transfer, deposit, and withdrawal operations using three different approaches:
        - **List-based approach**: Accounts inferred by index.
        - **Dynamic transaction processor**: Processes transactions from input lists.
        - **HashMap-based approach**: Stores accounts with explicit account IDs for flexibility.
11. [Block Placement Queries](block_placement_queries.md)  
    - Efficiently handles obstacle placements and block placement queries on an infinite number line using a **SortedList** and **gap tracking**. Queries are processed in reverse order to maintain increasing gaps dynamically.
12. [Count Operations to Obtain Zero](count_operations_to_zero.md)  
    - Uses a greedy approach to repeatedly subtract the smaller number from the larger one until reaching zero. Optimized with integer division for efficiency, similar to the Euclidean algorithm for GCD.
13. [Longest Continuous Subarray With Absolute Diff ≤ Limit](longest_continuous_subarray.md)  
    - Uses a **sliding window** approach with **monotonic deques** to efficiently track the min and max values in a subarray. Runs in **O(N) time**, ensuring optimal performance for large inputs.
14. [Simplify Path](simplify_unix_path.md)  
    - Uses a **stack-based approach** to simplify Unix-style file paths, efficiently handling `"."`, `".."`, and multiple slashes. Runs in **O(N) time**, ensuring optimal path normalization.
15. [Best Time to Buy and Sell Stock](best_time_to_buy_sell_stock.md)  
    - Uses a **greedy approach** to track the minimum price and maximize profit in a **single pass (O(N))**. Optimized for large inputs with **O(1) space complexity**.
16. [Best Time to Buy and Sell Stock II](best_time_to_buy_sell_stock_ii.md)  
    - Uses a **greedy approach** to maximize profit by summing all increasing segments in stock prices. Runs in **O(N) time**, ensuring efficient handling of large inputs.
17. [Best Time to Buy and Sell Stock III](best_time_to_buy_sell_stock_iii.md)  
    - Uses a **dynamic programming approach** to track two transactions efficiently, ensuring an **O(N) time complexity** with **O(1) space usage** for optimal profit calculation.
18. [Minimum Operations to Write the Letter Y on a Grid](minimum_operations_write_y.md)  
    - Uses **grid traversal and frequency counting** to determine the minimum transformations needed to form the letter Y. Runs in **O(n²) time** with an **efficient counting-based approach**.  
19. [Palindrome Number](palindrome_number.md)  
    - Checks whether an integer is a palindrome **without converting it to a string**. Uses an **O(log N) time** and **O(1) space** approach by reversing half of the number for comparison.
20. [Rotate Image](rotate_image.md)  
    - Rotates an `n x n` matrix **90 degrees clockwise in-place** using **matrix transposition and row reversal**. Runs in **O(N²) time** with **O(1) space complexity**.
21. [Find the Length of the Longest Common Prefix](longest_common_prefix_length.md)  
    - Uses **sorting and binary search** to efficiently find the longest common prefix between numbers in two arrays. Runs in **O(N log N + M log M) time**, significantly improving over brute force approaches.
22. [Add Strings](add_strings.md) **[TODO]**  
23. [Remove Boxes](remove_boxes.md) **[TODO]**  
24. [Non-overlapping Intervals](non_overlapping_intervals.md) **[TODO]**  
25. [Number of Islands](number_of_islands.md)  
    - A solution to count the number of islands in a 2D grid using DFS and BFS approaches.  
26. [Largest Rectangle in Histogram](largest_rectangle_histogram.md) **[TODO]**  
27. [Design File System](design_file_system.md) **[TODO]**  
28. [Meeting Rooms II](meeting_rooms_ii.md)  
    - A method to determine the minimum number of meeting rooms required, using a Min-Heap and sorting.  

---

## Making Your Interview Code "Production-Ready"

Whether you’re coding a banking system, an inventory tracker, or any other service in an interview, you can demonstrate a “production mindset” by incorporating the following practices. These suggestions are purposely generic, so you can adapt them to nearly any coding question.

### Key Topics
- **Validation & Error Handling**
- **Thread-Safety & Concurrency**
- **Logging & Monitoring**
- **Testing**
- **Documentation & Clean Code**
- **Configuration / Environment**
- **Extensibility & Maintainability**

Refer to the [Production-Ready Code Guide](production_ready_code.md) for a deeper dive into best practices for writing interview code that closely mimics real-world software development.

---

### **Usage**
Each problem’s Markdown file includes:
- **Problem Description**: Explanation of the problem, constraints, and expected output.
- **Code Implementation**: Optimized Python solution with explanations.
- **Complexity Analysis**: Breakdown of time and space complexity.
- **Interview Tips**: Key insights, edge cases, and common optimizations.

---

### **Contributions**
Feel free to fork this repository, add your own solutions, or suggest improvements!

### **License**
This repository is available under the MIT License. See `LICENSE` for details.

