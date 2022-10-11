# List of Algorithms

[List of algorithms](https://en.wikipedia.org/wiki/List_of_algorithms)
[Top 25 Algorithms Every Programmer Should Know](https://medium.com/techie-delight/top-25-algorithms-every-programmer-should-know-373246b4881b)
[Top 10 Algorithms](https://cs.gmu.edu/~henryh/483/top-10.html)

# Backtracking

[Difference between backtracking and recursion?](https://stackoverflow.com/questions/26670757/difference-between-backtracking-and-recursion)
[Introduction to Backtracking](https://www.geeksforgeeks.org/introduction-to-backtracking-data-structure-and-algorithm-tutorials/#:~:text=Difference%20between%20Recursion%20and%20Backtracking,best%20result%20for%20the%20problem.)

## Difference between Backtracking and Recursion

**Recursion** -

-   The problem can be broken down into smaller problems of same type.
    
-   Base case is required.
    
-   Is like a bottom-up process. **You can solve the problem just by using the result of the sub-problem**.
    

**Backtracking** -

-   General algorithmic technique that takes in all the possible combination to solve a computational problem.
    
-   Is still like a top-down process. Sometimes you can't solve the problem just by using the result of the sub-problem, **you need to pass the information you already got to the sub-problems**. The answer(s) to this problem will be computed at the lowest level, and then **these answer(s) will be passed back to the problem with the information you got along the way.**
    

## Backtrack on a bad leaf to continue the search
"Conceptually, you start at the root of a tree; **the tree probably has some good leaves and some bad leaves, though it may be that the leaves are all good or all bad**. You want to get to a good leaf. At each node, beginning with the root, you choose one of its children to move to, and you keep this up until you get to a leaf.

**Suppose you get to a bad leaf. You can backtrack to continue the search for a good leaf by revoking your most recent choice**, and trying out the next option in that set of options. If you run out of options, revoke the choice that got you here, and try another choice at that node. If you end up at the root with no options left, there are no good leaves to be found."

## Top of the call stack

[Neatcode](https://neetcode.io/practice) | [Subsets](https://leetcode.com/problems/subsets/) | [Github](https://github.com/ethanimproving/CodeWars/blob/master/src/main/java/com/leetcode/backtracking/Subsets.java)

Every time you make a method call, the JVM allocates a bucket of memory in Eden Space called a frame for all of the method arguments and the local variables this method will store. Imagine when you're debugging and you choose the option, "Drop a frame" to move backwards in the code. It's the same thing as saying, "Take me back to the previous method that called this method."

![](https://i.ibb.co/TbBkQR7/image.png)

When you are recursively calling a method, eventually you make it to the top of the call stack (when the conditions of your base case are satisfied), and you return a value to the previous call. The call stack begins to decrease again. You're going back to the previous node to continue the search.

![](https://i.ibb.co/tBLpzZk/image.png)

``` java
public List<List<Integer>> subsets(int[] nums) {  
    var ans = new ArrayList<List<Integer>>();  
    var subset = new ArrayList<Integer>();  
    helper(ans, 0, nums, subset);  
    return ans;  
}  
  
public void helper(List<List<Integer>> ans, int callStack, int[] nums, List<Integer> subset) {  
    int callStackBoundary = nums.length;  
    if (callStack >= callStackBoundary) {  
        /* Add subset to result. Then eliminate the branch and go back to the level before. */  
        ans.add(new ArrayList<>(subset));  
    } else {  
        /* Add the element and start the recursive call.  
         * All of these will be called nums.length times before ever reaching         * the backtracking calls, adding all the elements one at a time.         * Once it reaches the top of the call stack, it will add the subset,         * remove the last element, and add all the following backtracking subsets. */        subset.add(nums[callStack]); // Add the index of whichever call stack you are on.  
        helper(ans, callStack + 1, nums, subset);  
        /* Remove the last element and do the second call. */  
        subset.remove(subset.size() - 1);  
        helper(ans, callStack + 1, nums, subset);  
    }  
}
```

## Decision Tree

[Neatcode](https://neetcode.io/practice) | [Combination Sum](https://leetcode.com/problems/combination-sum/) | [Github](https://github.com/ethanimproving/CodeWars/blob/master/src/main/java/com/leetcode/backtracking/CombinationSum.java)

![](https://i.ibb.co/4N1Cg3T/image.png)

``` java
public List<List<Integer>> combinationSum(int[] candidates, int target) {  
    var listOfCombinationsThatSumToTarget = new ArrayList<List<Integer>>();  
    var currentSetBeingEvaluated = new ArrayList<Integer>();  
    backtrack(candidates, target, listOfCombinationsThatSumToTarget, currentSetBeingEvaluated, 0);  
    return listOfCombinationsThatSumToTarget;  
}  
  
public void backtrack(int[] candidates, int target, List<List<Integer>> ans, List<Integer> currentSetBeingEvaluated, int index) {  
    if (target == 0) { // Edge case  
        ans.add(new ArrayList<>(currentSetBeingEvaluated));  
    } else if (target < 0 || index >= candidates.length) {  
        return; // If target is less than 0, or if the index is out of bounds, backtrack to the previous node and continue searching.  
    } else {  
        /* FIRST DECISION IN DECISION TREE. Duplicate candidate. */  
        currentSetBeingEvaluated.add(candidates[index]); // Add the current index to set  
        backtrack(candidates, target - candidates[index], ans, currentSetBeingEvaluated, index); // Update target minus sum of current set  
  
        /* SECOND DECISION IN DECISION TREE. Remove candidates. */        currentSetBeingEvaluated.remove(currentSetBeingEvaluated.get(currentSetBeingEvaluated.size() - 1)); // Remove last element that was added to ensure no duplicates on this side of the decision tree  
        backtrack(candidates, target, ans, currentSetBeingEvaluated, index + 1); // Shift index to next number in candidates  
    }  
}
```
# Recursive Depth First Search

![](https://he-s3.s3.amazonaws.com/media/uploads/9fa1119.jpg)

Check out this awesome [visualizer](https://www.hackerearth.com/practice/algorithms/graphs/depth-first-search/visualize/).



## Maximum Depth of Binary Tree

[Neatcode](https://neetcode.io/practice) | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | [Github](https://github.com/ethanimproving/CodeWars/blob/master/src/main/java/com/leetcode/tree)

![[Maximum Depth of Binary Tree.png]]

``` java
public int maxDepth(TreeNode root) {  
    if (root == null) return 0;  
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));  
}
```

## [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree)

![[Pasted image 20221005221241.png]]

![[Diameter of Binary Tree 1.jpg]]

``` java
int result = -1;  
  
public int diameterOfBinaryTree(TreeNode root) {  
    dfs(root);  
    return result;  
}  
  
private int dfs(TreeNode current) {  
    if (current == null) {  
        return -1;  
    }  
    int left = 1 + dfs(current.left);  
    int right = 1 + dfs(current.right);  
    result = Math.max(result, (left + right));  
    return Math.max(left, right);  
}
```

### [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree)
[Neatcode](https://neetcode.io/practice) | [Github](https://github.com/ethanimproving/CodeWars/blob/master/src/main/java/com/leetcode/tree)

![[Balanced Binary Tree.png]]

``` java
public static boolean isBalanced(TreeNode root) {  
    return dfs(root).getValue0();  
}  
  
private static SimpleEntry<Boolean, Integer> dfs(TreeNode root) {  
    if (root == null) {  
        return new SimpleEntry<>(true, 0);  
    }  
  
    var left = dfs(root.left);  
    var right = dfs(root.right);  
  
    var balanced =  
            left.getKey() &&  
                    right.getKey() &&  
                    (Math.abs(left.getValue() - right.getValue()) <= 1);  
  
    return new SimpleEntry<>(  
            balanced,  
            1 + Math.max(left.getValue(), right.getValue())  
    );  
}
```

## [Same Tree](https://leetcode.com/problems/same-tree/)

![[Same Tree.png]]

``` java
public boolean isSameTree(TreeNode p, TreeNode q) {  
    if (p == null && q == null) return true;  
    if (p == null || q == null) return false;  
    if (p.val != q.val) return false;  
  
    boolean left = isSameTree(p.left, q.left);  
    boolean right = isSameTree(p.right, q.right);  
  
    return left && right;  
}
```

# Iterative Depth First Search

# Breadth First Search

# Two pointer Technique

# Heap / Priority Queue

## [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

![[Pasted image 20221010212804.png]]

``` java
private Queue<Integer> smallHeap = new PriorityQueue<>(Collections.reverseOrder());  
private Queue<Integer> largeHeap = new PriorityQueue<>();  
private boolean even = true;  
  
public double findMedian() { // O(1)  
    if (even) return (smallHeap.peek() + largeHeap.peek()) / 2.0;  
    else return smallHeap.peek();  
}  
  
public void addNum(int num) { // O(log n)  
    if (even) { // If even, add to largeHeap and poll smallest number to smallHeap  
        largeHeap.offer(num);  
        smallHeap.offer(largeHeap.poll());  
    } else {  
        smallHeap.offer(num); // Otherwise add to smallHeap and move largest number to largeHeap  
        largeHeap.offer(smallHeap.poll());  
    }  
    even = !even;  
}
```

The invariant of the algorithm is two heaps, small and large, each represent half of the current list. The length of smaller half is kept to be n / 2 at all time and the length of the larger half is either n / 2 or n / 2 + 1 depend on n's parity.

This way we only need to peek the two heaps' top number to calculate median.

Any time before we add a new number, there are two scenarios, (total n numbers, k = n / 2):

```lisp
(1) length of (small, large) == (k, k)
(2) length of (small, large) == (k, k + 1)
```

After adding the number, total (n + 1) numbers, they will become:

```lisp
(1) length of (small, large) == (k, k + 1)
(2) length of (small, large) == (k + 1, k + 1)
```

Here we take the first scenario for example, we know the large will gain one more item and small will remain the same size, but we cannot just push the item into large. What we should do is we push the new number into small and pop the maximum item from small then push it into large (all the pop and push here are heappop and heappush). By doing this kind of operations for the two scenarios we can keep our invariant.

Therefore to add a number, we have 3 O(log n) heap operations. Luckily the heapq provided us a function "heappushpop" which saves some time by combine two into one. The document says:

> Push item on the heap, then pop and return the smallest item from the heap. The combined action runs more efficiently than heappush() followed by a separate call to heappop().

Alltogether, the add operation is O(logn), The findMedian operation is O(1).

A further observation is that the two scenarios take turns when adding numbers, thus it is possible to combine the two into one. For this please see [stefan's post](https://leetcode.com/discuss/64910/very-short-o-log-n-o-1)