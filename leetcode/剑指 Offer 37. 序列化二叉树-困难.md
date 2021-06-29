# 一、题目
请实现两个函数，分别用来序列化和反序列化二叉树。      
      
你需要设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。     
      
提示：输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://leetcode-cn.com/faq/#binary-tree)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。       
    
**示例：**     
```
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
```
注意：本题与主站 297 题相同：https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/     
     
来源：力扣（LeetCode）     
链接：[https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof)      
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。     
# 二、分析及代码    
## 1. 前序遍历
### （1）思路
结合前序遍历方法，对二叉树进行序列化和反序列化。为简化代码，可用 "#" 表示空节点，用 "," 分隔不同节点的值。      
### （2）代码
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null)//空节点用"#"表示
            return "#";
        //通过前序遍历序列化
        StringBuffer sb = new StringBuffer();
        sb.append(String.valueOf(root.val));
        sb.append(",");//用","分隔节点
        sb.append(serialize(root.left));
        sb.append(",");
        sb.append(serialize(root.right));
        return sb.toString();        
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.length() == 0)
            return null;
        String[] nodeArr = data.split(",");//将序列化得到的整个字符串按分隔符解码
        List<String> nodeList = new LinkedList<String>(Arrays.asList(nodeArr));
        return DLR(nodeList);
    }

    //通过前序遍历反序列化
    public TreeNode DLR(List<String> nodeList) {
        String nodeStr =  nodeList.get(0);
        nodeList.remove(0);
        if (nodeStr.equals("#"))
            return null;

        TreeNode root = new TreeNode(Integer.valueOf(nodeStr));
        root.left = DLR(nodeList);
        root.right = DLR(nodeList);
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```
### （3）结果
执行用时 ：14 ms，在所有 Java 提交中击败了 88.35% 的用户；    
内存消耗 ：40.2 MB，在所有 Java 提交中击败了 76.67% 的用户。      
# 三、其他
暂无。  
