# 一、题目
小扣有一个根结点为 root 的二叉树模型，初始所有结点均为白色，可以用蓝色染料给模型结点染色，模型的每个结点有一个 val 价值。小扣出于美观考虑，希望最后二叉树上每个蓝色相连部分的结点个数不能超过 k 个，求所有染成蓝色的结点价值总和最大是多少？      
      
**示例 1：**    
```
输入：root = [5,2,3,4], k = 2
输出：12
解释：结点 5、3、4 染成蓝色，获得最大的价值 5+3+4=12
```
**示例 2：**      
```
输入：root = [4,1,3,9,null,null,2], k = 2
输出：16
解释：结点 4、3、9 染成蓝色，获得最大的价值 4+3+9=16
```
**提示：**      
- 1 <= k <= 10
- 1 <= val <= 10000
- 1 <= 结点数量 <= 10000
       
       
来源：力扣（LeetCode）     
链接：[https://leetcode-cn.com/problems/er-cha-shu-ran-se-UGC](https://leetcode-cn.com/problems/er-cha-shu-ran-se-UGC)        
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。       
# 二、分析及代码    
## 1. 动态规划
### （1）思路
二叉树中节点间的连接关系十分简明，因此可在深度优先遍历的同时，设计一个动态规划数组，记录包括当前节点在内连续染色 0 - k 个节点时所能提供的最大价值，自底向上递归求解。       
### （2）代码
```java
class Solution {
    int k;
    
    public int maxValue(TreeNode root, int k) {
        if (root == null)
            return 0;
        this.k = k;
        int[] vals = value(root);
        int ans = 0;
        for (int i = 0; i <= k; i++)
            ans = Math.max(ans, vals[i]);
        return ans;
    }
    
    //动态规划
    public int[] value(TreeNode node) {
        int[] vals = new int[k + 1];//包括当前节点合计提供连续的i个染色节点时, 提供的最大值
        Arrays.fill(vals, 0);
        //当前节点为叶节点
        if (node.left == null && node.right == null) {
            vals[1] = node.val;//叶节点最多只能提供连续的1个染色节点
            return vals;
        }
        
        //当前节点只有一个子节点
        int[] val1;//单个子节点提供的各最大值
        if (node.left == null || node.right == null) {
            if (node.left != null)//递归得到子节点提供的各最大值
                val1 = value(node.left);
            else
                val1 = value(node.right);
            vals[0] = val1[0];
            for (int i = 1; i <= k; i++) {
                vals[i] = node.val + val1[i - 1];//子节点提供连续的[0,k-1]个染色节点
                vals[0] = Math.max(vals[0], val1[i]);//当前节点不染色时的最大值，为所有子节点情况的最大值
            }
            return vals;
        }
        
        //当前节点有两个子节点
        int[] val2;//第二个子节点提供的各最大值
        val1 = value(node.left);//递归得到子节点提供的各最大值
        val2 = value(node.right);
        int val1Max = 0, val2Max = 0;//记录各子节点在最大值，用于不染色当前节点的情况
        for (int i = 0; i <= k; i++) {
            val1Max = Math.max(val1Max, val1[i]);
            val2Max = Math.max(val2Max, val2[i]);
        }
        vals[0] = val1Max + val2Max;
        for (int i = 1; i <= k; i++) {
            for (int l = 0; l < i; l++)//从左子树选择连续的l个染色节点，右子树选择i-1-l个
                vals[i] = Math.max(vals[i], val1[l] + val2[i - 1 - l]);
            vals[i] += node.val;//给当前节点染色
        }
        return vals;
    }
}
```
### （3）结果
执行用时 ：6 ms，在所有 Java 提交中击败了 99.52% 的用户；    
内存消耗 ：42.8 MB，在所有 Java 提交中击败了 49.76% 的用户。      
# 三、其他
暂无。  
