# Linked Lists
401 class 5 readings
## Linear data structures
  - One characteristic of linked lists is that they are linear data structures, which means that there is a sequence and an order to how they are constructed and traversed. 
  - must be went through sequentially
  - Similarly, when we use arrays in our code, we’re implementing a linear data structure! It can be helpful to think of arrays and linked lists as being similar in the way that we sequence data.
  - The fundamental difference between arrays and linked lists is that arrays are static data structures, while linked lists are dynamic data structures.

## Parts of a linked list
  -  A linked list is made up of a series of nodes, which are the elements of the list.
  - The starting point of the list is a reference to the first node, which is referred to as the head.
  - The end of the list isn’t a node, but rather a node that points to null, or an empty value.
  - A single node is also pretty simple. It has just two parts: data, or the information that the node contains, and a reference to the next node.

## Lists for all shapes and sizes
  - Singly linked lists are the simplest type of linked list, based solely on the fact that they only go in one direction. 
  - But just as a node can reference its subsequent neighbor node
    - it can also have a reference pointer to its preceding node, too! This is what we call a doubly linked list
    - there are two references contained within each node:
      - a reference to the next node,
      - as well as the previous node. 
    - This can be helpful if we wanted to be able to traverse our data structure not just in a single track or direction, but also backwards, too.
  - A circular linked list is a little odd in that it doesn’t end with a node pointing to a null value. Instead, it has a node that acts as the tail of the list (rather than the conventional head node)