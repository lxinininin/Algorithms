```java
// both of them can use push() and pop()
Deque<Character> stack1 = new LinkedList<>();
Deque<Integer> stack1.5 = new ArrayDeque<>(); // this is fastest
Stack<Character> stack2 = new Stack<>(); // it is better to not use this, to slow!!!

// but the first one can use other methods to improve flexibility
stack1.pollFirst(); // same as pop()
stack1.pollLast();
stack1.offerFirst(); // same as push()
stack1.offerLast();

// if we would like to iterate over a Stack
// Method 1: Using Iterator
System.out.println("Using Iterator:");
Iterator<Character> iterator = stack1.iterator();
while (iterator.hasNext()) {
  System.out.println(iterator.next());
}

// Method 2: Using while loop and pop
System.out.println("Using while loop and pop:");
while (!stack1.isEmpty()) {
  System.out.println(stack1.pop());
}
```

