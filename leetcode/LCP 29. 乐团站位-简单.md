# 一、题目
某乐团的演出场地可视作 num * num 的二维矩阵 grid（左上角坐标为 [0,0])，每个位置站有一位成员。乐团共有 9 种乐器，乐器编号为 1~9，每位成员持有 1 个乐器。        
为保证声乐混合效果，成员站位规则为：自 grid 左上角开始顺时针螺旋形向内循环以 1，2，...，9 循环重复排列。       
       
请返回位于场地坐标 [Xpos,Ypos] 的成员所持乐器编号。      
      
**示例 1：**     
```
输入：num = 3, Xpos = 0, Ypos = 2
输出：3
```
**示例 2：**    
```
输入：num = 4, Xpos = 1, Ypos = 2
输出：5
```
**提示：**      
- 1 <= num <= 10^9
- 0 <= Xpos, Ypos < num
      
来源：力扣（LeetCode）     
链接：[https://leetcode-cn.com/problems/SNJvJP](https://leetcode-cn.com/problems/SNJvJP)       
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。       
# 二、分析及代码    
## 1. 直接计算
### （1）思路
先确定成员所在的圈数，然后结合数学方法计算得到该圈左上角的起始值，再判断成员所在的边，计算得到乐器编号。      
### （2）代码
```java
class Solution {
    public int orchestraLayout(int num, int xPos, int yPos) {
        //确定成员在第round圈
        int xRound = Math.min(xPos + 1, num - xPos);
        int yRound = Math.min(yPos + 1, num - yPos);
        int round = Math.min(xRound, yRound);
        int len = num - 1;//当前圈边的长度
        
        //确定成员所在圈左上角起始值
        long start = 1L;
        start = (start + (len - round + 2L) * (round - 1L) * 4L) % 9L;
        len -= ((round - 1) * 2);
        
        //确定成员乐器编号
        int ans;
        if (xPos + 1 == round) {//成员在上边
            ans = (int)((start + yPos + 1 - round) % 9);
        } else if (num - yPos == round){//成员在右边
            start = (start + (len % 9)) % 9;
            ans = (int)((start + xPos + 1 - round) % 9);
        } else if (num - xPos == round){//成员在下边
            start = (start + (len % 9) * 2) % 9;
            ans = (int)((start + num - round - yPos) % 9);
        } else {//成员在左边
            start = (start + (len % 9) * 3) % 9;
            ans = (int)((start + num - round - xPos) % 9);
        }
        return (ans == 0) ? 9 : ans;//也可将start初始值设置为0，返回ans%9+1
    }
}
```
### （3）结果
执行用时 ：0 ms，在所有 Java 提交中击败了 100.00% 的用户；    
内存消耗 ：34.9 MB，在所有 Java 提交中击败了 96.57% 的用户。      
# 三、其他
暂无。  
