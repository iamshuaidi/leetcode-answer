
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```
示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

```
     //方法1：先把lia链表中代表的数字算处理，然后在把两个数字相加，然后在拆分出来放进链表里
 public ListNode addTwoNumbers1(ListNode l1, ListNode l2) {
     int num1 = 0;//用来存放第一个链表代表的数字
     int num2 = 0;//用来存放第二个链表代表的数字
     int sum = 0;//存放和
     //t = 1,10,100....用来代表链表中的个位，十位，百位...
     int t = 1;//因为是从个位开始遍历的，所以初值为1
     //算第一个数字
     while(l1 != null){
         num1 = num1 + l1.val * t;
         t = t * 10;
         l1 = l1.next;
     }
     t = 1;
     //算第二个数字
     while(l2 != null){
         num2 = num2 + l2.val * t;
         t = t * 10;
         l2 = l2.next;
     }
     //相加
     sum = num1 + num2;
     //拆分出来放进链表里；
     ListNode head  = null;//作为链表的头节点
     ListNode temp = head;//跟踪链表
     while(sum > 0) {//结束条件为sum等于0
         if(head == null){
             head = new ListNode(sum % 10);
             temp = head;
         }else{
             temp.next = new ListNode(sum % 10);
             temp = temp.next;
         }
         sum = sum / 10;
     }
     //由于有可能sum一开始就为0，所以还需要在检查一下
     if(head == null){
         head = new ListNode(0);
     }
     return head;
    }

    //方法二
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int cout = 0;//用来检查是否有进位,默认没有进位
        ListNode head = null;//用来作为链表头
        ListNode temp = head;//用来跟踪链表
        //注意循环结束条件
        while(l1 != null && l2 != null){
            if(head == null){
                head = new ListNode((l1.val + l2.val + cout) % 10);
                temp = head;
            }else{
                temp.next = new ListNode((l1.val + l2.val + cout) % 10);
                temp = temp.next;
            }
            //检查是否有进位
            cout = (l1.val + l2.val + cout) / 10;
            l1 = l1.next;
            l2 = l2.next;
        }
        while(l1 != null){
            temp.next = new ListNode((l1.val + cout) % 10);
            cout = (l1.val + cout) / 10;
            l1 = l1.next;
            temp = temp.next;
        }
        while(l2 != null){
            temp.next = new ListNode((l2.val + cout) % 10);
            cout = (l2.val + cout) / 10;
            l2 = l2.next;
            temp = temp.next;

        }
        //最后还得在检测一下时候最高位有进位
        if(cout == 1){
            temp.next = new ListNode(cout);
        }
        return head;
    }

    //简洁版
    public ListNode addTwoNumbers2(ListNode l1, ListNode l2) {
        //作为头节点，且最后返回时不要这个头节点
        ListNode head = new ListNode(0);
        ListNode temp = head;//跟踪
        int cout = 0;

        while(l1 != null || l2 != null || cout != 0){
            if(l1 != null){
                cout += l1.val;
                l1 = l1.next;
            }
            if(l2 != null){
                cout += l2.val;
                l2 = l2.next;
            }
            temp.next = new ListNode(cout % 10);
            temp = temp.next;
            cout = cout / 10;
        }
        return head.next;

    }
```