# Principal Topics

- Time Complexity
- Arrays
- Trees
- Heap
- Stack
- Graphs
- Hash Maps

## Why learn this shi*t?

- I don't give a fuck about that in the University, this doesn't mean that I'm completely dumb about the topic, but, I'm not even able to feel comfortable weaving a discussion of it

- As I tried to tease in other's essays: engineering is more than tooling, it is conceptual knowledge. Having a base familiarity with that(DSA), I'm sure that I can glimp different opportunities and be a better a engineer

- Creativity it's not about be a creative guy, you need to be exposed to different use cases and theory knowledge to be able to  play around different topics and connect them, the same could occour with any field, including computing

- It's fun!

In the end, just studying that to be approved in a job interview is pure bullshit, but as I have free time to spend on it, I think that is a good opportunity.

## Algorithms


## Time Complexity

![complexity](./complexity.png)

### **Rules**

- Input size is always big than zero
- functions do more work for more input
- drop all constants
    
    - Ignore the constants will not affect a lot the final result, for this reason, to simplify, we will adopt this practice to drop them

- Care about the big values

## Linked Lists

### Introduction

Try to fix a lack on arrays, that is: **It was designed to be always the right size**

**Components**

- Node: Have two pieces of information
    - pointer, that we call **next**
    - **data**, information that we decided to save on it
- The last node point to **NULL**
- The first component of a linked list is called HEAD

## Nodes and Size

## Boundaries Conditions

- Empty data structure
- Single element in the DS
- Adding/Removing in the beginning of DS
- Adding/Removing in the end of the DS
- Working in the middle

# Trees

## Binary Search Trees(BSTs)

Binary means two, so node in a binary tree have a maximum of **two children**

Avg. case: search, insert, delete -> O(log(N))

**BST Property:**

For each node u, any lode l in **its left subtree** satisfies l.key <= u.key, any node r in its **right subtree** satisfies r.key >= u.key

![bst](./bst.png)

## Tree traversals
Unlike linear data structures like, arrays, linked list, queues and stacks, which have only one logical way to traverse them, trees can be traversed in different ways.

A traversal algorithm aims to "process" **each node in the tree exactly once**

We have 2 types of traversals algorithms in tress, that are: Depth First Search or DFS & Level Order Traversal or Breadth First Search or BFS

DFS(As the name suggest, start from the depth point of the tree):

Obs: The results presented, will be on top of the above image tree.

**inorder:**
[1,2 3, 4, 5, 6]

```python
def inorder(u):
    if u:
        inorder(u.left)
        print(u.key)
        inorder(u.right)
```

**preorder:**
[1,2,3, 5, 4, 6]

```python
def preorder(u):
        if u is not None:
            print(u.key)
            preorder(u.left)
            preorder(u.right)
```

**postorder:**
[1,2,4,6,5,3]

```python
    def postorder(u):
        if us is not None:
            postorder(u.left)
            postorder(u.right)
            print(u.key)
```

## Minimum/Maximum O(h):

Minimum/Max: The logic behind those algorithms is the following: The min value will be in the last element of the left and the same for the Maximum, but to the right

## Search O(h)
TODO
## Insertion
TODO
## Deletion
TODO

## Resources

[Binary Search Trees: Samuel's tutorial](https://www.youtube.com/watch?v=0woI8l0ZWmA)

# Important Resources

- [Data Structures](https://www.youtube.com/playlist?list=PLpPXw4zFa0uKKhaSz87IowJnOTzh9tiBk)
- [DSA Handbook](https://www.techinterviewhandbook.org/)
