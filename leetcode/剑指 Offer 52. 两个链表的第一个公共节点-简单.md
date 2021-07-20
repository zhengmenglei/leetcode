# 一、题目
输入两个链表，找出它们的第一个公共节点。     
    
**示例 1：**     
```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```
**示例 2：**    
```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```
**示例 3：**    
```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```
**注意：**      
- 如果两个链表没有交点，返回 null.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。
- 本题与主站 160 题相同：[https://leetcode-cn.com/problems/intersection-of-two-linked-lists/](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)      
      
      
来源：力扣（LeetCode）    
链接：[https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof)     
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。    
# 二、分析及代码    
## 1. 双指针
### （1）思路
经典题目，设计 2 个指针，分别从链表 A、B 的头部开始同步遍历，当前链表完成时则从另一链表的头部继续，直至二者指向的内容相同。      
      
**原理：**     
假设链表 A、B 交点前的长度分别为 a、b，交点及之后的共同部分长度为 c。根据上述方法同步进行遍历，相遇时 2 个指针移动的距离 a + c + b == b + c + a，指针指向第一个公共节点。      
链表没有交点的情况，相当于 c = 0。根据上述方法，二者会在 2 个链表的末尾空节点处相遇，此时 a + b == b + a，指针指向 null。       
       
### （2）代码
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null)//任意链表为空链表，公共节点只能是空节点
            return null;
        ListNode node1 = headA, node2 = headB;//双指针，分别从链表A、B的头部节点开始遍历
        while (node1 != node2) {
            node1 = node1 == null ? headB : node1.next;//遍历完链表A后，从链表B的头部开始第二轮遍历
            node2 = node2 == null ? headA : node2.next;//遍历完链表B后，从链表A的头部开始第二轮遍历
        }
        return node1;//返回公共节点，可能为交点或空节点
    }
}
```
### （3）结果
执行用时 ：1 ms，在所有 Java 提交中击败了 100.00% 的用户；    
内存消耗 ：41.1 MB，在所有 Java 提交中击败了 65.13% 的用户。      
# 三、其他
暂无。  
