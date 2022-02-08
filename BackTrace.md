# 回溯

就是穷举, 加上剪枝, 主要解决下面几种问题

* 组合问题：N个数里面按一定规则找出k个数的集合
* 切割问题：一个字符串按一定规则有几种切割方式
* 子集问题：一个N个数的集合里有多少符合条件的子集
* 排列问题：N个数按一定规则全排列，有几种排列方式(组合顺序无关, 排列顺序有关)
* 棋盘问题：N皇后，解数独等等

> 在求和问题中，排序之后加剪枝是常见的套路！

## 解题的模板

```java
void backtracking(参数){
        if(终止条件){
        存放结果;
        return;
        }

        for(选择：本层集合中元素（树中节点孩子的数量就是集合的大小）){
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
        }
        }
```

## 常见的题

### 77. 组合

> 给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

```java
class Solution {
    private final List<List<Integer>> res = new ArrayList<List<Integer>>();
    private final LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> combine(int n, int k) {
        trace(n, k, 1);
        return res;
    }

    private void trace(int n, int k, int startIndex) {
        //终止条件
        if (path.size() == k) {
            res.add(new LinkedList<>(path));
            return;
        }
        //[1,2,3,4] 从左往右
        //n - (k-path.size()-1) 剪枝
        for (int i = startIndex; i <= n; i++) {
            path.add(i);
            //右上往下
            trace(n, k, i + 1);
            path.removeLast();
        }
    }
}
```

### 216.组合总和III

> 找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    LinkedList<Integer> tempList = new LinkedList<>();

    public List<List<Integer>> combinationSum3(int k, int n) {
        trace(k, n, 0, 1);
        return res;
    }

    void trace(int k, int n, int sum, int start) {
        //剪枝1
        if (sum > n) {
            return;
        }
        if (k == tempList.size()) {
            if (sum == n) {
                res.add(new LinkedList<>(tempList));
            }
            return;
        }
        //已加入 tempList.size();
        //剩余 k-(tempList.size())
        //剪枝2
        for (int i = start; i <= 9 - (k - tempList.size() - 1); i++) {
            tempList.add(i);
            trace(k, n, sum + i, i + 1);
            tempList.removeLast();
        }
    }
}
```

### 17.电话号码的字母组合

> 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

```java
class Solution {
    //设置全局列表存储最后的结果
    List<String> list = new ArrayList<>();
    String[] numStringMap = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return list;
        }
        //初始对应所有的数字，为了直接对应2-9，新增了两个无效的字符串""

        //迭代处理
        backTracking(digits, 0);
        return list;

    }

    //每次迭代获取一个字符串，所以会设计大量的字符串拼接，所以这里选择更为高效的 StringBuild
    StringBuilder temp = new StringBuilder();

    //比如digits如果为"23",pos 为0，则str表示2对应的 abc
    public void backTracking(String digits, int pos) {
        //遍历全部一次记录一次得到的字符串
        if (pos == digits.length()) {
            list.add(temp.toString());
            return;
        }
        //str 表示当前num对应的字符串
        String str = numStringMap[digits.charAt(pos) - '0'];
        for (int i = 0; i < str.length(); i++) {
            temp.append(str.charAt(i));
            //回溯
            backTracking(digits, pos + 1);
            //剔除末尾的继续尝试
            temp.deleteCharAt(temp.length() - 1);
        }
    }
}
```

### [39. 组合总和](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.md)

> 给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的

```java
class Solution {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        //排序后可剪枝
        Arrays.sort(candidates);
        traceback(candidates, target, 0);
        return res;
    }

    void traceback(int[] candidates, int left, int pos) {
        if (left < 0) return;
        //结束条件
        if (left == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        //剪枝
        for (int i = pos; i < candidates.length && (left - candidates[i] >= 0); i++) {
            int num = candidates[i];
            temp.add(num);
            traceback(candidates, left - num, i);
            temp.remove(temp.size() - 1);
        }
    }
}
```

> 40.组合总和II

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        boolean[] used = new boolean[candidates.length];
        trace(candidates, target, 0, used);
        return res;
    }

    void trace(int[] candidates, int left, int pos, boolean[] used) {
        //终止条件
        if (left < 0) {
            return;
        }
        if (left == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        //剪枝
        for (int i = pos; i < candidates.length && left >= candidates[pos]; i++) {

            // used[i - 1] == true，说明同一树支candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过, 这里的使用是因为递归后设置了false,
            // 这里可能是加入了, 也可能没加入, 当时说明上一条情况的已经遍历过了
            if (i > 0 && candidates[i] == candidates[i - 1] && !used[i - 1]) {
                continue;
            }
            temp.add(candidates[i]);
            used[i] = true;
            trace(candidates, left - candidates[i], i + 1, used);
            temp.remove(temp.size() - 1);
            used[i] = false;
        }
    }
}
```

