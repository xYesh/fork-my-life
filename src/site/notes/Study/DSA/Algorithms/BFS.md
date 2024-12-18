---
{"dg-publish":true,"permalink":"/study/dsa/algorithms/bfs/","dgPassFrontmatter":true}
---


# Just have a validation function & Directions array

```java
int[][] directions = new int[][]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

public boolean valid(int row, int col) {
	return 0 <= row && row < m && 0 <= col && col < n;
}
```

# Finding gates or closest value type questions
- It makes sense to look at the question ==backwards==.
	- i.e., look from the gate and calculate the closest steps. don't look from every path for the closest gate. 