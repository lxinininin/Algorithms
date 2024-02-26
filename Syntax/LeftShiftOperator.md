In Java, the "<<" operator is the left shift operator. It performs a bitwise left shift operation on the left operand by the number of positions specified by the right operand.

For example, if you have an integer variable `x` and you want to shift its bits to the left by 2 positions, you would use the `<<` operator like this:

```java
int x = 5; // Binary representation: 0000 0101
int result = x << 2; // Shift left by 2 positions
// Result: 0001 0100, which is 20 in decimal
```

In this example, `x` has a binary representation of `0000 0101` (which is 5 in decimal). After applying the left shift operator `<<` by 2 positions, the resulting binary representation is `0001 0100`, which is equivalent to 20 in decimal.