### [131.分割回文串](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.md)

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> temp = new ArrayList<>();

    public List<List<String>> partition(String s) {
        traceback(s, 0);

        return res;
    }

    void traceback(String s, int pos) {
        if (pos >= s.length()) {
            res.add(new ArrayList<>(temp));
            return;
        }

        //横向遍历, 增加长度: 0 , 1,2,3
        for (int len = pos; len < s.length(); len++) {
            if (isPalindrome(s, pos, len)) {
                temp.add(s.substring(pos, len + 1));
                traceback(s, len + 1);
                temp.remove(temp.size() - 1);
            }
        }
    }

    boolean isPalindrome(String s, int start, int end) {
        while (start <= end) {
            if (s.charAt(start++) != s.charAt(end--)) {
                return false;
            }
        }
        return true;
    }
}
```

### [93.复原IP地址](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.md)

```java
class Solution {
    List<String> res = new ArrayList<>();
    List<String> temp = new ArrayList<>();

    public List<String> restoreIpAddresses(String s) {
        traceback(s, 0);
        return res;
    }

    void traceback(String s, int startPos) {
        if (temp.size() > 4) {
            return;
        }
        if (temp.size() == 4 && startPos > s.length() - 1) {
            res.add(String.join(".", temp));
            return;
        }
        for (int len = 0; len < s.length() - startPos; len++) {
            String cur = s.substring(startPos, startPos + len + 1);
            if (isValidIpStr(cur)) {
                temp.add(cur);
                traceback(s, startPos + len + 1);
                temp.remove(temp.size() - 1);
            }
        }
    }


    boolean isValidIpStr(String i) {
        if (i.length() >= 4) return false;
        if (i.charAt(0) == '0' && i.length() >= 2) return false;
        long j = Integer.parseInt(i);
        return j >= 0 && j <= 255;
    }
}
```

### [第78题. 子集](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0078.%E5%AD%90%E9%9B%86.md)

> 那么组合问题和分割问题都是收集树的叶子节点，而子集问题是找树的所有节点！

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        Arrays.sort(nums);
        traceback(nums, 0);
        //剩余空集合
        res.add(Collections.emptyList());

        return res;
    }

    void traceback(int[] nums, int startPos) {
        if (startPos >= nums.length) {
            return;
        }
        for (int i = startPos; i < nums.length; i++) {
            temp.add(nums[i]);
            res.add(new ArrayList<>(temp));
            traceback(nums, i + 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```

### 第90题.子集II

> 和 40.组合总和II 的思路类试

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> temp = new LinkedList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        boolean[] used = new boolean[nums.length];
        Arrays.sort(nums);
        traceback(nums, 0, used);
        res.add(Collections.emptyList());
        return res;
    }

    void traceback(int[] nums, int startPos, boolean[] used) {
        if (startPos >= nums.length) {
            return;
        }
        for (int i = startPos; i < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;
            temp.add(nums[i]);
            res.add(new ArrayList<>(temp));
            used[i] = true;
            traceback(nums, i + 1, used);
            temp.removeLast();
            used[i] = false;
        }
    }

}
```

### [491.递增子序列](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.md)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> temp = new LinkedList<>();

    public List<List<Integer>> findSubsequences(int[] nums) {
        traceback(nums, 0);
        return res;
    }

    void traceback(int[] nums, int pos) {
        if (temp.size() > nums.length) {
            return;
        }
        //[-100, 100]之间201个元素
        boolean[] used = new boolean[201];
        for (int i = pos; i < nums.length; i++) {
            if ((!temp.isEmpty() && nums[i] < temp.getLast()) || used[nums[i] + 100]) continue;
            temp.add(nums[i]);
            if (temp.size() > 1) {
                res.add(new ArrayList<>(temp));
            }
            used[nums[i] + 100] = true;
            traceback(nums, i + 1);
            temp.removeLast();
        }
    }
}
```

