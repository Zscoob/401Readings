# Trees
## 401 class 15 reading

Common Terminology
Node - A node is the individual item/data that makes up the data structure
Root - The root is the first/top Node in the tree
Left Child - The node that is positioned to the left of a root or node
Right Child - The node that is positioned to the right of a root or node
Edge - The edge in a tree is the link between a parent and child node
Leaf - A leaf is a node that does not contain any children
Height - The height of a tree is determined by the number of edges from the root to the bottommost node
Sample Tree
Sample Tree

Traversals
An important aspect of trees is how to traverse them. Traversing a tree allows us to search for a node, print out the contents of a tree, and much more! There are two categories of traversals when it comes to trees:

Depth First
Breadth First
Depth First
Depth first traversal is where we prioritize going through the depth (height) of the tree first. There are multiple ways to carry out depth first traversal, and each method changes the order in which we search/print the root. Here are three methods for depth first traversal:

Pre-order: root >> left >> right
In-order: left >> root >> right
Post-order: left >> right >> root
Sample Tree

Given the sample tree above, our traversals would result in different paths:

Pre-order: A, B, D, E, C, F
In-order: D, B, E, A, F, C
Post-order: D, E, B, F, C, A
The most common way to traverse through a tree is to use recursion. With these traversals, we rely on the call stack to navigate back up the tree when we have reached the end of a sub-path.

Pre-order
Let’s break down the pre-order traversal. Here is the pseudocode for this traversal method:

ALGORITHM preOrder(root)

  OUTPUT <-- root.value

  if root.left is not NULL
      preOrder(root.left)

  if root.right is not NULL
      preOrder(root.right)

Pre-order means that the root has to be looked at first. In our case, looking at the root just means that we output its value. When we call preOrder for the first time, the root will be added to the call stack:

PreOrder 01

Next, we start reading our preOrder function’s code from top to bottom. The first line of code reads this:

OUTPUT <-- root.value
This means that we will output the root.value out to the console. Then, our next block of code instructs us to check if our root has a left node set. If the root does, we will then send the left node to our preOrder method recursively. This means that we make another function call, where B is our new root:

PreOrder 02

This process continues until we reach a leaf node. Here’s the state of our tree when we hit our first leaf, D:

PreOrder 03

It’s important to note a few things that are about to happen:

The program will look for both a root.left and a root.right. Both will return null, so it will end the execution of that method call
D will pop off of the call stack and the root will be reassigned back to B
This is the heart of recursion: when we complete a function call, we pop it off the stack and are able to continue execution through the previous function call
PreOrder 04

The code block will now pick up where it left off when B was the root. Since it already looked for root.left, it will now look for root.right.

PreOrder 05

E will output to the console. Since E is a leaf, it will complete the method code block, and pop E off of the call stack and make its way back up to B.

PreOrder 06

In the function call, B has already checked for root.left, and root.right. There are no further lines of code to execute, so B will be popped off the call stack, so that we can resume execution of A.

PreOrder 07

Following the same pattern as we did with the other nodes, A’s call stack frame will pick up where it left off, and check out root.right. C will be added to the call stack frame, and it will the new function’s root.

PreOrder 08

C will be outputted to the console, and root.left will be evaluated. Because C has a left child, preOrder will be called, with the parameter root.left.

PreOrder 09

At this point, the program will find that F does not have any children and it will make its way back up the call stack up to C.

PreOrder 10

C does not have a root.right, so it will pop off the call stack and return to A.

PreOrder 11

Congratulations! Your pre-order traversal is completed!

Traversal Pseudocode
Here is the pseudocode for all three of the depth first traversals:

Pre-order
ALGORITHM preOrder(node)
// INPUT <-- root node
// OUTPUT <-- pre-order output of tree nodes

    OUTPUT <-- node.value

    if node.left is not Null
        preOrder(node.left)

    if node.right is not NULL
        preOrder(node.right)
In-order
ALGORITHM inOrder(node)
// INPUT <-- root node
// OUTPUT <-- in-order output of tree nodes

    if node.left is not NULL
        inOrder(node.left)

    OUTPUT <-- node.value

    if node.right is not NULL
        inOrder(node.right)
Post-order
ALGORITHM postOrder(node)
// INPUT <-- root node
// OUTPUT <-- post-order output of tree nodes

    if node.left is not NULL
        inOrder(node.left)

    if node.right is not NULL
        inOrder(node.right)

    OUTPUT <-- node.value
Notice the similarities between the three different traversals above. The biggest difference between each of the traversals is when you are looking at the root node.

