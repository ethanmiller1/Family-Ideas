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


Stack-based. Children are visited before the siblings.

### [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

![[Pasted image 20221026191525.png]]

``` java
public int iterativeDfs(TreeNode root) {  
    if (root == null) {  
        return 0;  
    }  
  
    Stack<TreeNode> stack = new Stack<>();  
    Stack<Integer> depth = new Stack<>();  
    stack.push(root);  
    depth.push(1);  
    int max = 0;  
    while (!stack.isEmpty()) {  
        TreeNode node = stack.pop();  
        int currentDepth = depth.pop();  
        max = Math.max(currentDepth, max);  
        if (node.left != null) {  
            stack.push(node.left);  
            depth.push(currentDepth + 1);  
        }  
        if (node.right != null) {  
            stack.push(node.right);  
            depth.push(currentDepth + 1);  
        }  
    }  
    return max;  
}
```

# Breadth First Search

### [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

Queue-based, level by level. Siblings are visited before the children.

![[Pasted image 20221026190308.png]]

``` java
public int iterativeBfs(TreeNode root) {  
    int depth = 0;  
    if (root == null) return depth;  
    Queue<TreeNode> queue = new LinkedList<TreeNode>();  
    queue.add(root);  
    while (!queue.isEmpty()) {  
        int parents = queue.size();  
        depth++;  
        while (parents > 0) {  
            TreeNode temp = queue.poll();  
            if (temp.left != null)  
                queue.add(temp.left);  
            if (temp.right != null)  
                queue.add(temp.right);  
            parents--;  
        }  
    }  
    return depth;  
}
```

## [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

``` java
public int orangesRotting(int[][] grid) {
	int m = grid.length, n = grid[0].length;
	Queue<int[]> queue = new LinkedList<>();
	int fresh = 0;

	for (int i = 0; i < m; i += 1) {
		for (int j = 0; j < n; j += 1) {
			if (grid[i][j] == 2) queue.offer(new int[] { i, j }); else if (
				grid[i][j] == 1
			) fresh += 1;
		}
	}

	int count = 0;
	int[][] dirs = { { 1, 0 }, { -1, 0 }, { 0, 1 }, { 0, -1 } };
	while (!queue.isEmpty() && fresh != 0) {
		count += 1;
		int sz = queue.size();
		for (int i = 0; i < sz; i += 1) {
			int[] rotten = queue.poll();
			int r = rotten[0], c = rotten[1];
			for (int[] dir : dirs) {
				int x = r + dir[0], y = c + dir[1];
				if (0 <= x && x < m && 0 <= y && y < n && grid[x][y] == 1) {
					grid[x][y] = 2;
					queue.offer(new int[] { x, y });
					fresh -= 1;
				}
			}
		}
	}
	return fresh == 0 ? count : -1;
}
```

## [Word Ladder](https://leetcode.com/problems/word-ladder/)

``` java
public int ladderLength(
	String beginWord,
	String endWord,
	List<String> wordList
) {
	Map<String, List<String>> adjlist = new HashMap<>();
	wordList.add(beginWord);
	for (String word : wordList) {
		StringBuilder string = null;
		for (int i = 0; i < word.length(); i++) {
			string = new StringBuilder(word);
			string.setCharAt(i, '*');
			List<String> wordlist = adjlist.getOrDefault(
				string.toString(),
				new ArrayList<String>()
			);
			wordlist.add(word);
			adjlist.put(string.toString(), wordlist);
		}
	}

	Queue<String> queue = new LinkedList<>();
	queue.offer(beginWord);
	Set<String> visited = new HashSet<>();
	visited.add(beginWord);
	int step = 1;
	StringBuilder string = null;
	while (!queue.isEmpty()) {
		step++;
		int size = queue.size();
		for (int j = 0; j < size; j++) {
			String str = queue.poll();

			for (int i = 0; i < str.length(); i++) {
				string = new StringBuilder(str);
				string.setCharAt(i, '*');
				for (String pat : adjlist.get(string.toString())) {
					if (pat.equals(endWord)) {
						return step;
					}
					if (visited.contains(pat)) {
						continue;
					}
					queue.offer(pat);
					visited.add(pat);
				}
			}
		}
		// step++;
	}
	return 0;
}
```

## [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

