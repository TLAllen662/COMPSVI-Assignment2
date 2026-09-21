# Written Analysis

## Complexity Analysis (200-250 words)

Let $N$ be the total number of filesystem entries (files and directories), and let $H$ be the maximum directory depth. For `count_files()`, the recurrence for a directory containing children with subtree sizes $n_1, n_2, ..., n_k$ is

$$T_c(N) = \Theta(k) + \sum_{i=1}^{k} T_c(n_i),$$

with the base case $T_c(1) = \Theta(1)$ for a file. The $\Theta(k)$ term accounts for listing and iterating through the directory's children. Since every file and directory is visited once, the costs across all recursive calls add to $\Theta(N)$, so `count_files()` has time complexity $O(N)$ (more precisely, $\Theta(N)$ under the usual constant-cost filesystem-operation assumption).

For `find_infected_files()`, the recurrence is

$$T_f(N) = \Theta(k) + \sum_{i=1}^{k} T_f(n_i),$$

with a file base case of $\Theta(1)$ for checking its extension and returning either an empty or one-item list. It also visits every entry exactly once, so its time complexity is $O(N)$, or $\Theta(N)$ under the same assumption. The returned list can contain up to $F$ matching files, requiring $O(F)$ output space. In both functions, the recursion call stack uses $O(H)$ space, because only one path from the root to the current entry is active at a time. These results make sense because branching changes how work is distributed among calls, but no entry is revisited; the total work is therefore proportional to the filesystem tree's size, while memory is governed by its height.

## Reflection

The company file system contains **21,673 files**, of which **6,468** are infected because they have the `.encrypted` extension. Among the department directories, HR has the most infected files, with **1,900**, compared with **813** in Sales and **794** in Finance. There are also **2,961** infected files elsewhere in the breach-data tree, outside those three department directories. HR's much larger count suggests that the breach spread especially widely through HR's nested folders, or that HR had more accessible or heavily used shared files. It does not prove the original entry point, but it identifies HR as the area needing the most urgent investigation and containment.

This problem is naturally suited to recursion because a file system is a tree: a directory contains files and other directories, and each subdirectory has the same structure as the whole. The functions can handle one entry, then apply the same logic to every child directory until reaching a file, which is the base case. This mirrors the data model and keeps the traversal logic readable without separately tracking every nesting level.

I would choose iteration instead when directory depth could be extremely large or uncontrolled. A recursive implementation consumes call-stack space proportional to depth and could hit Python's recursion limit, while an explicit stack or queue avoids that limit and gives more control over memory. Iteration may also be preferable when performance matters because it avoids function-call overhead. For ordinary, moderately sized directory trees, recursion is concise and communicates the hierarchy clearly; for production tools scanning untrusted or very deep paths, iteration is generally safer and more robust.
