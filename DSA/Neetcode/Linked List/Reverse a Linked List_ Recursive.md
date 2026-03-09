**Using recurrence;  
<br/>**

```java
//while solving this problem focus on recursion breaking condition
//suppose this is list p -> q -> r -> s -> t -> u -> null
//our recursion will end when head goes to u
//then it will return and go to t
//now head = t
//consider this link
// t -> u
// expected link u -> t
//for that we need to do following changes
// u.next=t
// t.next=null (break link between t to u)
// t=head and u=head.next
//
//which is equivalent to 
//
// head.next.next=head
// head.next=null





    public static ListNode reverseList(ListNode head) {

        if (head == null || head.next == null) {
            return head;
        }

       // We're using newHead  to keep track of the new head of the reversed list.
       //newHead is updated at each recursive call to point to the new head of the reversed list.
       // When the recursion ends, newHead points to the original last node (u),
       // which is now the new head of the reversed list.
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }

```

&nbsp;