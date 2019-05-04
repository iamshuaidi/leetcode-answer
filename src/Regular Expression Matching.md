给定一个字符串 (s) 和一个字符模式 (p)。实现支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符。
'*' 匹配零个或多个前面的元素。
匹配应该覆盖整个字符串 (s) ，而不是部分字符串。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。

```
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
示例 2:

输入:
s = "aa"
p = "a*"
输出: true
解释: '*' 代表可匹配零个或多个前面的元素, 即可以匹配 'a' 。因此, 重复 'a' 一次, 字符串可变为 "aa"。
示例 3:

输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个('*')任意字符('.')。
示例 4:

输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 'c' 可以不被重复, 'a' 可以被重复一次。因此可以匹配字符串 "aab"。
示例 5:

输入:
s = "mississippi"
p = "mis*is*p*."
输出: false

```


```
	// 方法1：暴力
    public boolean isMatch(String s, String p) {
        if(s == null || p == null)
            return false;
        if(s.length() >= 1 && p.length() < 1)
            return false;
        int i = 0;
        int j = 0;
        return match(s, i, p, j);
    }



    public  boolean match(String s, int i, String p, int j){
        //先验证是否匹配结束
        //同时都到结尾了
        if(i >= s.length() && j >= p.length())
            return true;
        //s还没结尾，但p到达结尾了
        if(i < s.length() && j >= p.length())
            return false;

        //如果下一个字符不是*，或者这个字符就是最后一个字符了，则直接匹配
        if(j+1<p.length() && p.charAt(j+1)!='*' || p.length()==j+1){
            if(i<s.length() && (s.charAt(i)==p.charAt(j)||p.charAt(j)=='.'))
                return match(s,i+1, p, j+1);
            else
                return false;
        }else{
            //下一个字符是*，则分为两种种情况
            //1.如果当前字符不匹配(或者s已经越界了)，
            // 则p字符移动2位，而s不移动
            if((i<s.length()&&s.charAt(i)!=p.charAt(j)&&p.charAt(j)!='.') || i == s.length())
                return match(s, i, p, j+2);
            //2.如果当前字符匹配的话，分两种情况讨论
            else{
                //(1).匹配0个字符,则p直接移动2位,s不移动
                //(2).匹配一个，s移动一位，p移动两位
                //(3),匹配多个，s移动一位，p不移动
                return match(s, i, p, j+2) || match(s, i+1, p, j) ||
                        match(s, i+1, p, j+2);
            }
        }
    }

    //方法2：采用dp
    public boolean isMatch1(String s, String p){
        if(s == null || p == null)
            return false;
        int len_s = s.length();
        int len_p = p.length();
        //存放状态
        boolean[][]dp = new boolean[len_s+1][len_p+1];
        //初始化
        dp[0][0] = true;
        for(int i = 1; i <= len_p; i++){
            if(p.charAt(i-1) == '*')
                dp[0][i] = dp[0][i-2];
        }
        for(int i = 1; i <= len_s; i++){
            for(int j = 1; j <= len_p; j++){
                //如果不为‘*’且匹配
                if(p.charAt(j-1)=='.'||p.charAt(j-1)==s.charAt(i-1))
                    dp[i][j] = dp[i-1][j-1];
                //如果是*
                else if(p.charAt(j-1)=='*'){
                    //如果p中*号前面的字符与s当前字符不匹配，则匹配0个
                    if(j!=1&&p.charAt(j-2)!='.'&&p.charAt(j-2)!=s.charAt(i-1)){
                        dp[i][j] = dp[i][j-2];
                    }else{
                        //否则有三种情况：
                        //匹配0个，匹配1个，匹配多个
                        dp[i][j] = dp[i][j-2] || dp[i][j-1]||dp[i-1][j];
                    }
                }
            }
        }
        return dp[len_s][len_p];
    }
```