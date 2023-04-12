最小/最大栈支持栈的 `push`, `pop`, `top` 操作外，能在 $O(1)$ 内获取到最小/最大元素；其内部用了两个栈来存储，其中一个栈专门用于记录“历史最小/最大值”，该栈是严格递增/递减的。

以最小栈为例：

```java
class MinStack {

    private Stack<Integer> s1 = null;
    private Stack<Integer> s2 = null;

    public MinStack() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }

    public void push(int x) {
        s1.push(x);
        if (s2.isEmpty() || s2.peek() >= x)
            s2.push(x);
    }

    public void pop() {
        int x= s1.pop();
        if (s2.peek() == x)
            s2.pop();
    }

    public int top() {
        return s1.peek();
    }

    public int getMin() {
        return s2.peek();
    }
}
```
