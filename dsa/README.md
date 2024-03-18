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

- As I tried to tease in other's essays: engineering is more than tooling, it is conceptual knowledge. Having a base familiarity with that(DSA), I'm sure that I can glimp different opportunities and be a better engineer

- Creativity it's not about being a creative guy, you need to be exposed to different use cases and theory knowledge to be able to  play around with different topics and connect them, the same could occur with any field, including computing

- It's fun!

In the end, I'm just studying that to be approved for a job interview is pure bullshit, but as I have free time to spend on it, I think that is a good opportunity.

## Algorithms


## Time Complexity

![complexity](./complexity.png)

### **Rules**

- Input size is always bigger than zero
- functions do more work for more input
- drop all constants

    - Ignore the constants will not affect the final result, for this reason, to simplify, we will adopt this practice of dropping them

- Care about the big values

## Dynamic Programming

The method used to solve complex problems chunking them into the smallest subproblems, storing these values and using them to solve further problems.

So, the solution of the whole problem is a result of the solution of the subproblems.

- Establish a mathematician relationship between the subproblems and the main problem and use it to solve the whole problem

![](./DP.jpeg)

O exemplo acima é um bem simples sobre como o DP funciona, aqui,estamos tentando encontrar o valor máximo de possibilidades para chegar ao topo(N) dando 1 ou 2 saltos.

Podemos observar alguns pontos importantes:

- Caso o numero de degraus seja 4, podemos observar que todas as possibilidades de chegar ao 4 foram formadas até a altura 3 dessa árvore.
- A complexidade para resolver esse problema deve contemplar todas as possibilidades que estamos tentando formar. Fazendo algumas relações básicas, é simples de observar que isso é 2^n, ou seja, exponencial.
- Agora a parte mais diícil, nos convencermos de que a solução para N=3, é a soma das soluções para N=1 + N=2, ou seja, 3 = 1 + 2.
    - Para N=4, a solução é a soma das soluções para N=2 + N=3, ou seja, 5 = 2 + 3
    - Para N=5, a solução é a soma das soluções para N=3 + N=4, ou seja, 8 = 3 + 5
    - E assim por diante

Sabendo destes pequenos detalhes, podemos aplicar para outros problemas de DP. Entre eles, é comum observar:

- Problemas de maximização ou minimização


Por ser um problema que pode ser resolvido usando recursão, é extremamente imporante pensar em como podemos parar a recursão para ela não se tornar infinita.

Nesse caso, seria quanto o valor de N for 0 ou 1, que são os casos base. E para N < 0.

### Store(Memoization != memorization) also known as Top-Down

A technique to speed up the algorithm by storing the results that were already calculated and using them on future calculations.

The term "memoization" comes from the **Latin** word "memorandum" which means "to remember", and it's confused with "memorization" which means "to memorize".

But both are very similar.

### Bottom-up

Similar to the top-down approach, but instead of using recursion, we use a loop to solve the problem. The tricky is to find the mathematical relationship between the subproblems, but the performance and the memory usage are better.

## Linked Lists

### Introduction

Try to fix a lack of arrays, that is: **It was designed to be always the right size**

**Components**

- Node: Have two pieces of information
    - pointer, that we call **next**
    - **data**, information that we decided to save on it
- The last node points to **NULL**
- The first component of a linked list is called HEAD

## Nodes and Size

## Boundaries Conditions

- Empty data structure
- A single element in the DS
- Adding/Removing at the beginning of DS
- Adding/Removing at the end of the DS
- Working in the middle

# Trees

## Binary Search Trees(BSTs)

**Obs:** the difference between a binary tree and a binary search tree is that the binary search follows some rules, that are explained ahead.

Binary means two, so nodes in a binary tree have a maximum of **two children**

Avg. case: search, insert, delete -> O(log(N))

**BST Property:**

For each node u, any lode l in **its left subtree** satisfies l.key <= u.key, and any node r in its **right subtree** satisfies r.key >= u.key

![bst](./bst.png)

## Tree traversals
Unlike linear data structures like arrays, linked lists, queues and stacks, which have only one logical way to traverse them, trees can be traversed in different ways.

A traversal algorithm aims to "process" **each node in the tree exactly once**

We have 2 types of traversal algorithms in tress, that are: **Depth First Search or DFS** & Level Order Traversal or **Breadth First Search or BFS**

DFS(As the name suggests, start from the depth point of the tree):

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

## Perils and Advantages

**Advantages:**

- Tree offers Efficient search depending on the type of tree, with an average **search** time of O(log(N))
Trees provide a hierarchical representation of data, making it easy to organize and navigate large amounts of information
- The recursive nature of trees makes them easy to **traverse and manipulate**

**Perils:**

- Unbalanced trees, meaning that the height of the tree is skewed towards one side, which can lead to inefficient search times
- Trees demand more memory space
- The implementation and manipulation of trees can be complex and require a good understanding of the underlying algorithms


## Resources

- [Binary Search Trees: Samuel's tutorial](https://www.youtube.com/watch?v=0woI8l0ZWmA)
- [All you need to know about tree data structure](https://www.freecodecamp.org/news/all-you-need-to-know-about-tree-data-structures-bceacb85490c/)

# Arrays

Arrays are one of the basic structures in computer science and one of the most used. Here I will try to cover some of the most important techniques and algorithms that we can use to solve problems.

## Sliding Window

### Types

- Fixed Size
- Dynamic Size

## Two Pointers

## Sorting

## Circular array

This is a simple approach to solving problems that involve circular arrays.

It's important to notice the following math operation around the module:

- 1 % 5 = 1
- 2 % 3 = 2

If the divided is smaller than the divisor, the result will be the divided.

So, if we have a circular array with 5 elements, and we want to access the 6th element, we can do the following:

```go
n := len(arr)
for i := 0; i < n; i++ {
    fmt.Println(arr[i % n])
}
```


## Resources
- [Array CheatSheet](https://www.techinterviewhandbook.org/algorithms/array/)

- [Solve subarray problems FASTER (using sliding window)](https://www.youtube.com/watch?v=GcW4mgmgSbw)

# Difficulty to learn x Recompensation

![](./learn-table.png)

# Algorithms a visual introduction

[Top 7 Algorithms](https://www.youtube.com/watch?v=kp3fCihUXEg)

These include:

- Binary Search: **Try to find a specific value in a sorted array**
- Depth-First Search:
- Breadth-First Search
- Merge Sort
- Quick Sort
- Greedy

# General Important Resources

- [Data Structures](https://www.youtube.com/playlist?list=PLpPXw4zFa0uKKhaSz87IowJnOTzh9tiBk)
- [DSA Handbook](https://www.techinterviewhandbook.org/)
- [Leetcode questions with resolution 75](https://docs.google.com/spreadsheets/d/1A2PaQKcdwO_lwxz9bAnxXnIQayCouZP6d-ENrBz_NXc/edit#gid=0)
