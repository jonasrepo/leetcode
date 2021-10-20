# 经典题
## 用栈实现队列
```java
class MyQueue {
      private final Stack<Integer> left;
      private final Stack<Integer> right;

    /** Initialize your data structure here. */
    public MyQueue() {
        left = new Stack<>();
        right = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        left.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (! right.empty()) {
            return right.pop();
        }
        while (! left.empty()) {
           right.push(left.pop());
        }
        return right.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if (! right.empty()) {
            return right.peek();
        }
        while (! left.empty()) {
            right.push(left.pop());
        }
        return right.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return right.empty() && left.empty();
    }
}
```
## 用队列实现栈
```java
class MyStack {

    //使用一个队列就可以实现栈
    private Queue<Integer> queue;

    public MyStack() {
        queue = new LinkedList<Integer>();
    }
    
    public void push(int x) {
        //每次push后都要将前面的元素重新调整一遍
        int size = queue.size();
        queue.offer(x);
        while (size >0) {
            queue.offer(queue.poll());
            size --;
        }
    }
    
    public int pop() {
        return queue.poll();
    }
    
    public int top() {
        return queue.peek();
    }
    
    public boolean empty() {
        return queue.isEmpty();
    }
}
```
## 20. 有效的括号
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        HashMap<Character, Character> map = new HashMap<>();
        map.put('(',')');
        map.put('[',']');
        map.put('{','}');
        for(int i=0; i < s.length(); i++) {
            if (map.containsKey(s.charAt(i))) {
                stack.push(s.charAt(i));
            } else {
                if (stack.isEmpty() || map.get(stack.pop()) != s.charAt(i)) {
                    return false;
                }
            }
        }
        return stack.isEmpty();
    }
}
```
## 150. 逆波兰表达式求值
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<String> stack = new Stack<>();
        for (String token : tokens) {
            int tem;
            if (isToken(token)) {
                int value1 = Integer.parseInt(stack.pop());
                int value2 = Integer.parseInt(stack.pop());
                if ("+".equals(token)) {
                    tem =  value1 + value2;
                } else if ("-".equals(token)) {
                    tem = value2-value1;
                } else if ("*".equals(token)) {
                    tem = value1*value2;
                } else {
                    tem = value2/value1;
                }
                stack.push(String.valueOf(tem));
            } else {
                stack.push(token);
            }
        }
        return Integer.parseInt(stack.pop());
    }

    private boolean isToken(String token) {
        return "+".equals(token) || "-".equals(token) || "*".equals(token) || "/".equals(token);
    }
}
```
## 239. [滑动窗口最大值](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.md)
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k){
        MyQueue queue = new MyQueue();
        int[] res = new int[nums.length-k+1];
        //构建原始的单调队列
        for (int i = 0; i < k; i++) {
            queue.add(nums[i]);
        }
        res[0] = queue.peek();
        for (int j = k; j < nums.length; j++) {
            //为了移出最大元素
            queue.poll(nums[j-k]);
            //加入队列
            queue.add(nums[j]);
            //将队列的最大数放入结果中
            res[j-k+1] = queue.peek();
        }

        return res;
    }

  /**
   * 订单队列是一个递增或递减的队列（队列中的元素值是单调的），带有pop，push，peek三个方法
   */
  class MyQueue {
      Deque<Integer> deque = new LinkedList<>();
      //弹出元素时，比较当前要弹出的数值是否等于队列出口的数值，如果相等则弹出
      //移出当前的最大值，不然队列会超过设定的长度
      //同时判断队列当前是否为空
      void poll(int val) {
          if (!deque.isEmpty() && deque.peek() == val) {
              deque.poll();
          }
      }
      //添加元素时，如果要添加的元素大于入口处的元素，就将入口元素弹出
      //保证队列元素单调递减
      //比如此时队列元素3,1，2将要入队，比1大，所以1弹出，此时队列：3,2
      void add(int val) {
          while (!deque.isEmpty() && deque.getLast() < val) {
              deque.pollLast();
          }
          deque.addLast(val);
      }
      //队列队顶元素始终为最大值
      int peek() {
          return deque.peek();
      }
  }
}
```
## 347.前 K 个高频元素
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        //构造map
        for (int num : nums) {
            map.put(num,map.getOrDefault(num,0)+1);
        }
        //求最大值，使用小顶堆
        PriorityQueue<int[]> priorityQueue = new PriorityQueue<>((int[] a1, int[] a2) -> {
            return a1[1] - a2[1];
        });
        //构造小顶堆
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            priorityQueue.add(new int[]{entry.getKey(), entry.getValue()});
            if (priorityQueue.size() > k) {
                priorityQueue.poll();
            }
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = priorityQueue.poll()[0];
        }
        return res;
    }
}
```
### 71.简化路径
```java
class Solution {
    public String simplifyPath(String path) {
        String[] split = path.split("/");
        Stack<String> stack = new Stack<>();
        for (String s : split) {
            //过滤 /ab//, /a/./ 这种
            if(s.equals("") || s.equals(".")) continue;
            if (s.equals("..")) {
                if (!stack.isEmpty()) {
                    stack.pop();
                }
            } else {
                stack.push(s);
            }
        }
        if (stack.isEmpty()) {
            return "/";
        }
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.insert(0,stack.pop());
            sb.insert(0,"/");
        }
        return sb.toString();
    }
}
```
## 1047.删除字符串中的所有相邻元素
```java
class Solution {
    public String removeDuplicates(String s) {
        //使用sb实现类似于栈的概念，用于消除重复元素
        StringBuilder sb = new StringBuilder();
        int top = -1;
        //abbaca
        for (int i = 0; i < s.length(); i++) {
            if (top>=0 && sb.charAt(top) == s.charAt(i)) {
                sb.deleteCharAt(top);
                top--;
            } else {
                sb.append(s.charAt(i));
                top++;
            }
        }

        return sb.toString();
    }
}
```