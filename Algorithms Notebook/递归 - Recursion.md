# 递归 - Recursion

# 递归和循环没有明显的边界

例子一：

1. 从前有个山
2. 山里有个庙
3. 庙里有个和尚讲故事
4. 返回1

例子二：

盗梦空间

- 向下进入到不同梦境中；向上又回到原来一层
- 通过声音（参数）传递来同步回到上一层
- 每一层的环境和周围的人都是一份拷贝，主角等几个人穿越不同层级的梦境（发生和携带变化）

# 递归 Recursion

- 简单的递归例子就是算阶乘

    计算$n!$

    $n! = 1 *2*3*...*n$

    ```jsx
    function Factorial(n) {
    	if (n <= 1) return 1;
    	return n * Factorial(n - 1);
    }
    ```

# 思维要点

1. 不要人肉进行递归（最大误区） - 手动去算
2. 找到最近最简方法，将其拆解成可重复解决的问题（重复子问题）
3. 数学归纳法思维 - 只要第一个能运行，并且能让下一层也运行，即后续都可以进行。

# 递归栈

- 展开递归

    ![%E9%80%92%E5%BD%92%20-%20Recursion%20abad96b559034a29a8cdc8312360f5b3/Untitled.png](%E9%80%92%E5%BD%92%20-%20Recursion%20abad96b559034a29a8cdc8312360f5b3/Untitled.png)

# 递归模版

## JS递归模版

```jsx
// JavaScript
const recursion = (level, params) =>{
   // recursion terminator
   if(level > MAX_LEVEL){
     process_result
     return 
   }
   // process current level
   process(level, params)

   //drill down
   recursion(level+1, params)

   //clean current level status if needed
   
}
```

## Python递归模版

![%E9%80%92%E5%BD%92%20-%20Recursion%20abad96b559034a29a8cdc8312360f5b3/Untitled%201.png](%E9%80%92%E5%BD%92%20-%20Recursion%20abad96b559034a29a8cdc8312360f5b3/Untitled%201.png)

```python
# Python
def recursion(level, param1, param2, ...): 
    # recursion terminator 
    if level > MAX_LEVEL: 
	   process_result 
	   return 
    # process logic in current level 
    process(level, data...) 
    # drill down 
    self.recursion(level + 1, p1, ...) 
    # reverse the current level status if needed
```

## Java递归模版

```java
// Java
public void recur(int level, int param) { 
  // terminator 
  if (level > MAX_LEVEL) { 
    // process result 
    return; 
  }
  // process current logic 
  process(level, param); 
  // drill down 
  recur( level: level + 1, newParam); 
  // restore current status 
 
}
```