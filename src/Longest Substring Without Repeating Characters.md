给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

```
示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
```
请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

```

    //三个for循环版本
    public int solve(int[] arrs){
        int max = 0;//用来存放目标子序列的和
        int temp = 0;//用来存每个子序列的和
        for(int i = 0; i < arrs.length; i++){
            for(int j = i; j < arrs.length; j++){
                temp = 0;
                //计算子序列的和
                for(int k = 0; k < arrs.length; k++){
                    temp += arrs[k];
                }
                //进行比较
                if(temp > max){
                    max = temp;
                }
            }
        }
        return max;
    }

    //两个for循环版本
    public int solve2(int[] arrs){
        int max = 0;
        int temp = 0;
        for(int i = 0; i < arrs.length; i++){
            temp = 0;
            for(int j = i; j < arrs.length; j++){
                //一边处理数据
                temp += arrs[j];
                //进行比较选择
                if(max < temp){
                    max = temp;
                }
            }
        }
        return temp;
    }

    //递归版本
    public int solve3(int[] arrs, int left, int right){
        int max = 0;
        //表示只有一个元素，无需在分解
        if(left == right){
            //为什么？因为低于0的数肯定不可以是最大值的
            //大不了最大值为0
            max = arrs[left] >= 0 ? arrs[left]:0;
        }else{
            int center = (left + right)/2;
            //求解左半部分最大子序列
            int leftMax = solve3(arrs, left, center);
            //求解右半部分最大子序列
            int rightMax = solve3(arrs, center+1, right);
            //求解kua跨越左右两部分的最大子序列
            //1.求包含左部分最右元素的最大和
            int l = 0;
            int l_max = 0;
            for(int i = center; i >= left; i--){
                l += arrs[i];
                if(l > l_max){
                    l_max = l;
                }
            }
            //2.求包含右部分最左元素的最大和
            int r = 0;
            int r_max = 0;
            for(int i = center+1; i <= right; i++){
                r += arrs[i];
                if(r > r_max){
                    r_max = r;
                }
            }
            //跨越左右两部分的最大子序列
             max = l_max + r_max;
            //取三者最大值
            if(max < leftMax) max = leftMax;
            if(max < rightMax) max = rightMax;
        }
        return max;
    }

    //基于动态规划的思想
    public int solve4(int[] arrs){
        int max = 0;//存放目标子序列的最大值
        int temp = 0;//存放子序列的最大值
        for(int i = 0; i < arrs.length; i++){
            temp += arrs[i];
            if(temp > max){
                max = temp;
            }else{
                //如果这个子序列的值小于0，那么淘汰
                //从后面的子序列开始算起
                if(temp < 0){
                    temp = 0;
                }
             }
        }
        return max;
    }

```