``` java
public int[] findOrder(int numCourses, int[][] prerequisites) {
	Map<Integer, List<Integer>> adjList = new HashMap<Integer, List<Integer>>();
	int[] indegree = new int[numCourses];
	int[] topologicalOrder = new int[numCourses];

	//creating the adjlist
	for (int i = 0; i < prerequisites.length; i++) {
		int post = prerequisites[i][0];
		int pre = prerequisites[i][1];
		List<Integer> lst = adjList.getOrDefault(
			pre,
			new ArrayList<Integer>()
		);
		lst.add(post);
		adjList.put(pre, lst);

		indegree[post] += 1;
	}

	Queue<Integer> q = new LinkedList<Integer>();
	for (int i = 0; i < numCourses; i++) {
		if (indegree[i] == 0) {
			q.add(i);
		}
	}

	int i = 0;
	while (!q.isEmpty()) {
		int node = q.remove();
		topologicalOrder[i++] = node;

		if (adjList.containsKey(node)) {
			for (Integer neighbor : adjList.get(node)) {
				indegree[neighbor]--;

				if (indegree[neighbor] == 0) {
					q.add(neighbor);
				}
			}
		}
	}

	if (i == numCourses) {
		return topologicalOrder;
	}

	return new int[0];
}
```

## [Amazon Warehouse Problem](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

Given M x N grid
- k delivery guys are placed at any (x, y) positions
- We have an amazon warehouse W at (p, q)
- There are s stones placed at different locations on the grid, and the cell having stone cannot be visited

All of these delivery guys want to reach the warehouse; Assuming they take the best possible path; Find out who reaches the warehouse first.

```
S S S S S
D * * * *
* * * * W
* * * D *
```


Here the answer is : (3,0)

``` java
int[][] dirs = new int[][]{{0,1},{0,-1},{1,0},{-1,0}};
public int shortestPath(int[][] grid, int k) {
	int n = grid.length;
	int m = grid[0].length;
	Queue<int[]> q = new LinkedList();
	boolean[][][] visited = new boolean[n][m][k+1];
	visited[0][0][0] = true;
	q.offer(new int[]{0,0,0});
	int res = 0;
	while(!q.isEmpty()){
		int size = q.size();
		for(int i=0; i<size; i++){
			int[] info = q.poll();
			int r = info[0], c = info[1], curK = info[2];
			if(r==n-1 && c==m-1){
				return res;
			}
			for(int[] dir : dirs){
				int nextR = dir[0] + r;
				int nextC = dir[1] + c;
				int nextK = curK;
				if(nextR>=0 && nextR<n && nextC>=0 && nextC<m){
					if(grid[nextR][nextC]==1){
						nextK++;
					}
					if(nextK<=k && !visited[nextR][nextC][nextK]){
						visited[nextR][nextC][nextK] = true;
						q.offer(new int[]{nextR, nextC, nextK});
					}
				}
			}                
		}
		res++;
	}
	return -1;
}
```

``` java
public int[] findnearestDeliveryGuy( char[][] grid )
{
    var nearestDeliveryGuy = char[2];
    var warehouse = findWarehouse(grid);
    return bfs(grid, warehouse, warehouse);
}

private int[] findWarehouse( char[][] grid )
{
    for( var row : grid )
    {
        for( var coordinate : row )
        {
            if( coordinate == 'W' ) return coordinate;
        }
    }
}

private int[] bfs( char[][] grid, int[] coordinate )
{
    Queue<int[]> queue = new Deque<int[]>;
    queue
    
    
// while loop queue is not empty
// look up
// look down left right, etc
// if base case, remove from queue
// if 'D' return that coordinate
// 
    
    
    if( if x < 0 || y < 0 || y >= grid.length || x >= grid[0].length || grid[x][y] == 'S' ) return null;
    bfs( grid, x, y - 1); // down
    bfs( grid, x, y + 1); // up
    bfs( grid, x, y - 1); // left
    bfs( grid, x, y + 1); // right
    
    
}
```

# Two pointer Technique

## [Two Sum II Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

![[Pasted image 20221026194615.png]]

``` java
public int[] twoSum(int[] numbers, int target) {
    int leftPointer = 0;
    int rightPointer = numbers.length - 1;
    int leftValue, rightValue;

    while (leftPointer < rightPointer) {
        leftValue = numbers[leftPointer];
        rightValue = numbers[rightPointer];

        if (leftValue + rightValue == target) break;

        if (leftValue + rightValue < target) {
            leftPointer++;
            continue;
        }

        rightPointer--;
    }

    return new int[]{leftPointer + 1, rightPointer + 1};
}
```


## [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

