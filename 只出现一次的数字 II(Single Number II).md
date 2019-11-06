# 一、题目
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。  
  
说明：  
  
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？  
  
示例 1:  
```c++
输入: [2,2,3,2]
输出: 3
```
示例 2:  
```c++
输入: [0,1,0,1,0,1,99]
输出: 99
```
来源：力扣（LeetCode）  
链接：[https://leetcode-cn.com/problems/single-number-ii](https://leetcode-cn.com/problems/single-number-ii)  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  
# 二、分析及代码
## 1. 设计“三进制”
### （1）思路
结合第136题“只出现一次的数字”解题思路，本题的关键是：  
设计一种“三进制”，num 第一次出现时值为 num，num第三次出现时值为0。  
（具体过程可通过真值表推算，或结合数字电路设计方法）  
其中一种算法为：  
```c++
one = one ^ nums[i] & ~two;//第1次出现时为1，第2、3次为0
two = two ^ nums[i] & ~one;//第2次出现时为1，第1、3次为0
```
### （2）代码
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        if (nums.empty())
            return 0;
        int one = 0, two = 0;
        for (int i = 0; i < nums.size(); i++) {
            one = one ^ nums[i] & ~two;
            two = two ^ nums[i] & ~one;
        }
        return one;        
    }
};
```
### （3）结果
执行用时 ：12 ms, 在所有 C++ 提交中击败了 82.45% 的用户；  
内存消耗 ：9.6 MB, 在所有 C++ 提交中击败了 66.41% 的用户。  
# 三、其他
暂无。  
