Emmanuel's Notes: [Kirb 300]([https://docs.google.com/spreadsheets/d/15bOx-eVFRA5BgruH-DGESjcQZdG2sehwfLvErBIxZJU/edit?usp=sharing](https://docs.google.com/spreadsheets/d/15bOx-eVFRA5BgruH-DGESjcQZdG2sehwfLvErBIxZJU/edit?usp=sharing)
Neatcode Notes: [Blind 75](https://docs.google.com/spreadsheets/u/0/d/1A2PaQKcdwO_lwxz9bAnxXnIQayCouZP6d-ENrBz_NXc/htmlview)
Top Amazon Questions: [Leetcode](https://leetcode.com/problem-list/top-amazon-questions/)
Types of algorithms: [Algorithms](https://www.geeksforgeeks.org/fundamentals-of-algorithms/)


# Time Complexity

[Understanding Time Complexity with Simple Examples](https://www.geeksforgeeks.org/understanding-time-complexity-simple-examples/)

Time complexity is the number of times a statement executes.

## **constant: O(1)**
``` C++
int main()
{
    cout << "Hello World";
    return 0;
}
```

## **linear: O(n)**
``` C++
int main()
{
    int i, n = 8;
    for (i = 1; i <= n; i++) {
        cout << "Hello World !!!\n";
    }
    return 0;
}
```

## logarithmic: O(log2(n))
``` C++
int main()
{
    int i, n = 8;
    for (i = 1; i <= n; i=i*2) {
        cout << "Hello World !!!\n";
    }
    return 0;
}
```

### Diagrams
Usually applied to Binary Search ([Neatcode](https://youtu.be/BgLTDT03QtU?t=648)). Any algorithm where you can eliminate half of the elements from consideration each time the statement executes.
![[Pasted image 20221026132917.png]]

This is common in Binary Tree solutions.
![[Pasted image 20221026133150.png]]



## logarithmic: O(log(log n))
``` C++
int main()
{
    int i, n = 8;
    for (i = 2; i <= n; i=pow(i,2)) {
        cout << "Hello World !!!\n";
    }
    return 0;
}
```


# Kirb 300

### **Most Common**
DFS - pre/post/inorder traversal (V + E)
BFS - shortest path, unweighted graph (V + E)
[Topological Sort](https://www.geeksforgeeks.org/topological-sorting/) - find ordering of nodes, verifies if graph is DAG (V + E)

### **Less Common**
Union Find - find/count disjoint sets, adding path compression is one line but improves runtime massively
Dijkstra - shortest path, works for weighted, no negative weights allowed (E log V)
Longest Path - brute force exhaustive will work everytime; but if acyclic, there is also a solution involving topo sort/DP
Cycle Detection - for directed graph, use topo sort; for undirected, can use DFS or union find

### **Rare**
Bellman Ford - shortest path, weighted and negative weights allowed (E * V)
Minimum Spanning Tree - use Prim's or Kruskal's algorithms

## BEFORE STARTING LEETCODE  
Learn Data Structures and Algorithms - preferably a class, but online courses are fine too  
Do the [Grokking Cours](https://www.educative.io/courses/grokking-the-coding-interview)e  
Do the [Grokking DP Cour](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews)se  
Skip Challenge Problems in both; don't exhaustively complete a section, just do enough to understand the pattern.  
  
## SUGGESTED PRACTICE/STUDY PLAN (blue -> yellow -> green -> orange)  
(1st pass) From top to bottom, do Blind 75
(2nd pass) do LC Top Interview questions  
(3rd pass) do LC Patterns questions  
(4th pass) do NeetCode questions  
(5th pass) do all others such that you complete all problems on spreadsheet  
(5th+ pass) Revisit all problems top to bottom (ad infinitum)

## SUPPLEMENTAL TOPICS FOR TECH INTERVIEWS  
Object Oriented Design (OOD)  
System Design  
Concurrency and other OS concepts  
Database design  
Computer networks  
  
## DISCLAIMERS  
- THE COLOR CODING MAPS TO WHERE THE PROBLEM IS SOURCED FROM  
- this is a "one stop shop" to study long-term and target certain topics easily  
- Blind 75 is often enough for many people to pass interviews (assuming you really dominate it)  
- the additional sources beyond Blind 75 are popular, tried, and trusted; this obviously means more breadth, which can only help  
- Mediums are most common in interviews  
- doing company-tagged problems has varying degrees of usefulness  
- OOD problems are a silent killer, do not negpulect them

## POPULAR COLLECTIONS / SOURCE:

- [ ] [Blind 75](https://leetcode.com/discuss/general-discussion/460599/blind-75-leetcode-questions)
- [ ] [LC Top Intervew Questions](https://leetcode.com/explore/featured/card/top-interview-questions-easy/)
- [ ] [LC Patterns ("sean prashad"](https://seanprashad.com/leetcode-patterns/))
- [ ] [NeetCode YouTube](https://www.youtube.com/c/NeetCode/playlists)
- [ ] [Neetcode.io](https://neetcode.io/)
- [ ] ["Blind 50"](https://www.techinterviewhandbook.org/best-practice-questions/)
- [ ] [Grind 75](https://www.grind75.com/)
- [ ] [LC Company-Tagged  
- [ ] (Click Tags > Companies), requires Premium](https://leetcode.com/problemset/all/)


