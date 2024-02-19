If the class implements the `Comparable` interface, the natural ordering is used. If not, you can provide a custom `Comparator` when constructing the `TreeMap` to define the ordering of the keys.

```java
import java.util.TreeMap;

class MyClass implements Comparable<MyClass> {
  private int value;

  public MyClass(int value) {
    this.value = value;
  }

  public int getValue() {
    return value;
  }

  @Override
  public int compareTo(MyClass other) {
    // Implement comparison logic based on your requirements
    return Integer.compare(this.value, other.value);
  }

  public static void main(String[] args) {
    TreeMap<MyClass, String> treeMap = new TreeMap<>();

    MyClass key1 = new MyClass(3);
    MyClass key2 = new MyClass(1);
    MyClass key3 = new MyClass(5);

    treeMap.put(key1, "Value1");
    treeMap.put(key2, "Value2");
    treeMap.put(key3, "Value3");

    // TreeMap will be sorted based on the natural ordering (or comparator)
    for (MyClass key : treeMap.keySet()) {
      System.out.println("Key: " + key.getValue() + ", Value: " + treeMap.get(key));
    }
  }
}

```

In this example, `MyClass` implements the `Comparable` interface, providing a natural ordering based on the `value` field. Alternatively, you can pass a custom `Comparator` to the `TreeMap` constructor if you want to define a different ordering logic.