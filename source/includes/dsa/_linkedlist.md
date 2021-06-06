# Data Structures & Algorithms

Werkkk

# LinkedLists
> Definition of a Node
 
```python
class Node:
    def __init__(self, val=0, next=None, previous=None):
        self.val = val
        self.next = next
        self.previous = previous # if Doubly Linked
```
```markdown
- Node holds two (or three) things:
    - the data
    - pointer to the next node
    - pointer to previous node (if doubly linked list)
```
> Definition of Singly LinkedList

```python
class LinkedList:
    def __init__(self, items=None):
        self.head = None
        self.tail = None

        if items:
            for item in items:
                self.append(item)

    def prepend(self, val):
        node = Node(val)
        if not self.head: # empty LL -> node set to head & tail 
            self.tail = node
        else: 
            node.next = self.head
        self.head = node 
    
    def append(self, val):
        node = Node(val)
        if not self.tail: # empty LL -> node set to head & tail 
            self.head = node
        else:
            self.tail.next = node 
        self.tail = node
    # ...   
```
```markdown
- LinkedList init with head & tail node, if exists
- If init with array, create the list with append()
```

The most common variants of linked lists are:

| |`next`|`previous`| if Circular:
|-|:-:|:-:|-|
| Singly Linked | ☑️ | | `tail.next` -> `head` 
| Doubly Linked | ☑️ | ☑️ | `tail.next` -> `head` <br /> `head.previous` -> `tail`

### Terminology
|||
|-|-
| node | position in LinkedList containing the `value` of whatever is stored at the position and at least one reference to another node (`next`)
| head | node at `beginning` of list
| tail | node at `end` of the list
| sentinel | a `dummy` node, typically placed at the head or end of the list to help make operations simpler (e.g., delete) or to indicate the termination of list


### Time & Space Complexity
||Best | Worst |
|-:|:-:|:-:|
*Occurs when `node` at* | `head` | `last`
Accessing / Search | O(1) |  O(N)
Inserting at `head` | O(1) | O(N) 
Deleting at `head` | O(1) | O(N) 

## Strategy: Dummy Head
> Delete

```python
    def delete(self, val):
        dummy = Node("sentinel")
        dummy.next = self
        placeholder = dummy
        current = self
        while current:
            if current.val == val:
                placeholder.next = current.next
                return dummy.next
            placeholder = current
            current = current.next
        return dummy.next         
```

```markdown
1. Create `dummy` node
    - `dummy.next` points to `head`
2. Set `placeholder` to `dummy` and `current` to `head`
3. Iterate through `current` by:
    - Set `placeholder` to `current` node
    - Set `current` to `current.next`
5. If we find `val`:
    - set `placeholder.next` to node at `current.next`
6. Return `dummy.next`

```
Typically saves you creating *special edge condition logic* in order to operate on the head of a linked list with some algorithms. 

<aside class="notice">
Thanks to `dummy head`, deleting the head of the original list is the same as deleting any other element in the list.
</aside>

## Strategy:  Multi Pass
> Get Length of List

```python
    def __len__(self):
        current = self
        length = 0
        while current:
            length += 1
            current = current.next
        return length
```
```markdown
1. Iterate through list
2. Increment `length`
3. Set `current` to `current.next`
```
Most computations on a list will require O(N) time complexity, so a simple but very useful technique is to pass through the list a constant number of times to calculate some summary of the list that will simplify your algorithm.

The looping is implicitly done for you by the execution of your program, namely every successive call to your recursive function places some data on your `call stack` and then goes to the next function.

## Note on Recursion vs. Iteration:
> Recursive Length of List

```python
    def get_length_recursive(self):
        linklist = self
        if not linklist: 
            return 0 # A None path has 0 length
        return get_length_recursive(linklist.next) + 1 
        # The length at this node is 1 + length of rest of list
```

```markdown
1. Base Case: return `0` if `None`
2. Recurse through call stack, 
adding `+1` to previous val of `length` on call stack
```
<aside class="notice"> 
Recursive algorithms are often very easy to write with linked lists because the list is structured recursively.
</aside>
<aside class="warning">
Major drawback of recursive algorithm. For most situations, this means you are paying a storage penalty equal to the size of the list.
</aside>

## Strategy: Two Pointer

> Two-Pointer Solution for Detecting Cycle

```python
def has_cycle(ll):
    if not ll or not ll.next:
        return False

    slow = ll
    fast = ll.next

    while fast and fast.next:
        
        if fast == slow or fast.next == slow:
            return True
        fast = fast.next.next # Because it's fast
        slow = slow.next
    return False
```
```markdown
Similar to a race track,   
the faster pointer will eventually cross/lap the slower pointer,   
whereas if no cycle, they will never cross paths  
```

Very useful technique for dealing with linked lists involves iterating through the list with 2 or more pointers. 

The differences between how the pointers iterate can be used to make calculations on the list more efficient.

| | Time | Space 
|-|-|-
| Using Hashmap | O(n) | O(n)
| Two Pointer | O(n) | O(1)

### Explanation:
The first thing that most people think of is to use another data structure to store nodes that we have already seen as we traverse the list. Then as we move through the nodes of the list we check to see if we have already stored the current node in our auxiliary data structure and if we have then we have found a cycle. The typical data structure to choose here is a hash map because it offers constant time insertion and lookup. 

We can get rid of the extra auxiliary data structure by utilizing only one additional pointer. We can then use the two pointers to iterate through the list at two different speeds. The motivation being that if there is a cycle, then the list can be thought of as a circle (at least the part of the list past the self-intersection). Similar to a race track, the faster pointer must eventually cross paths with the slower pointer, whereas if there is not a cycle they will never cross paths.
