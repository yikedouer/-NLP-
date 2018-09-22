## 思路比较简单，位运算
- ^运算：逐位异或，相同的得0，不同的得1
- &1运算：取最后一位
- <<1运算：除去最后一位
```
class Solution {
    public int hammingDistance(int x, int y) {
        int xory = x ^ y;
        int res = 0;
        while(xory != 0){
            res += xory & 1;
            xory >>=1;
        }
        return res;
    }
}
```