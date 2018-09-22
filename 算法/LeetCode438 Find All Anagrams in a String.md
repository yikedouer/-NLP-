*Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.The order of output does not*

**Input:
s: "cbaebabacd" p: "abc"
Output:
[0, 6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".**

# 基本思想：
- **将被匹配的串放进一个Hash表里，加快查找速度**
- **定义左右两个指针，维护一个窗口，定义一个计数器，表示目前待匹配的字符个数**
- **循环**
    - **在循环最开始，两个指针均指向s的最左端，计数器为p的长度**
    - **移动右指针，不断将字符添加进窗口中，判断该字符在Hash中的值是否大于1，如果大于一，说明这个字符在p中，我们已经匹配到了一个，计数器减一。不论其在Hash中的值是否大于1，都将其在Hash表中的次数减一，表示我当前的串口中已经包含了一个该字符。**
    - **判断计数器是否为0，为零的话说明我们找到了一个完全匹配，将左指针加进返回值里**
    - **当左右指针相差p的长度时，窗口撑到了最大，需要移动左指针继续遍历。首先判断左指针指向的字符在Hash中的值是否大于零，如果大于零，说明该字符是在p中的，那么我们就相当于在窗口中除去了一个需要匹配的字符，因此计数器要加回1.不管以上条件是否满足，窗口中的字符都少了一个，必须把Hash表中该字符的出现次数加一**
- **循环结束，返回结果**

***代码如下***：

**未优化版**
```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<Integer>();
        if (s == null || p == null || s.length() < p.length()){
            return res;
        }
        int[] map = new int[256];
        int left = 0,right = 0, cnt = p.length();
        for(char c : p.toCharArray()){
            map[c]++;
        }
        while (right < s.length()){
            if(map[s.charAt(right)] > 0 ){
                cnt--;
            }
            map[s.charAt(right)]--;
            right++;
            if(cnt == 0){
                res.add(left);
            }
            if(right - left == p.length()){
                if (map[s.charAt(left)] >= 0){
                    cnt++;
                }
                map[s.charAt(left)]++;
                left++;
            }
            
        }
        return res;
    }
}
```
**简洁优化版**
```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<Integer>();
        if (s == null || p == null || s.length() < p.length()){
            return res;
        }
        int[] map = new int[256];
        int left = 0,right = 0, cnt = p.length();
        for(char c : p.toCharArray()){
            map[c]++;
        }
        while (right < s.length()){
            if(map[s.charAt(right++)]-- > 0 ){
                cnt--;
            }
            if(cnt == 0){
                res.add(left);
            }
            if(right - left == p.length() && map[s.charAt(left++)]++ >= 0){
                cnt++;
            }
            
        }
        return res;
    }
}
```

另外，建立的map是256大小的，其实完全可以改成26大小的，不过代码要稍加修改，对每个字符要减去'a'

**Todo:**
    总结下滑动窗口的模板，再练习几道类似的题目