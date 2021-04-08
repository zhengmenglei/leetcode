# 一、题目
小力将 N 个零件的报价存于数组 nums。小力预算为 target，假定小力仅购买两个零件，要求购买零件的花费不超过预算，请问他有多少种采购方案。      
注意：答案需要以 1e9 + 7 (1000000007) 为底取模，如：计算初始结果为：1000000008，请返回 1       
      
**示例 1：**     
```
输入：nums = [2,5,3,5], target = 6
输出：1
解释：预算内仅能购买 nums[0] 与 nums[2]。
```
**示例 2：**     
```
输入：nums = [2,2,1,9], target = 10
输出：4
解释：符合预算的采购方案如下：
nums[0] + nums[1] = 4
nums[0] + nums[2] = 3
nums[1] + nums[2] = 3
nums[2] + nums[3] = 10
```
**提示：**     
- 2 <= nums.length <= 10^5
- 1 <= nums[i], target <= 10^5
       
来源：力扣（LeetCode）       
链接：[https://leetcode-cn.com/problems/4xy4Wx](https://leetcode-cn.com/problems/4xy4Wx)        
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。      
# 二、分析及代码    
## 1. 双指针
### （1）思路
对报价数组 nums 排序后，在首尾位置设计 2 个指针。依次移动左指针，并将右指针移动到不超过预算所能购买的最大范围，该区间的长度就是以当前左指针对应零件为较便宜部分的采购方案数。      
### （2）代码
```java
class Solution {
    public int purchasePlans(int[] nums, int target) {
        int n = nums.length, mod = 1000000007;
        Arrays.sort(nums);
        int l = 0, r = n - 1;//双指针
        long ans = 0L;
        while (l < r) {
            while (l < r && nums[l] + nums[r] > target)//超出范围，移动右指针
                r--;
            ans = (ans + r - l) % mod;
            l++;//当前位置计算完成，移动左指针
        }
        return (int)ans;
    }
}
```
### （3）结果
执行用时 ：37 ms，在所有 Java 提交中击败了 59.11% 的用户；    
内存消耗 ：47.9 MB，在所有 Java 提交中击败了 95.04% 的用户。      
# 三、其他
还可先统计各个价格对应的零件数，再结合双指针方法进行计算。  
