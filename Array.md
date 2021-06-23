# 常见的解题算法
- 二分法
- 双指针
- 滑动窗口
- 模拟行为

## [二分法](https://mp.weixin.qq.com/s/4X-8VRgnYRGd5LYGZ33m4w)

注意细节
1. 溢出 left + ((right - left) >> 2)
2. 注意区间的开闭，如果是左闭又开，则是left < right,如果是左闭右闭，则是left <=right
3. 注意是否有重复元素

### 相关题目

#### 35.搜索插入位置
> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
```java
//左闭右闭
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while (left <= right) {
            int mid = left + (right-left >>1);
            if (nums[mid] > target) {
                right = mid-1;
            } else if (nums[mid] < target) {
                left = mid+1;
            }else {
                return mid;
            }
        }
        return Math.max(0,left);
    }
}

//左闭右开
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + (right-left >>1);
            if (nums[mid] > target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid+1;
            }else {
                return mid;
            }
        }
        return Math.max(0,left);
    }
}
```

#### 34.在排序数组中查找元素的第一个和最后一个位置
> 给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left <= right) {
            int mid = left + (right-left>>1);
            if (nums[mid] > target) {
                right = mid-1;
            } else if (nums[mid] < target) {
                left = mid+1;
            } else {
                //需要注意数组重复出现的情况
                int leftPoint = mid , rightPoint = mid;
                while(leftPoint-1>=0) {
                    if(nums[leftPoint-1] == target) {
                        leftPoint--;
                    } else {
                        break;
                    }
                }
                while (rightPoint+1<=right) {
                    if (nums[rightPoint+1] == target) {
                        rightPoint++;
                    } else {
                        break;
                    }
                }
                return new int[]{leftPoint,rightPoint};
            }
        }
        return new int[]{-1,-1};
    }
}
```

#### 69.x 的平方根
```java
class Solution {
    public int mySqrt(int x) {
        if (x ==1 || x==2 || x==3) {
            return 1;
        }
        int left = 1;
        int right = x;
        while(left <= right) {
            int mid = left + (right-left>>1);
            if ((long)mid * mid > x) {
                right = mid-1;
            } else if (mid * mid < x){
                left = mid+1;
            } else {
                return mid;
            }
        }
        return right;

    }
}
```

#### 367.有效的完全平方数
> 给定一个 正整数 num ，编写一个函数，如果 num 是一个完全平方数，则返回 true ，否则返回 false
```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if (num ==1 ) return true;
        if (num == 2 || num == 3) return false;
        int left =1;
        int right = num;
        while (left <= right) {
            int mid = left + (right-left>>1);
            if ((long)mid * mid > num) {
                right = mid-1;
            }else if (mid * mid < num) {
                left = mid +1;
            } else {
                return true;
            }
        }
        return false;
    }
}
```

## 双指针(快慢指针，左右端点指针，固定间距指针)
> 用一个快指针和一个慢指针在一个循环下完成两个循环的工作
### 模板
#### 快慢指针
```
l = 0
r = 0
while 没有遍历完
  if 一定条件
    l += 1
  r += 1
return 合适的值
```
#### 左右端点指针
```
l = 0
r = n - 1
while l < r
  if 找到了
    return 找到的值
  if 一定条件1
    l += 1
  else if  一定条件2
    r -= 1
return 没找到
```
#### 固定间距指针
```
l = 0
r = k
while 没有遍历完
  自定义逻辑
  l += 1
  r += 1