Breadth First
Breadth first traversal iterates through the tree by going through each level of the tree node-by-node. So, given our starting tree one more time:

Sample Tree

Our output using breadth first traversal is now:

Output: A, B, C, D, E, F

Traditionally, breadth first traversal uses a queue (instead of the call stack via recursion) to traverse the width/breadth of the tree. Let’s break down the process.

Given our starting tree shown above, let’s start by putting the root into the queue:

Breadth First Traversal 01

Now that we have one node in our queue, we can dequeue it and use that node in our code.

Breadth First Traversal 02

From our dequeued node A, we can enqueue the left and right child (in that order).

Breadth First Traversal 03

This leaves us with B as the new front of our queue. We can then repeat the process we did with A: Dequeue the front node, enqueue that node’s left and right nodes, and move to the next new front of the queue.

Breadth First Traversal 04

Now our front is C, so we repeat the dequeue + enqueue children process:

Breadth First Traversal 05

And we continue onwards. When we reach a node that doesn’t have any children, we just dequeue it without any further enqueue.

Breadth First Traversal 06 Breadth First Traversal 07 Breadth First Traversal 08

Pseudocode
Here is the pseudocode, utilizing a built-in queue to implement a breadth first traversal.

ALGORITHM breadthFirst(root)
// INPUT  <-- root node
// OUTPUT <-- front node of queue to console

  Queue breadth <-- new Queue()
  breadth.enqueue(root)

  while breadth.peek()
    node front = breadth.dequeue()
    OUTPUT <-- front.value

    if front.left is not NULL
      breadth.enqueue(front.left)

    if front.right is not NULL
      breadth.enqueue(front.right)
Binary Trees
In all of our examples, we’ve been using a Binary Tree. Trees can have any number of children per node, but Binary Trees restrict the number of children to two (hence our left and right children).

There is no specific sorting order for a binary tree. Nodes can be added into a binary tree wherever space allows. Here is what a binary tree looks like:

Binary Tree

Adding a node
Because there are no structural rules for where nodes are “supposed to go” in a binary tree, it really doesn’t matter where a new node gets placed.

One strategy for adding a new node to a binary tree is to fill all “child” spots from the top down. To do so, we would leverage the use of breadth first traversal. During the traversal, we find the first node that does not have 2 child nodes, and insert the new node as a child. We fill the child slots from left to right.

In the event you would like to have a node placed in a specific location, you need to reference both the new node to create, and the parent node upon which the child is attached to.

Big O
The Big O time complexity for inserting a new node is O(n). Searching for a specific node will also be O(n). Because of the lack of organizational structure in a Binary Tree, the worst case for most operations will involve traversing the entire tree. If we assume that a tree has n nodes, then in the worst case we will have to look at n items, hence the O(n) complexity.

The Big O space complexity for a node insertion using breadth first insertion will be O(w), where w is the largest width of the tree. For example, in the above tree, w is 4.

A “perfect” binary tree is one where every non-leaf node has exactly two children. The maximum width for a perfect binary tree, is 2^(h-1), where h is the height of the tree. Height can be calculated as log n, where n is the number of nodes.

Binary Search Trees
A Binary Search Tree (BST) is a type of tree that does have some structure attached to it. In a BST, nodes are organized in a manner where all values that are smaller than the root are placed to the left, and all values that are larger than the root are placed to the right.

Here is how we would change our Binary Tree example into a Binary Search Tree:

Binary Search Tree

Searching a BST
Searching a BST can be done quickly, because all you do is compare the node you are searching for against the root of the tree or sub-tree. If the value is smaller, you only traverse the left side. If the value is larger, you only traverse the right side.

Let’s say we are searching 15. We start by comparing the value 15 to the value of the root, 23.

15 < 23, so we traverse the left side of the tree. We then treat 8 as our new “root” to compare against.

15 > 8, so we traverse the right side. 16 is our new root.

15 < 16, so we traverse the left side. And aha! 15 is our new root and also a match with what we were searching for.

Binary Search Tree

The best way to approach a BST search is with a while loop. We cycle through the while loop until we hit a leaf, or until we reach a match with what we’re searching for.

Big O
The Big O time complexity of a Binary Search Tree’s insertion and search operations is O(h), or O(height). In the worst case, we will have to search all the way down to a leaf, which will require searching through as many nodes as the tree is tall. In a balanced (or “perfect”) tree, the height of the tree is log(n). In an unbalanced tree, the worst case height of the tree is n.

The Big O space complexity of a BST search would be O(1). During a search, we are not allocating any additional space.