![[Pasted image 20221027105058.png]]
``` java
public int trap(int[] heights) {
	int left[] = new int[heights.length], right[] = new int[heights.length], max =
		heights[0], c = 0;

	for (int i = 0; i < heights.length; i++) {
		left[i] = Math.max(heights[i], max);
		max = left[i];
	}

	max = heights[heights.length - 1];
	for (int i = heights.length - 1; i >= 0; i--) {
		right[i] = Math.max(heights[i], max);
		max = right[i];
	}

	for (int i = 0; i < heights.length; i++) {
		c = c + Math.min(left[i], right[i]) - heights[i];
	}
	return c;
}
```
# Sliding Window

## [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

![[Pasted image 20221026200111.png]]
``` java
public int lengthOfLongestSubstring(String input) {  
    HashSet<Character> set = new HashSet<>();  
    int left = 0;  
    int ans = 0;  
    for (int right = 0; right < input.length(); right++) {  
        while (set.contains(input.charAt(right))) {  
            set.remove(input.charAt(left));  
            left++;  
        }  
        set.add(input.charAt(right));  
        ans = Math.max(ans, right - left + 1);  
    }  
    return ans;  
}
```

## [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

![[Pasted image 20221026214609.png]]

``` java
public String minWindow(String s, String t) {
        HashMap<Character, Integer> map = new HashMap<>();

        for (char x : t.toCharArray()) {
            map.put(x, map.getOrDefault(x, 0) + 1);
        }

        int matched = 0;
        int start = 0;
        int minLen = s.length() + 1;
        int subStr = 0;
        for (int endWindow = 0; endWindow < s.length(); endWindow++) {
            char right = s.charAt(endWindow);
            if (map.containsKey(right)) {
                map.put(right, map.get(right) - 1);
                if (map.get(right) == 0) matched++;
            }

            while (matched == map.size()) {
                if (minLen > endWindow - start + 1) {
                    minLen = endWindow - start + 1;
                    subStr = start;
                }
                char deleted = s.charAt(start++);
                if (map.containsKey(deleted)) {
                    if (map.get(deleted) == 0) matched--;
                    map.put(deleted, map.get(deleted) + 1);
                }
            }
        }
        return minLen > s.length() ? "" : s.substring(subStr, subStr + minLen);
    }
```

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


# Math & Geometry

## [Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)
![[Pasted image 20221027102710.png]]

``` java
public String convert(String s, int numRows) {

	if (numRows == 1) return s;

	StringBuilder ret = new StringBuilder();
	int n = s.length();
	int cycleLen = 2 * numRows - 2;

	for (int i = 0; i < numRows; i++) {
		for (int j = 0; j + i < n; j += cycleLen) {
			ret.append(s.charAt(j + i));
			if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
				ret.append(s.charAt(j + cycleLen - i));
		}
	}
	return ret.toString();
}
```

``` java
public String convert(String s, int numRows) {
    if (numRows == 1) return s;

    List<StringBuilder> rows = new ArrayList<>();
    for (int i = 0; i < Math.min(numRows, s.length()); i++)
        rows.add(new StringBuilder());

    int curRow = 0;
    boolean goingDown = false;

    for (char c : s.toCharArray()) {
        rows.get(curRow).append(c);
        if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
        curRow += goingDown ? 1 : -1;
    }

    StringBuilder ret = new StringBuilder();
    for (StringBuilder row : rows) ret.append(row);
    return ret.toString();
}
```

# Linked List

## [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

Different Sizes Edge Case
![[Pasted image 20221027103441.png]]


Put the 1 in the result even if there aren't any input nodes.
![[Pasted image 20221027103610.png]]

``` java
public ListNode addTwoNumbers(ListNode first, ListNode second) {
        int q = 0;
        int r = 0;
        int sum = 0;
        ListNode head = null;
        ListNode temp = null;
        while (first != null || second != null) {
            sum =
                q +
                (
                    ((first != null) ? first.val : 0) +
                    ((second != null) ? second.val : 0)
                );
            r = sum % 10;
            q = sum / 10;
            ListNode newNode = new ListNode(r);
            if (head == null) {
                head = newNode;
            } else {
                temp = head;
                while (temp.next != null) {
                    temp = temp.next;
                }
                temp.next = newNode;
                newNode.next = null;
            }
            if (first != null) {
                first = first.next;
            }
            if (second != null) {
                second = second.next;
            }
        }
        if (q > 0) {
            ListNode newNode = new ListNode(q);
            temp = head;
            while (temp.next != null) {
                temp = temp.next;
            }
            temp.next = newNode;
            newNode.next = null;
        }
        return head;
    }
```
# Binary Search

## [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

