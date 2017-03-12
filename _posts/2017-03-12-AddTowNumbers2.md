---
layout: post
title: Add Two Numbers2
categories: 算法
tags: leecode list
---

### no.445

Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)  

Output: 7 -> 8 -> 0 -> 7  

思路:使用stack,每次计算两组stack pop出来的值,  

注意next使用中间变量过渡。

---

<pre><code>

	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
  public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
            
    	Stack<Integer> l1Stack = new Stack<Integer>();
    	Stack<Integer> l2Stack = new Stack<Integer>();
    	while(l1!= null){
    		
    		l1Stack.push(l1.val);
    		l1 = l1.next;
    	}
    	while(l2!= null){
    		
    		l2Stack.push(l2.val);
    		l2 = l2.next;
    	}
    	Integer sum = 0;
    	ListNode resultNode = new ListNode(0);
    	while(sum!=0 || l1Stack.size()>0 || l2Stack.size()>0){
    		Integer temp1 = 0;
    		Integer temp2 = 0;
    		if(l1Stack.size()>0){
    			
    			temp1 = l1Stack.pop();
    		}
    		if(l2Stack.size()>0){
    			
    			temp2 = l2Stack.pop();
    		}
    		Integer tempNodeVal = temp1+temp2+sum;
    		sum = tempNodeVal/10;
    		ListNode newNode = new ListNode(tempNodeVal%10);
    		ListNode tempNode = resultNode.next;
    		resultNode.next = newNode;
    		newNode.next = tempNode;
    	}
    	return resultNode.next;
    }
}

</code></pre>

