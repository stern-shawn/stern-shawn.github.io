---
layout: post
title: Digging into algorithms... Level-Order Traversals!
tags: algorithms, python, hackerrank, problem-solving
---

### Some Backstory
Back in June, [HackerRank](http://www.hackerrank.com) was running a 30 Days of Code challenge. Thinking that this would be a perfect opportunity to brush up on the Python skills that I had picked up on a rushed need-to-know-for-this-project basis over the last year or so, I signed up and committed to solving the daily problems.

I've decided to go over a number of these challenges and my solutions going forward over the next few posts, as well as a series on some of the algorithm challenges and design projects I've been doing as part of the [FreeCodeCamp](http://www.freecodecamp.com) curriculum.

### Getting to it
Day 23 of 30 Days of Code brought back a concept from my second intro-level CS course: binary tree traversals. However, it came with a twist. Since having learned how to apply recursion, concepts like pre-order, in-order, and post-order traversals are naturally easy to think up and recode on the spot, but this challenge was asking for an implementation of Level-Order Traversal! Having never navigated a binary tree in this way, I was excited to figure out a solution, especially since a variant of this problem comes up in Cracking the Code Interview...

#### What is a Level-Order Traversal anyway?
While the previously mentioned traversals typically drill down into a BST until hitting the leaf nodes, the Level-Order traversal is unique in that it visits all nodes on a given level before descending to the next deepest level. Visually, you can think of it like below where the dashed line represents our path through the example tree from left to right, top to bottom:

![BFS Diagram]({{ site.baseurl }}/images/Sorted_binary_tree_breadth-first_traversal.svg "BFS Diagram")
[1]

Another name for this method of navigating the data structure is a Breadth-first search, which comes up more frequently in graph theory. I sort of wish this had been introduced along with the other traversals, because it would've made learning BFS that much easier in my algorithms course!

#### That's neat, how do we actually implement this?
This one was a challenge to start but ultimately made a ton of sense. Having done all other traversals using recursive methods, my first instinct was to also tackle this problem in the same way.

After some time spent banging my head against the wall, and thinking that there was no way to do this purely recursively without some helper data structure to keep track of your position in the tree, I did some googling and ran across this pseudo-code on Wikipedia.

```
levelorder(root)
  q ← empty queue
  q.enqueue(root)
  while (not q.isEmpty())
    node ← q.dequeue()
    visit(node)
    if (node.left ≠ null)
      q.enqueue(node.left)
    if (node.right ≠ null)
      q.enqueue(node.right)
```

Look at that, we aren't even recursing! Instead, this solution relies on a First In First Out (FIFO) Queue data structure as a helper. *(Think of how lining up to order food works: the first person in line will reach the counter first. This is different from a Last In First Out (LIFO) Queue, which works more like an elevator: the most recent person to get in is able to get out first.)* If we're given the root node, we first queue it up, pop it out, "visit" the node (in our case, we're just printing its value), and then throw all of it's existing children nodes into the queue, starting with the left child.

Upon the next iteration of the loop, that leftmost child pops out, and the same operation occurs. Because we're using a queue, the new child nodes of THIS current node are added to the end of the existing queue, and won't be processed until after everything on this level, which was enqueued earlier, is visited. All without recursion!

Pseudo-code and explanations in English are great, but problems are solved using real code. Let's take a look at how I interpreted this into Python as part of a Binary Search Tree Class implementation.

```python
def levelOrder(self,root):
  # Perform a level-order traversal of the given root node
  if root is not None:
    # Establish a 'queue' starting with root
    queue = [root]
    while(queue):
      # 'De-queue' first element
      curr = queue.pop(0)

      # Print the current node on the same line, pad with a space
      print(curr.data, end=" ")

      # Enqueue any left/right children in order
      if (curr.left is not None):
        queue.append(curr.left)
      if (curr.right is not None):
        queue.append(curr.right)
```

First off, as good practice it's smart to check if the input even exists, or is non-null in this case. This allows the code to avoid attempting to process something undefined and throwing errors.

The next line, `queue = [root]`, is straightforward enough, this is where we establish the beginnings of the algorithm by initializing the queue using our definitely-not-null root node. But wait, this isn't really a queue, persay; we're just using a Python list since this was initialized using square brackets. Ignore that for now, I'll explain this later!

`while(queue):` manages the iterative looping over the tree, and the condition of simply 'queue' vs having some more involved logic of determining if the queue is empty or not is just another fun feature of using the language Python. If the queue is empty, evaluating it as a boolean will return false, just the same as executing something along the lines of queue.isEmpty() in another language.

`curr = queue.pop(0)` is where we are able to squeeze queue-like functionality out of Python's list data structure. By popping the element at index 0, we're effectively dequeuing the first element and letting the remaining elements shift over by one to fill its place. In terms of absolute performance this may not be optimal, since the pop operation itself now has to shift its contents around. If I were approaching this problem with absolute performance in mind, it would be better to manually implement or import an existing queue object with pointers to the beginning and end elements that doesn't need to readjust all of its contents on each .pop() operation. However, for my purposes of simply getting something to produce the desired output, this works perfectly and is simpler.

`print(curr.data, end=" ")` This is simply the "visiting" portion of the algorithm. One requirement of the challenge was to print the contents of the tree on a single line, with values delimited by spaces. In other languages such as Java or C which have separate print() and println() functions which print to the same line or print and move to the next line separately, this wouldn't require much thought. However, having only experience with Python's print() function and knowing that it always seemed to print a newline character meant I needed to find a way to call print() an undetermined number of times with a space on the end instead of a newline. Luckily, some quick research through the [Python documentation](https://docs.python.org/3/tutorial/inputoutput.html) provided a reasonable solution, the end=" " argument which allows the coder to define an alternate end character for the print statement other than a newline character!

```python
if (curr.left is not None):
  queue.append(curr.left)
if (curr.right is not None):
  queue.append(curr.right)
```
The block above is pretty straightforward if you get familiar with the Python syntax (which I used earlier to check for the existence of the root node). Here, the code is checking to see if the left child node exists or not, and adding it to the queue if so. Same for the right.

And that is all! If I were to run the full code on this tree below, the output would be a correct 3 2 5 1 4 7.

![Example]({{ site.baseurl }}/images/bstexample.JPG "Example")

----

Full source code for my solution, as well as the problem statement provided by HackerRank, can be found [here](https://github.com/stern-shawn/HackerRank/tree/master/30DaysOfCode/23%20-%20BST%20Level-Order%20Traversal)!

----

Sources:

[1] - Image from: <https://en.wikipedia.org/wiki/Tree_traversal>
