---
{"dg-publish":true,"permalink":"/study/dsa/data-structures/heap/"}
---


# Declaration

#leetcode/lamda

```java
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
            // Sort by the first element ascending
	if (a[0] != b[0]) {
		return Integer.compare(a[0], b[0]);
	}
	// If first elements are equal, sort by the second element descending
	return Integer.compare(b[1], a[1]);
});
```

