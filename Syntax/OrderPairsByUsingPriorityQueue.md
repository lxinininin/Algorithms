```java
PriorityQueue<int[]> pq = new PriorityQueue<>(new Comparator<int[]>() {
  public int compare(int[] v1, int[] v2) {
    if (v1[0] != v2[0]) {
      return v1[0] - v2[0];
    } else {
      return v1[1] - v2[1];
    }
  }
});
```

