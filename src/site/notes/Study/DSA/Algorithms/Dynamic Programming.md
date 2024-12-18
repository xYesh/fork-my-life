---
{"dg-publish":true,"permalink":"/study/dsa/algorithms/dynamic-programming/"}
---


# Kadane's Algorithm 

[Kadane's Algorithm to Maximum Sum Subarray Problem - YouTube](https://www.youtube.com/watch?v=86CQq3pKSUw)

- At each index we try to determine the largest sum till that index.
- The largest sub array ending at the n<sup>th</sup> index is  <font color="#4f6128">either the current element alone or prev sum + curr element.</font>
- There is<font color="#953734"> no two pointer </font>here