### 46.全排列.md

```java
class Solution {
    private List<List<Integer>> res = new ArrayList<>();
    private LinkedList<Integer> temp = new LinkedList<>();

    public List<List<Integer>> permute(int[] nums) {
        boolean[] used = new boolean[nums.length];
        traceback(nums, used);
        return res;
    }

    private void traceback(int[] nums, boolean[] used) {
        if (temp.size() == nums.length) {
            res.add(new ArrayList<>(temp));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            temp.add(nums[i]);
            traceback(nums, used);
            temp.removeLast();
            used[i] = false;
        }
    }
}
```

### 47.全排列 II

```java
class Solution {
    private List<List<Integer>> res = new ArrayList<>();
    public LinkedList<Integer> temp = new LinkedList<>();

    public List<List<Integer>> permuteUnique(int[] nums) {
        //去重判断需要排序
        Arrays.sort(nums);
        //判断同一层是否取了
        boolean[] used = new boolean[nums.length];
        traceback(nums, used);

        return res;
    }

    private void traceback(int[] nums, boolean[] used) {
        if (temp.size() == nums.length) {
            res.add(new LinkedList<>(temp));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i - 1] == nums[i] && used[i - 1]) continue;
            if (used[i]) continue;
            used[i] = true;
            temp.add(nums[i]);
            traceback(nums, used);
            used[i] = false;
            temp.removeLast();
        }
    }
}
```

### n皇后

```java
class Solution {
    private List<List<String>> res = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        //某个位置放置后, 横, 竖, 斜线上都不能放
        char[][] chessBoard = new char[n][n];
        for (char[] chess : chessBoard) {
            Arrays.fill(chess, '.');
        }
        traceback(n, chessBoard, 0);

        return res;
    }

    private void traceback(int n, char[][] chessBoard, int row) {
        if (n == row) {
            res.add(list2Array(chessBoard));
            return;
        }
        for (int col = 0; col < n; col++) {
            if (isValid(n, chessBoard, row, col)) {
                chessBoard[row][col] = 'Q';
                traceback(n, chessBoard, row + 1);
                chessBoard[row][col] = '.';
            }
        }
    }

    private boolean isValid(int n, char[][] chessBoard, int row, int col) {
        //竖
        for (int i = 0; i < row; i++) {
            if (chessBoard[i][col] == 'Q') {
                return false;
            }
        }
        //斜线 45度
        for (int j = row - 1, k = col + 1; j >= 0 && k < n; j--, k++) {
            if (chessBoard[j][k] == 'Q') {
                return false;
            }
        }
        //斜线 135度
        for (int l = row - 1, m = col - 1; l >= 0 && m >= 0; l--, m--) {
            if (chessBoard[l][m] == 'Q') {
                return false;
            }
        }
        return true;
    }

    private List<String> list2Array(char[][] chessBoard) {
        List<String> res = new ArrayList<>();
        for (char[] chess : chessBoard) {
            res.add(String.copyValueOf(chess));
        }

        return res;
    }
}
```

### 37. 解数独

```java
class Solution {
    public void solveSudoku(char[][] board) {
        traceback(board);
    }

    private boolean traceback(char[][] board) {
        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {
                if (board[row][col] != '.') continue;
                for (char i = '1'; i <= '9'; i++) {
                    if (isValid(board, i, row, col)) {
                        board[row][col] = i;
                        if (traceback(board)) {
                            return true;
                        }
                        board[row][col] = '.';
                    }
                }
                //试完了9个数
                return false;
            }
        }

        return true;
    }

    private boolean isValid(char[][] board, char num, int row, int col) {
        //row
        for (int i = 0; i < 9; i++) {
            if (board[i][col] == num) {
                return false;
            }
        }
        //col
        for (int j = 0; j < 9; j++) {
            if (board[row][j] == num) {
                return false;
            }
        }
        //3*3
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++) {
            for (int j = startCol; j < startCol + 3; j++) {
                if (board[i][j] == num) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

