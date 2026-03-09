```java
public static ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        //create a node and iniliaze it with 0
        ListNode list3 = new ListNode(0);
        //then create another node which we will use for traversal and
        // list3 will always have the head address
        ListNode p = list3;
        //now while either of two nodes is not null
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                //if list1 node value is less we link that list1 node to p.next
                p.next = list1;
                list1 = list1.next;
            } else {
                p.next = list2;
                list2 = list2.next;
            }
            p=p.next;
        }
        //if list1 is null and list2 is all left then direct link p.next to list2 node
        if(list1==null){
            p.next=list2;
        } else if (list2 ==null) {
            p.next=list1;
        }
        // return list.next since first node we have initialzed with 0
        return list3.next;
    }
```