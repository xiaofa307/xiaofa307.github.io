---
layout: post
title: leecode-Add Two Numbers
categories: 算法
tags: leecode
---

### LeeCode - no.2 add two numbers

ou are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

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
	         ListNode prev = new ListNode(0);
	    	ListNode result = prev;
	    	int next = 0;
	    	
	    	while(null != l1 || null != l2 || next>0){
	    		ListNode temp = new ListNode(0);
	    		int sum = (null == l1 ? 0 : l1.val )+ (null == l2 ? 0 : l2.val )+ next;
	    		temp.val = (sum)%10;
	    		next = (sum)/10;
	    		prev.next = temp;
	    		prev = temp;
	    		l1 = l1 == null ?  null : l1.next; 
	    		l2 = l2 == null ?  null : l2.next; 
	    		
	    	}
	    	return result.next;
	        
	    }
	}

</code></pre>

