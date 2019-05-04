给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

```
示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21
```

```
    public int reverse(int x) {
        //标记正负数
        boolean flag = false;
        //存放反转后的数
        long y = 0;
        if(x < 0){
            flag = true;
            x = -x;
        }
        //进行反转
        while(x > 0){
            y = y * 10 + x % 10;
            x = x / 10;
        }
        if(flag)
            y = -y;

        if(y > Math.pow(2,31) - 1 || y < Math.pow(-2,31)){
            return 0;
        }else{
            return (int)y;
        }
    }
```