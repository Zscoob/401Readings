# Stacks and Queues
.NET 401d1 Reading for Class 10

## What is a Stack?
  -  A stack is a data structure that consists of Nodes.
  - Each Node in a stack references the next Node but not the  previous

### Terminology 
The following is a list of core terminology dealing with stacks
  - Push = Nodes or items that are put into the stack are pushed
  - Pop = Nodes or items that are removed from the stack are popped.
    - When you attempt to pop an empty stack an exception will be raised.
  - Top = the top of the stack
  - Peek = Allows you to view the value of the top Node in the stack. 
    - When you attempt to peek an empty stack an  exception will be raised.
  - IsEmpty = returns true when the stack is empty, otherwise returns false

### Stacks follow the concepts:

#### FILO
  - First In Last Out
    - This means that the first item added in the stack will be the last item popped out of the stack.

#### LIFO
  - Last In First Out
    - This means that the last item added to the stack will be the first item popped out of the stack.

### Stack Visualization
  - An example of a stack follows:
         
  Push [_|___}          op [_|___}  
    \                      /
   __|____________________|__
  |     |   Value: "blue"    |
  |   4 |    Next: 3         |   <--Top
  |_____|____________________|
  |     |   Value: "green"   |
  |   3 |    Next: 2         |
  |_____|____________________|
  |     |   Value: "orange"  |
  |   2 |    Next: 1         |
  |_____|____________________|
  |     |   Value: "red"     |
  |   1 |    Next: null      |
  |_____|____________________|

  - The topmost item is denoted as the "top"
  - when you push something to the stack it becomes the new "top"
  When you pop something from the stack you pop the current "top" and set the next "top" as "top.next"

### Push O(1)
  - Pushing a Node onto a stack will always be an O(1) opperation.
  - Takes the same amount of time no matter how many Nodes (n) you have in the stack.
  - When added a Node you 
    - push it into the stack by assigning it the new top with its next property equal to the original top

    Walkthrough

    - The first thing you should have is the Node you want to add.


    ___________________________
    |     |   Value: "gray"    |
    |   5 |    Next: null      | push:
    |_____|____________________|

    ____________________________
    |     |   Value: "blue"    |
    |   4 |    Next: 3         |   <--Top
    |_____|____________________|
    |     |   Value: "green"   |
    |   3 |    Next: 2         |
    |_____|____________________|
    |     |   Value: "orange"  |
    |   2 |    Next: 1         |
    |_____|____________________|
    |     |   Value: "red"     |
    |   1 |    Next: null      |
    |_____|____________________|

    - Next you assign the "next" property of Node 5 to reference the same Node that top is referencing: Node 4

    ___________________________
    |     |   Value: "gray"    |
    |   5 |    Next: 4         | push:
    |_____|____________________|

    ___________________________
    |     |   Value: "blue"    |
    |   4 |    Next: 3         |   <--Top
    |_____|________________ ___|
    |     |   Value: "green"   |
    |   3 |    Next: 2         |
    |_____|____________________|
    |     |   Value: "orange"  |
    |   2 |    Next: 1         |
    |_____|____________________|
    |     |   Value: "red"     |
    |   1 |    Next: null      |
    |_____|____________________|

    - At this point the new Node is added to your stack. The next thing to do is to reassign the top reference to the newly added Node

    - This is a visualization of a successful push of Node 5 onto the stack.

    - The pseudo code to push is as follows:
      ALOGORITHM push(value)
      // INPUT <-- value to add, wrapped in Node internally
      // OUTPUT <-- none
        node = new Node(value)
        node.next <-- Top
        top <-- Node

