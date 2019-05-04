给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

```
示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

```
    //三个for暴力解决
    public static int lengthOfLongestSubstring(String s) {
        int max = 0;
        int temp = 0;
        char[] arr = new char[s.length()];
        for(int i = 0; i < s.length(); i++){
            arr[i] = s.charAt(i);
            temp = 1;
            for(int j = i + 1; j < s.length(); j++){
                int flag = 0;//用来判断s[j]是否在arr中
                for(int k = i; k < i + temp; k++){
                    if(arr[k] == s.charAt(j)){
                        flag = 1;
                        break;
                    }
                }
                if(flag == 1){ break;//重复
                } else {
                    arr[i+temp] = s.charAt(j);
                    temp++;
                }
            }
            if(temp > max){
                max = temp;
            }
        }
        return max;
    }

    //用hashMap映射
    public static int lengthOfLongestSubstring2(String s) {
        int max = 0;//保存最长子串的长度
        //用来记录子串是从哪个下标开始的
        int i = 0;
        Map<Character, Integer> map = new HashMap<>();

        for(int j = 0; j < s.length(); j++){
            if(map.containsKey(s.charAt(j))){
                //从第一个重复元素的后一个开始
                i = Math.max(map.get(s.charAt(j))+ 1, i);
            }
            //j - i + 1 表示计算此时子串的长度
            max = Math.max(max, j - i + 1);
            map.put(s.charAt(j), j);
        }
        return max;
    }
```