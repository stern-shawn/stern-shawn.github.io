---
layout: post
title: Digging into algorithms... Level-Order Traversals!
tags: algorithms, python, hackerrank, problem-solving
---

### Some Backstory
Back in June, [HackerRank](hackerrank.com) was running a 30 Days of Code challenge. Thinking that this would be a perfect opportunity to brush up on the Python skills that I had picked up on a rushed need-to-know-for-this-project basis over the last year or so, I signed up and committed to solving the daily problems.

I've decided to go over a number of these challenges and my solutions going forward over the next few posts, as well as a series on some of the algorithm challenges and design projects I've been doing as part of the [FreeCodeCamp](freecodecamp.com) curriculum.

### Getting to it
Day 23 of 30 Days of Code brought back a concept from my second intro-level CS course: binary tree traversals. However, it came with a twist. Since having learned how to apply recursion, concepts like pre-order, in-order, and post-order traversals are naturally easy to think up and recode on the spot, but this challenge was asking for an implementation of Level-Order Traversal! Having never navigated a binary tree in this way, I was excited to figure out a solution, especially since a variant of this problem comes up in Cracking the Code Interview...

#### What is a Level-Order Traversal anyway?
While the previously mentioned traversals typically drill down into a BST until hitting the leaf nodes, the Level-Order traversal is unique in that it visits all nodes on a given level before descending to the next deepest level. Visually, you can think of it like below where the dashed line represents our path through the example tree from left to right, top to bottom:
![BFS Diagram]({{ site.baseurl }}/images/Sorted_binary_tree_breadth-first_traversal.svg "BFS Diagram")
[1]

Another name for this method of navigating the data structure is a Breadth-first search, which comes up more frequently in graph theory. I sort of wish this had been introduced along with the other traversals, because it would've made learning BFS that much easier in my algorithms course!

#### That's neat, how do we actually implement this?