### Pop O(1)
  - Popping a Node off a stack is the action of removing a Node from the top
  - When conducting a pop, the top Node will be reassigned to the Node that lives below and the top Node is returned to the user
  - Good practice dictates to check IsEmpty before popping to ensure an exception is not raised. 
    - you can also wrap the call in a try/catch block

  Below is a visual representation of conducting a pop on Node 5 from a stack.

     __________________________
    |     |   Value: "gray"    | 
    |   5 |    Next: 4         |   <--Top
    |_____|____________________|    
    |     |   Value: "blue"    |
    |   4 |    Next: 3         |  
    |_____|____________________|
    |     |   Value: "green"   |
    |   3 |    Next: 2         |
    |_____|____________________|
    |     |   Value: "orange"  |
    |   2 |    Next: 1         |
    |_____|____________________|
    |     |   Value: "red"     |
    |   1 |    Next: null      |
    |_____|____________________|

    - The first step of removing Node 5 is to create a reference named "temp"
      - be sure that "temp" points to the same Node that "top" points to.
    
    ___________________________
    |     |   Value: "gray"    |   <--Top
    |   5 |    Next: 4         |   <--Temp
    |_____|____________________|    
    |     |   Value: "blue"    |
    |   4 |    Next: 3         |  
    |_____|____________________|
    |     |   Value: "green"   |
    |   3 |    Next: 2         |
    |_____|____________________|
    |     |   Value: "orange"  |
    |   2 |    Next: 1         |
    |_____|____________________|
    |     |   Value: "red"     |
    |   1 |    Next: null      |
    |_____|____________________|

  - Once you've created the new reference type you need to reassign "top" to the value that the "next" property is referencing.
  - in the following visual the "next" property is pointing to Node 4.
  - we need to reassign Node 4 to be the "top"

     __________________________
    |     |   Value: "gray"    | 
    |   5 |    Next: 4         | <--temp  
    |_____|____________________| 

    ___________________________
    |     |   Value: "blue"    |
    |   4 |    Next: 3         | <--top 
    |_____|____________________|
    |     |   Value: "green"   |
    |   3 |    Next: 2         |
    |_____|____________________|
    |     |   Value: "orange"  |
    |   2 |    Next: 1         |
    |_____|____________________|
    |     |   Value: "red"     |
    |   1 |    Next: null      |
    |_____|____________________| 

    - Now we can remove Node 5 without affecting the rest of the stack.
    - Be sure to clear out the Next property in the current "temp" reference
      - that ensures that there are no stray references to Node 4 that can interact with the stack.
      - Also allows correct removal.

     __________________________
    |     |   Value: "gray"    | 
    |   5 |    Next: null      | <--temp  
    |_____|____________________| 

    ___________________________
    |     |   Value: "blue"    |
    |   4 |    Next: 3         | <--top 
    |_____|____________________|
    |     |   Value: "green"   |
    |   3 |    Next: 2         |
    |_____|____________________|
    |     |   Value: "orange"  |
    |   2 |    Next: 1         |
    |_____|____________________|
    |     |   Value: "red"     |
    |   1 |    Next: null      |
    |_____|____________________| 

    - next, and finally we return the value of the temp Node that was popped off.

    - pseudo code for pop is as follows

      ALGORITHM pop()
      // INPUT <-- No input
      // OUTPUT <-- value of top Node in stack
      // EXCEPTION if stack is empty

        Node temp <-- top
        top <-- top.next
        temp.next <-- null
         return temp.value

### Peek O(1)
  - When conducting a peek you will only be inspecting the top Node of the stack
  - good practice is checking isEmpty before conducting a peek to ensure an exception is not raised.
    - can also wrap the call in a try/catch block

    pseudocode for peek follows:

    ALGORITHM peek()
    // INPUT <-- none
    // OUTPUT <-- value of top Node in stack
    // EXCEPTION if stack is empty

      return top.value

  - Do not reassign the Next property when we use peek because we cant to keep the reference to the next Node in the stack.
    - This allows the top to say the top until we pop

### IsEmpty O(1)

  - Pseudocode for isEmpty :

  ALGORITHM isEmpty()
  // INPUT <-- none
  // OUTPUT <-- boolean

  return top = NULL

### What is a Queue

Common terminology for a queue is as follows
  - Enqueue = Nodes or items that are added to the queue
  - Dequeue = Nodes or items that are removed from the queue
    - If called when the queue is empty an exception will be raised
  - Front = This is the front/first Node of the queue
  - Read = This is the rear/last Node of the queue
  - Peek = When you peek you view the value of the front Node in the queue.
    - if called when queue is empty an exception will be raised
  - IsEmpty = returns true when queue is empty, otherwise returns false