return 合适的值
```
### 相关题目
#### 27. 移除元素
> 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。  
> //输入：nums = [3,2,2,3], val = 3  
> //输出：2, nums = [2,2]  
![演示](https://mmbiz.qpic.cn/mmbiz_gif/ciaqDnJprwv43W2OFzic9tNsB9dGwCaYbQjds2cInASicrZXXVXaW7f08DAelOzRYv8EJez8N3jNzJ5HQhvfQWLQQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)
```java
class Solution {
      //输入：nums = [0,1,2,2,3,0,4,2], val = 2
    public int removeElement(int[] nums, int val) {
        //慢指针指的是能赋值的位置
        int slow = 0;
        //快指针指的是需要处理的位置
        int fast = 0;
        while(fast < nums.length) {
            //如果不相等，则将快指针的值赋值到慢指针，快慢指针都要移动
            //否则慢指针不动，只需要快指针移动，因为当前快指针位置的值不需要保存
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        return slow;
    }
}

```
#### 26.删除排序数组中的重复项
> 给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。  
> 输入：nums = [0,0,1,1,1,2,2,3,3,4]  
> 输出：5, nums = [0,1,2,3,4]
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int fast = 1, slow = 1;
        while (fast < n) {
            if (nums[fast] != nums[fast - 1]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
}
```
#### 283.移动零
> 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。  
> 输入: [0,1,0,3,12]  
> 输出: [1,3,12,0,0] 
```java
class Solution {
    public void moveZeroes(int[] nums) {
        //[0,1,0,3,12]
        int slow = 0;
        int fast = 0;
        while(fast < nums.length) {
            if (nums[fast] != 0) {
                int temp = nums[slow];
                nums[slow] = nums[fast];
                nums[fast] = temp;
                slow++;
            }
            fast++;
        }
    }
}
```
#### 977.有序数组的平方
> 给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序  
> 输入：nums = [-4,-1,0,3,4,5,6,7,10]  
> 输出：[0,1,9,16,100]  
> 
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        //找到边界条件后，从边界的位置开始比较
        //[-4,-1,0,3,4,5,6,7,10]
        int lessThenZeroCount = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < 0) {
                lessThenZeroCount++;
            } else {
                break;
            }
        }
        int[] result = new int[nums.length];
        int i = lessThenZeroCount-1, j = lessThenZeroCount;
        int index =0;
        while (i>=0 || j < nums.length) {
            if (i<0) {
                result[index++] = nums[j] * nums[j];
                j++;
                continue;
            }
            if (j==nums.length) {
                result[index++] = nums[i] * nums[i];
                i--;
                continue;
            }
            if (-nums[i] <= nums[j]) {
                result[index++] = nums[i] * nums[i];
                i--;
            } else {
                result[index++] = nums[j] * nums[j];
                j++;
            }
        }
        return result;

    }
}

```

## 滑动窗口
> 滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置。从而将O(n^2)的暴力解法降为O(n)  

### 相关题目
#### 长度最小的子数组
> 给定一个含有 n 个正整数的数组和一个正整数 target  
> target = 7, nums = [2,3,1,2,4,3]  
> 输出：2
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        //target = 7, nums = [2,3,1,2,4,3]
        if (nums.length == 0) {
            return 0;
        }
        int left = 0 ,right = 0, sum =0, minLen = Integer.MAX_VALUE;
        while (right < nums.length) {
            sum += nums[right];
            while (sum >= target) {
                minLen = Math.min((right - left + 1), minLen);
                sum -= nums[left++];
            }
            right++;
        }

        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```

