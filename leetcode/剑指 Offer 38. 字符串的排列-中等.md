# 一、题目
输入一个字符串，打印出该字符串中字符的所有排列。   
   
你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。    
    
**示例:**     
```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```
**限制：**     
- 1 <= s 的长度 <= 8
      
      
来源：力扣（LeetCode）     
链接：[https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof)      
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。     
# 二、分析及代码    
## 1. 按字典序求解
### （1）思路
针对输入的字符串，从最小排列开始，依次求解下一字典序的排列并输出，直至得到最大排列。      
### （2）代码
```java
class Solution {
    public String[] permutation(String s) {
        List<String> list = new ArrayList<>();//临时存储答案
        char[] str = s.toCharArray();//转为char[]简化代码
        Arrays.sort(str);//获得字典序最小的排列
        list.add(String.valueOf(str));
        while (nextPermutation(str) == true)//不断求解下一字典序的排列并添加答案
            list.add(String.valueOf(str));
        
        //将List<String>转为String[]
        int size = list.size();
        String[] ans = new String[size];
        for (int i = 0; i < size; i++)
            ans[i] = list.get(i);
        return ans;
    }

    //求解下一字典序的排列
    public boolean nextPermutation(char[] str) {
        int i = str.length - 2, j = str.length - 1;//i为从后向前第一个值不满足最大排列的位置，j为从后向前第一个值比i大的位置
        while (i >= 0 && str[i] >= str[i + 1])
            i--;
        if (i < 0)//当前的str已经是最大排列
            return false;
        while (j > i && str[i] >= str[j])
            j--;
         
        //交换str[i]和str[j]
        char temp = str[i];
        str[i] = str[j];
        str[j] = temp;

        //翻转str[i+1,length-1]
        int l = i + 1, r = str.length - 1;
        while (l < r) {
            temp = str[l];
            str[l++] = str[r];
            str[r--] = temp;
        }
        return true;
    }
}
```
### （3）结果
执行用时 ：8 ms，在所有 Java 提交中击败了 98.28% 的用户；    
内存消耗 ：42.7 MB，在所有 Java 提交中击败了 77.72% 的用户。      
# 三、其他
本题也可结合回溯法，完成所有排列的求解。  