### FIFO
 - First In First Out
  - This means the first item in the queue will be the first item out of the queue

### LILO
  - Last In Last Out
    - This means the last item in the queue will be the last item out of the queue

### Queue Visualization\

  - The visualization of a Queue is as follows:

         Enqueue                                             Dequeue
             \                                                 /
      ________|_______________________________________________|________
      |               |               |               |               |
      |       4       |       3       |       2       |       1       |
      |_______________|_______________|_______________|_______________|
      |               |               |               |               |
      |   value =     |    value =    |    value =    |   value =     |
      |   "blue"      |    "green"    |   "orange"    |    "red"      |
      |               |               |               |               |
      |  Next = null  |   Next = 4    |   Next = 3    |   Next = 2    |
      |_______________|_______________|_______________|_______________|
              |                                              |
             /                                                \
           Rear                                              Front

### Enqueue O(1)

  - When you add an item to the queue you use the "enqueue" action. this is done with an O(1) operation in time because it does not matter how many other items live in the queue (n)
    - takes the same amount of time to perform the operation

  Walkthrough for the process of adding a node to a queue:


    Enqueue
       |                                                       
   ____|_________     ________________________________________________________________
  |              |    |               |               |               |               |
  |      5       |    |       4       |       3       |       2       |       1       |
  |______________|    |_______________|_______________|_______________|_______________|
  |              |    |               |               |               |               |
  |   value =    |    |   value =     |    value =    |    value =    |   value =     |
  |   "gray"     |    |   "blue"      |    "green"    |   "orange"    |    "red"      |
  |              |    |               |               |               |               |
  | Next = null  |    |  Next = null  |   Next = 4    |   Next = 3    |   Next = 2    |
  |______________|    |_______________|_______________|_______________|_______________|
                                |                                              |
                               /                                                \
                             Rear                                              Front

  - first step is to change the "next" property of Node 4 to point to the Node we are adding.
    - reassign Node 4's .next to Node 5.
    - only way to access Node 4 is through the "rear" reference
    - rear.next must be changed to Node 5 in this case.

    Enqueue
       |                                                       
   ____|_________     ________________________________________________________________
  |              |    |               |               |               |               |
  |      5       |    |       4       |       3       |       2       |       1       |
  |______________|    |_______________|_______________|_______________|_______________|
  |              |    |               |               |               |               |
  |   value =    |    |   value =     |    value =    |    value =    |   value =     |
  |   "gray"     |    |   "blue"      |    "green"    |   "orange"    |    "red"      |
  |              |    |               |               |               |               |
  | Next = null  |    |   Next = 5    |   Next = 4    |   Next = 3    |   Next = 2    |
  |______________|    |_______________|_______________|_______________|_______________|
                                |                                              |
                               /                                                \
                             Rear                                              Front

  - After the "next" property has been set, we can reassign the rear reference to Node 5
    - by doing so we can keep a reference of were rear is
    - can continue to enqueue Nodes into the queue as needed.

//    Enqueue
//       |                                                       
//   ____|__________________________________________________________________________
//  |              |               |               |               |               |
//  |      5       |       4       |       3       |       2       |       1       |
//  |______________|_______________|_______________|_______________|_______________|
//  |              |               |               |               |               |
//  |   value =    |   value =     |    value =    |    value =    |   value =     |
//  |   "gray"     |   "blue"      |    "green"    |   "orange"    |    "red"      |
//  |              |               |               |               |               |
//  | Next = null  |   Next = 5    |   Next = 4    |   Next = 3    |   Next = 2    |
//  |______________|_______________|_______________|_______________|_______________|
//        |                                                                |
//        /                                                                 \
//     Rear                                                               Front

  - this is a depiction of successfully adding a Node to a queue by using the enqueue action

  The pseudocode for the enqueue method is as follows:
  
    ALGORITHM enqueue(value)
    // INPUT <-- value to add to queue (will be wrapped in Node internally)
    // OUTPUT <-- none
       node = new Node(value)
       rear.next <-- node
       rear <-- node


