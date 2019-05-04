将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

```
示例 1:

输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
示例 2:

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

```
	// 方法1
    public String convert(String s, int numRows) {
        //如果只有一行的话，直接返回
        if (s==null || s.length() <= 1 || numRows == 1)
            return s;
        //需要创建的数组个数
        int nums = Math.min(numRows, s.length());
        //用StringBuilder来充当数组方便一点
        StringBuilder[] rows = new StringBuilder[nums];
        for(int i = 0; i < nums; i++){
            rows[i] = new StringBuilder();
        }
        //表示是向下移动还是向上移动
        boolean flag = false;
        //当前遍历的行数
        int curRow = 0;
        for(int i = 0; i < s.length(); i++){
            rows[curRow].append(s.charAt(i));
            //第一行或者最后一行
            if(curRow == 0 || curRow == numRows - 1)
                flag = !flag;
            curRow += flag ? 1 : -1;
        }
        //全部拼接起来
        for(int i = 1; i < nums; i++){
            rows[0].append(rows[i].toString());
        }
        return rows[0].toString();
    }

	
	// 方法2
    public String convert1(String s, int numRows) {
        if (s==null || s.length() <= 1 || numRows == 1)
            return s;
        //每次都会用到2*numRows-2，所以用一个变量保存起来
        int temp = 2*numRows - 2;
        StringBuilder str = new StringBuilder();
        for(int i = 0; i < numRows; i++){
            //第0行
            if(i == 0){
                for(int k = 0; k * temp < s.length(); k++){
                    str.append(s.charAt(k*temp));
                }
            }
            //第numRows-1行
            else if(i == numRows -1){
                for(int k = 0; k*temp+numRows-1 < s.length();k++){
                    str.append(s.charAt(k*temp+numRows-1));
                }
            }
            //其他行
            else{
                for(int k = 0; k*temp+i < s.length(); k++){
                    str.append(s.charAt(k*temp+i));
                    if((k+1)*temp -i < s.length()){
                        str.append(s.charAt((k+1)*temp - i));
                    }
                }
            }
        }
        return str.toString();
    }

	// 方法3
    public String convert2(String s, int numRows) {
        if (s==null || s.length() <= 1 || numRows == 1)
            return s;
        int temp = 2*numRows - 2;
        StringBuilder str = new StringBuilder();
        for(int i = 0; i < numRows; i++){
            //此时的k相当于上面的k*temp
            for(int k = 0; k + i < s.length(); k += temp){
                //本来弄了很多注释，后来全删了，感觉还是不要弄注释了吧
                //假如不理解的话可以找几个字符串自己模拟想象下
                str.append(s.charAt(k+i));
                //如果是中间行则需要再加上一个字符
                if(i != 0 && i != numRows-1 && k + temp - i < s.length()){
                    str.append(s.charAt(k+temp-i));
                }
            }
        }
        return str.toString();
    }
```