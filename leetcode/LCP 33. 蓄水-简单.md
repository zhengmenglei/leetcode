# 一、题目
给定 N 个无限容量且初始均空的水缸，每个水缸配有一个水桶用来打水，第 i 个水缸配备的水桶容量记作 bucket[i]。小扣有以下两种操作：       
- 升级水桶：选择任意一个水桶，使其容量增加为 bucket[i]+1
- 蓄水：将全部水桶接满水，倒入各自对应的水缸
        
        
每个水缸对应最低蓄水量记作 vat[i]，返回小扣至少需要多少次操作可以完成所有水缸蓄水要求。      
        
注意：实际蓄水量 达到或超过 最低蓄水量，即完成蓄水要求。       
        
**示例 1：**     
```
输入：bucket = [1,3], vat = [6,8]
输出：4
解释：
第 1 次操作升级 bucket[0]；
第 2 ~ 4 次操作均选择蓄水，即可完成蓄水要求。
```
**示例 2：**    
```
输入：bucket = [9,0,1], vat = [0,2,2]
输出：3
解释：
第 1 次操作均选择升级 bucket[1]
第 2~3 次操作选择蓄水，即可完成蓄水要求。
```
**提示：**       
- 1 <= bucket.length == vat.length <= 100
- 0 <= bucket[i], vat[i] <= 10^4
       
       
来源：力扣（LeetCode）        
链接：[https://leetcode-cn.com/problems/o8SXZn](https://leetcode-cn.com/problems/o8SXZn)        
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。        
# 二、分析及代码    
## 1. 遍历
### （1）思路
结合贪心算法，在打水过程中，小扣应该先升级水桶再蓄水。因此，当蓄水次数为 i 时，各水桶所需的升级次数为 Math.max(0, (vat[j] + i - 1) / i - bucket[j])。       
针对所有水缸，可能的最多蓄水次数为：
- 水桶容量大于 0，最大次数为 vat[i] + bucket[i] - 1) / bucket[i]
- 水桶容量等于 0，最大次数为 vat[i] + 1
         
         
结合上述方法，遍历计算各蓄水次数对应的操作次数，获取最小值。过程中还可通过已获得的最小值进行剪枝，当拟计算的蓄水次数已超过该值时可直接跳出。      
### （2）代码
```java
class Solution {
    public int storeWater(int[] bucket, int[] vat) {
        if (bucket.length == 0)
            return 0;
        int n = bucket.length, ans = Integer.MAX_VALUE, maxTime = 0;
        for (int i = 0; i < n; i++) {//统计最多蓄水次数
            if (vat[i] > 0) {
                if (bucket[i] > 0)
                    maxTime = Math.max(maxTime, (vat[i] + bucket[i] - 1) / bucket[i]);
                else
                    maxTime = Math.max(maxTime, vat[i] + 1);
            }
        }
        if (maxTime == 0)//水缸无蓄水要求
            return 0;
        
        for (int i = 1; i <= maxTime && i < ans; i++) {
            int time = i;//全部水桶蓄水i次
            for (int j = 0; j < n; j++)
                time += Math.max(0, (vat[j] + i - 1) / i - bucket[j]);//各水桶升级次数
            ans = Math.min(ans, time);
        }
        return ans;
    }
}
```
### （3）结果
执行用时 ：4 ms，在所有 Java 提交中击败了 88.61% 的用户；    
内存消耗 ：36.3 MB，在所有 Java 提交中击败了 34.60% 的用户。      
# 三、其他
暂无。  