#### 904 水果成篮
> 有两个篮，每个篮里只能装一个水果，最多能装多少个水果
```java
class Solution {
    public int totalFruit(int[] tree) {
        //[1,2,1,1,3,4,5,5]
        //2个篮子表示最多只能选两种水果，用一个hashmap保存水果与对应的个数
        HashMap<Integer, Integer> container = new HashMap<>();
        int left = 0, maxLen = 0;
        for(int right=0; right< tree.length; right++) {
            //保存该水果的个数
            container.put(tree[right], container.getOrDefault(tree[right],0)+1);
            //如果水果的个数大于2，则左移做边界
            while (container.size() > 2) {
                container.put(tree[left], container.get(tree[left])-1);
                if (container.get(tree[left]) == 0) {
                    container.remove(tree[left]);
                }
                left++;
            }
            maxLen = Math.max(right - left +1, maxLen);
        }
        return maxLen;
    }
}
```
#### 76.最小覆盖子串
> 给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。   
> 输入：s = "ADOBECODEBANC", t = "ABC"  
> 输出："BANC"  
```java
class Solution {
    public String minWindow(String s, String t) {
        //s = "ADOBECODEBANC", t = "ABC"
        HashMap<Character,Integer> need = new HashMap<>();
        HashMap<Character,Integer> window = new HashMap<>();
        for(char ch : t.toCharArray()){
            need.put(ch,need.getOrDefault(ch,0) + 1);
        }
        //左右指针
        int left = 0,  right = 0;
        //有效的位数
        int valid = 0;
        //最优的开始位置和长度
        int start = 0, len = Integer.MAX_VALUE;
        while(right < s.length()){
            char c = s.charAt(right);
            right++;
            if(need.containsKey(c)){
                window.put(c,window.getOrDefault(c,0) + 1);
                ////这个得改成equals
                if(window.get(c).equals(need.get(c))){
                    valid++;
                }
            }
            //收缩左边界
            while(valid == need.size()){
                if(right - left < len){
                    start = left;
                    len = right - left;
                }
                char d = s.charAt(left);
                left++;
                if(need.containsKey(d)){
                    //这个得改成equals
                    if(window.get(d).equals(need.get(d))){
                        valid--;
                    }
                    window.put(d,window.getOrDefault(d,0) - 1);
                }
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start,start + len);
    }
}
```
## 模拟行为
> 一般都是涉及到数组的循环打印或者赋值, 需要考虑边界问题, 通常使用左开右闭的方式去遍历
### 相关题目
#### [螺旋矩阵II](https://mp.weixin.qq.com/s/Hn6-mlCPvKAdWbiFfQyaaw)
![](https://mmbiz.qpic.cn/mmbiz_png/ciaqDnJprwv5TSShYYt6QDDePAibrpibjJewwibBBCZIibtHm4JTgYGic2qSoTUsky9Gj09PJnVBMZyzlPBpZiaj9ppUQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
> 给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix   
> 输入：n = 3  
> 输出：[[1,2,3],[8,9,4],[7,6,5]]
```java
class Solution {
    public int[][] generateMatrix(int n) {
        //使用左闭右开的区间作为分解判断依据
        //每次循环都会生成一个圈,所以需要循环的次数是 n / 2
        //如果n是奇数,最后需要填充中心的点,坐标为[n/2,n/2],
        int startX=0, startY=0;
        int bound = 1;//初始边界是1,每次循环完后,边界++
        int loop = n /2;
        int[][] res = new int[n][n];
        int number = 1;
        while (loop > 0) {
            int i = startX;
            int j = startY;
            //从左到右
            for (; j < n - bound; j++) {
                res[i][j] = number++;
            }
            //从右到下
            for (; i < n - bound; i++) {
                res[i][j] = number++;
            }
            //从右到左
            for (; j > startY; j--) {
                res[i][j] = number++;
            }
            //从左到上
            for (;i > startX; i--) {
                res[i][j] = number++;
            }
            loop--;
            startX++;
            startY++;
            bound++;
        }
        if ((n & 1) == 1) {
            int mid = n / 2;
            res[mid][mid] = number;
        }
        return res;
    }
}

//
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int t = 0, b = n - 1, l = 0, r = n - 1; //t为上边界；b为下边界；l为左边界；r为右边界
        int i = 1;  //记录每个位置的值
        while(i <= n * n) { //每个位置的值不可能超过n的平方
            for(int j = l; j <= r; j++) //从左遍历到右边
                ans[t][j] = i++;
            t++;
            for(int j = t; j <= b; j++) //从上到下
                ans[j][r] = i++;
            r--;
            for(int j = r; j >= l; j--) //从右到左
                ans[b][j] = i++;
            b--;
            for(int j = b; j >= t; j--) //从下到上
                ans[j][l] = i++;
            l++;
        }
        return ans;
    }
}
```
