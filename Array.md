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

## 双指针
> 用一个快指针和一个慢指针在一个循环下完成两个循环的工作

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