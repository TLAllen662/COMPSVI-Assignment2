# Written Analysis

## Complexity Analysis (200-250 words)

Let $N$ be the total number of filesystem entries (files and directories), and let $H$ be the maximum directory depth. For `count_files()`, the recurrence for a directory containing children with subtree sizes $n_1, n_2, ..., n_k$ is

$$T_c(N) = \Theta(k) + \sum_{i=1}^{k} T_c(n_i),$$

with the base case $T_c(1) = \Theta(1)$ for a file. The $\Theta(k)$ term accounts for listing and iterating through the directory's children. Since every file and directory is visited once, the costs across all recursive calls add to $\Theta(N)$, so `count_files()` has time complexity $O(N)$ (more precisely, $\Theta(N)$ under the usual constant-cost filesystem-operation assumption).

For `find_infected_files()`, the recurrence is

$$T_f(N) = \Theta(k) + \sum_{i=1}^{k} T_f(n_i),$$

with a file base case of $\Theta(1)$ for checking its extension and returning either an empty or one-item list. It also visits every entry exactly once, so its time complexity is $O(N)$, or $\Theta(N)$ under the same assumption. The returned list can contain up to $F$ matching files, requiring $O(F)$ output space. In both functions, the recursion call stack uses $O(H)$ space, because only one path from the root to the current entry is active at a time. These results make sense because branching changes how work is distributed among calls, but no entry is revisited; the total work is therefore proportional to the filesystem tree's size, while memory is governed by its height.
