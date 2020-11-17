

# 根据链表算法学习对象引用



# 题目



给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

```java
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```



> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/add-two-numbers



## 解答 



```json
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
      
        ListNode pre = new ListNode(0);
        ListNode cur = pre;
      
        int carry = 0;
        while(l1 != null || l2 != null) {
            int x = l1 == null ? 0 : l1.val;
            int y = l2 == null ? 0 : l2.val;
            int sum = x + y + carry;
            
            carry = sum / 10;
            sum = sum % 10;
          
            cur.next = new ListNode(sum);
            cur = cur.next;
          
            if(l1 != null)
                l1 = l1.next;
            if(l2 != null)
                l2 = l2.next;
        }
        if(carry == 1) {
            cur.next = new ListNode(carry);
        }
        return pre.next;
    }
}
```



> 来源:  力扣(LeetCode)
>
> 链接: https://leetcode-cn.com/problems/add-two-numbers/solution/hua-jie-suan-fa-2-liang-shu-xiang-jia-by-guanpengc/



# 讲解学习内容



做Java开发挺长时间了, 突然看到这样的写法有点不知所以然, 首先我们看前两行代码

```java
    ListNode pre = new ListNode(0);
        ListNode cur = pre;
```



`pre` 在内存中申请了一块空间, `cur`也指向了那块空间



然后我们看第二块代码

```java
   cur.next = new ListNode(sum);
            cur = cur.next;
```



我看到的第一眼我蒙了, 你给` cur.next = new ListNode(sum)` 这句代码我可以理解`将一个新的链表追加到了下一个节点`, `cur = cur.next`你现在吧`cur`的指向更改了, 那结果还能对?  



`pre`不进行改变, `cur = cur.next;` 这句话也就代表了 `cur`等于`pre.next`  , `cur`改变了内存指向, 所以他的改变不会影响`pre` 而 `pre`的`next`属性依旧在向下关联





# 画图解析



## 第一步



![image text](https://raw.githubusercontent.com/huxuekuo/note/master/images/LeetCode两数相加_1.png)

## 第二步



![image text](https://raw.githubusercontent.com/huxuekuo/note/master/images/LeetCode两数相加_2.jpg)

