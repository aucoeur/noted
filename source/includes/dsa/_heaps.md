# Heaps
> Min Heap

```markdown
           1 <- smallest
         /   \
        3      2
       / \    /
      4   6  5
```

```python
import heapq

class MinHeap:
    def __init__(self, minheap): # minheap is the list that we can to convert to a heap
        heapq.heapify(minheap) # Use the heapify function to convert list to a heap
        self.minheap = minheap

    def insert(self, key):
        heapq.heappush(self.minheap, key) # Insert key into the heap (heapq automatically maintains the heap property)

    def getMin(self):
        return self.minheap[0] # Returns the smallest element of the heap in O(1) time

    def removeMin(self):
        heapq.heappop(self.minheap) # The heappop function removes the smallest element in the heap

    def printHeap(self):
        print(self.minheap) # Prints the heap
```

Heaps are special tree based data structures that satisfy two properties:

1.  Ordered in a specific way:
    
    |heaps|`root` | children |
    |-|:-|-|
    | Min |  smallest element | `parent <=` sum of `children`  |
    | Max | largest element | `parent >=` sum of `children`  |

2. Are **complete binary trees**     

    |||
    |-|-|   
    | Binary | At most, two children: `left` and `right`  |    
    | Complete | Fills each level entirely, except the last level, which **must** be `left`  |

Heaps are useful when for getting the largest or smallest elements, and in situations where you don’t care about fast lookup, delete, or search.


### Terminology

> Max Heap

```markdown
           6 <- largest
         /   \
        5     3
       / \    /
      4   2  1
```

```python
class MaxHeap:
    def __init__(self):
        # Initialize a heap using list
        self.heap = []

    def getParentPosition(self, i):
        # The parent is located at floor((i-1)/2)
        return int((i-1)/2)

    def getLeftChildPosition(self, i):
        # The left child is located at 2 * i + 1
        return 2*i+1

    def getRightChildPosition(self, i):
        # The right child is located at 2 * i + 2
        return 2*i+2

    def hasParent(self, i):
        # This function checks if the given node has a parent or not
        return self.getParentPosition(i) < len(self.heap)

    def hasLeftChild(self, i):
        # This function checks if the given node has a left child or not
        return self.getLeftChildPosition(i) < len(self.heap)

    def hasRightChild(self, i):
        # This function checks if the given node has a right child or not
        return self.getRightChildPosition(i) < len(self.heap)

    def insert(self, key):
        self.heap.append(key) # Adds the key to the end of the list
        self.heapify(len(self.heap) - 1) # Re-arranges the heap to maintain the heap property

    def getMax(self):
        return self.heap[0] # Returns the largest value in the heap in O(1) time.

    def heapify(self, i):
        while(self.hasParent(i) and self.heap[i] > self.heap[self.getParentPosition(i)]): # Loops until it reaches a leaf node
            self.heap[i], self.heap[self.getParentPosition(i)] = self.heap[self.getParentPosition(i)], self.heap[i] # Swap the values
            i = self.getParentPosition(i) # Resets the new position

    def printHeap(self):
        print(self.heap) # Prints the heap
```
|||
|-|-
| Bubble down | moving an element down by swapping with one of its children in proper position |
| Bubble up | moving an element down by swapping with one of its children in proper position |
| Priority Queue |  queue data structure with associated priority value, higher priority is dequeue before lower priority, if same, then dequeued based on location in array
### Heap Operations

### Building

This complex data structure can be represented using an array. Often implemented as arrays because they are super efficient ways of representing *priority queues*.

*Binary heaps* are super efficient for implementing priority queues because it’s very easy to know and retrieve/remove the element with the highest priority: *it will always be the **root** node*!

Building a heap only takes `O(n)` time, so you can potentially optimize a solution by building a heap from a list instead of running *insertion* `n` times to create the heap.

By property of it being a complete binary tree, we can see how the parent-child relationships are maintained in the array using these formulas: 

||||
|-|:-:|-|
| Parent | `(n - 1) // 2` | `n ==` index of current node
| `Left`  | `2i + 1` |  `i ==` index of parent node
| `Right`  | `2i + 2` |

### Insertion
When growing a heap, we can only ever add a node to the `left-most` available node, at `lowest possible level`.

If necessary to follow both rules of shape and order, we `swap` the two nodes that are out of order

### Removal
When deleting or removing an element, most heaps are usually concerned with removing the `root` node.

In order to maintain rules of shape and order: 

1. For shape, remove the `right-most` node at the `lowest level`, make it `root`
2. Then, for order, compare with child nodes and `swap`
3. Continue bubbling down (step 2) until no longer violating `heap order property`

## Time Complexity
In the worst case scenario, the swapping procedure for insertions and deletions will move the element through the height of the heap. 

Because heaps are binary trees that are guaranteed to be as complete as possible, the number of levels in the heap will be `log n`.

||Best | Worst |
|-:|:-:|:-:|
Reading root node |	O(1) | O(1)
Insertion |	O(log n) | O(log n)
Deletion |	O(log n) | O(log n)
Build heap | O(n) (from list) | O(n log n) (inserting into empty heap)

## References
- [https://medium.com/basecs/learning-to-love-heaps-cef2b273a238](https://medium.com/basecs/learning-to-love-heaps-cef2b273a238) 
- [https://www.geeksforgeeks.org/binary-heap/](https://www.geeksforgeeks.org/binary-heap/)
- [https://www.section.io/engineering-education/heap-data-structure-python/](https://www.section.io/engineering-education/heap-data-structure-python/)
