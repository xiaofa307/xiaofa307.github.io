---
layout: post
title: Add Two Numbers2
categories: 算法
tags: leecode list
---

### no.237

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.  

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3,  

the linked list should become 1 -> 2 -> 4 after calling your function.

思路:题目没给多余的信息,只能通过一个node来进行删除,而且题目说了除了最后一个节点,所以  

把给定节点赋值成节点的next,其余节点依次赋值next即可.

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
    public class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }

	}

</code></pre>