### Dequeue O(1)

  - You use the dequeue action to remove an item from the queue.
  - This is done with an O(1) operation in time because it doesnt matter how many other items are in the queue.
  - you are always removing the "front" node of the queue.

  -good practice dictates you check isEmpty before conducting dequeue to avoid any exceptions being raised.
    - you can also wrap the call in a try/catch block

  - Walkthrough for removing a Node from a queue is as follows:

  - The first thing we need to do is create a temporary reference type named "temp"
  - temp needs to point to the same Node that the "front" is pointing to
    - temp would point to Node 1

                                                                      Dequeue 
                                                                         |
   ______________________________________________________________________|_______
  |              |               |               |               |               |
  |      5       |       4       |       3       |       2       |       1       |
  |______________|_______________|_______________|_______________|_______________|
  |              |               |               |               |               |
  |   value =    |   value =     |    value =    |    value =    |   value =     |
  |   "gray"     |   "blue"      |    "green"    |   "orange"    |    "red"      |
  |              |               |               |               |               |
  | Next = null  |   Next = 5    |   Next = 4    |   Next = 3    |   Next = 2    | 
  |______________|_______________|_______________|_______________|_______________|
        |                                                                |    |
        /                                                                 \    \
     Rear                                                               Front   \
                                                                                Temp

  - next we need to reassign "front" to the "next" value that the Node "front" is referencing.
  - In the walkthroughs case, this would be Node 2

                                                                          Dequeue 
                                                                             |
   ______________________________________________________________    ________|_______
  |              |               |               |               |  |               |
  |      5       |       4       |       3       |       2       |  |       1       |
  |______________|_______________|_______________|_______________|  |_______________|
  |              |               |               |               |  |               |
  |   value =    |   value =     |    value =    |    value =    |  |   value =     |
  |   "gray"     |   "blue"      |    "green"    |   "orange"    |  |    "red"      |
  |              |               |               |               |  |               |
  | Next = null  |   Next = 5    |   Next = 4    |   Next = 3    |  |   Next = 2    | 
  |______________|_______________|_______________|_______________|  |_______________|
        |                                                 |                   |
        /                                                  \                   \
     Rear                                                 Front                 \
                                                                                Temp

  - Now that we have moved "front" to the second Node in line we can reassign the "next" property on the "temp" Node to null
  - We do this to ensure there are no unnecessary references interacting with our code.

                                                                          Dequeue 
                                                                             |
   ______________________________________________________________    ________|_______
  |              |               |               |               |  |               |
  |      5       |       4       |       3       |       2       |  |       1       |
  |______________|_______________|_______________|_______________|  |_______________|
  |              |               |               |               |  |               |
  |   value =    |   value =     |    value =    |    value =    |  |   value =     |
  |   "gray"     |   "blue"      |    "green"    |   "orange"    |  |    "red"      |
  |              |               |               |               |  |               |
  | Next = null  |   Next = 5    |   Next = 4    |   Next = 3    |  |  Next = null  | 
  |______________|_______________|_______________|_______________|  |_______________|
        |                                                 |                   |
        /                                                  \                   \
     Rear                                                 Front                 \
                                                                                Temp 

  - last, we return the value of the temp Node that was removed.
  - This completes the depiction of successfully using the dequeue action.

  The pseudocode is as follows for dequeue:

     ALGORITHM dequeue()
    // INPUT <-- none
    // OUTPUT <-- value of the removed Node
    // EXCEPTION if queue is empty

       Node temp <-- front
       front <-- front.next
       temp.next <-- null

       return temp.value


### Peek O(1)

  - When conducting a peek you will only be inspecting the front Node of the queue
  - good practice dictates to check isEmpty before using a peek to ensure expections are not raised.
    - can also wrap the call in a try/catch block

    pseudocode for peek is as follows

    ALGORITHM peek()
    // INPUT <-- none
    // OUTPUT <-- value of the front Node in Queue
    // EXCEPTION if Queue is empty

       return front.value

  - do noy reassign the next property when peek is used because we want to keep the reference to the next Node in the queue
  - This allows the front to stay in front until we decide to dequeue

### IsEmpty O(1)

  - the pseudocode for isEmpty is as follows:

    ALGORITHM isEmpty()
    // INPUT <-- none
    // OUTPUT <-- boolean

    return front = NULL