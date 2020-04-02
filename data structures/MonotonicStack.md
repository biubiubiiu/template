顾名思义，单调栈即满足单调性的栈结构。与单调队列相比，其只在一端进行进出。

将一个元素插入单调栈时，为了维护栈的单调性，需要在保证将该元素插入到栈顶后整个栈满足单调性的前提下弹出最少的元素。

```java
class MonotonicStack {
    
    Stack<Integer> stack = null;
    
    public MonotonicStack() {
		stack = new Stack<>();
    }
    
    /**
    * @return 弹出元素的个数
    */
    public int push(int x) {
        int ret = 0;
		while(!stack.isEmpty() && x >= stack.peek()) {
            stack.pop();
            ret++;
        }
		stack.push(x);
        return ret;
    }
    
   //... same with Stack
}
```

