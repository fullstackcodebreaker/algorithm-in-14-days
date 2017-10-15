# Tree

Trees are one of the most commonly used data structures in web development. This statement holds true for both developers and users. Every web developer who has written HTML and loaded it into a web browser has created a tree, which is referred to as the Document Object Model \(DOM\). Every user of the Internet who has, in turn, consumed information on the Internet has received it in the form of a tree—the DOM. 

Now, here's the climax: The article that you are reading at this moment is rendered in your browser as a tree! The paragraph that you are reading is represented as text in a`<p>`element; the`<p>`element is nested inside a`<body>`element; and the`<body>`element is nested inside an`<html>`element. 

The nesting of data is similar to a family tree. The`<html>`element is a parent, the`<body>`element is a child, and the`<p>`element is a child of the`<body>`element. If this analogy of a tree seems useful to you, then you will find comfort in knowing that more analogies will be used during our implementation of a tree. 

In this article, we will create a tree using two different methods of tree traversal: Depth-First Search \(DFS\) and Breadth-First Search \(BFS\). \(If the word traversal is unfamiliar to you, consider it to mean visiting every node of the tree.\) Both of these types of traversals highlight different ways of interacting with a tree; both travels, moreover, incorporate the use of data structures that we've covered in this series. DFS uses a stack and BFS uses a queue to visit nodes. That's cool!

## Tree \(Depth-First Search and Breadth-First Search\)

In computer science, a tree is a data structure that simulates hierarchical data with nodes. Each node of a tree holds its own data and pointers to other nodes.

The terminology of nodes and pointers may be new to some readers, so let's describe them further with an analogy. Let's compare a tree to an organizational chart. The chart has a top-level position \(root node\), such as CEO. Directly underneath this position are other positions, such as vice president \(VP\). 

To represent this relationship, we use arrows that point from the CEO to a VP. A position, such as the CEO, is a node; the relationship we create from a CEO to a VP is a pointer. To create more relationships in our organizational chart, we just repeat this process—we have a node point to another node. 

On a conceptual level, I hope that nodes and pointers make sense. On a practical level, we can benefit from using a more technical example. Let's consider the DOM. A DOM has an`<html>`element as its top-level position \(root node\). This node points to a`<head>`element and a`<body>`element. This process is repeated for all nodes in the DOM. 

One of the beauties of this design is the ability to nest nodes: a`<ul>`element, for instance, can have many `<li>`elements nested inside it; moreover, each`<li>`element can have sibling`<li>`nodes. That's weird, yet funny and true!

### Operations of a Tree 

Since every tree contains nodes, which can be a separate constructor from a tree, we will outline the operations of both constructors:`Node`and`Tree`.



### Node 

* `data`stores a value.
* `parent`points to a node's parent.
* `children`points to the next node in the list.

#### Tree

* `_root`points to the root node of a tree.
* `traverseDF(callback)`traverses nodes of a tree with DFS.
* `traverseBF(callback)`traverses nodes of a tree with BFS.
* `contains(data, traversal)` searches for a node in a tree.
* `add(data, toData, traverse)` adds a node to a tree.
* `remove(child, parent)`removes a node in a tree. 

### Implementation of a Tree 

Now let's write the code for a tree!

### Properties of a Node 

For our implementation, we will first define a function named`Node`and then a constructor named`Tree`.

```
function Node(data) {
    this.data = data;
    this.parent = null;
    this.children = [];
}
```

  
Every instance of Node contains three properties:`data`,`parent`, and`children`. The first property holds data associated with a node. The second property points to one node. The third property points to many children nodes.

#### Properties of a Tree 

Now, let's define our constructor for`Tree`, which includes the`Node` constructor in its definition: 

```
function Tree(data) {
    var node = new Node(data);
    this._root = node;
}
```

`Tree`contains two lines of code. The first line creates a new instance of`Node`; the second line assigns`node`as the root of a tree. 

The definitions of`Tree` and`Node` require only a few lines of code. These lines, however, are enough to help us simulate hierarchical data. To prove this point, let's use some example data to create an instance of`Tree`\(and, indirectly,`Node`\).

```
var tree = new Tree('CEO');
 
// {data: 'CEO', parent: null, children: []}
tree._root;

```

Thanks to the existence of`parent`and`children`, we can add nodes as children of`_root`and also assign`_root`as the parents of those nodes. In other words, we can simulate the creation of hierarchical data. 

#### Methods of a Tree 

Next, we will create the following five methods: 

###  **Tree**

1. `traverseDF(callback)`
2. `traverseBF(callback)`
3. `contains(data, traversal)`
4. `add(child, parent)`
5. `remove(node, parent)`

Since every method of a tree requires us to traverse a tree, we will first implement methods that define different types of tree traversal. \(Traversing a tree is a formal way of saying visiting every node of a tree.\) 

**1 of 5:`traverseDF(callback)`**  


This method traverses a tree with depth-first search.  

```
Tree.prototype.traverseDF = function(callback) {
 
    // this is a recurse and immediately-invoking function 
    (function recurse(currentNode) {
        // step 2
        for (var i = 0, length = currentNode.children.length; i < length; i++) {
            // step 3
            recurse(currentNode.children[i]);
        }
 
        // step 4
        callback(currentNode);
         
        // step 1
    })(this._root);
 
};
```

`traverseDF(callback)`has a parameter named`callback`. If it's unclear from the name,`callback`is presumed to be a function, which will be called later in `traverseDF(callback)`. 

The body of `traverseDF(callback)` includes another function named`recurse`. This function is a recursive function! In other words, it is self-invoking and self-terminating. Using the steps mentioned in the comments of`recurse`, I'll describe the general process that`recurse`uses to traverse the entire tree. 

Here are the steps: 

1. Immediately invoke`recurs`with the root node of a tree as its argument. At this moment,`currentNode`
   points to the current node. 
2. Enter a`for`loop and iterate once for each child of`currentNode`, beginning with the first child. 
3. Inside the body of the`for`loop, invoke recurse with a child of`currentNode`. The exact child depends on the current iteration of the
   `for`
   loop. 
4. When
   `currentNode`
   no longer has children, we exit the
   `for`
   loop and invoke the
   `callback`
   we passed during the invocation of 
   `traverseDF(callback)`
   . 

Steps 2 \(self-terminating\), 3 \(self-invoking\), and 4 \(callback\) repeat until we traverse every node of a tree.   


Recursion is a very hard topic to teach and requires an entire article to adequately explain it. Since the explanation of recursion isn't the focus of this article—the focus is implementing a tree—I'll suggest that any readers lacking a good grasp of recursion do the following two things.

First, experiment with our current implementation of `traverseDF(callback)` and try to understand to a degree how it works. Second, if you want me to write an article about recursion, then please request it in the comments of this article.

The following example demonstrates how a tree would be traversed with `traverseDF(callback)`. To traverse a tree, I'll create one in the example below. The approach that I will use at this moment isn't ideal, but it works. A better approach would be to use`add(value)`, which we will implement in step 4 of 5. 



```
var tree = new Tree('one');
 
tree._root.children.push(new Node('two'));
tree._root.children[0].parent = tree;
 
tree._root.children.push(new Node('three'));
tree._root.children[1].parent = tree;
 
tree._root.children.push(new Node('four'));
tree._root.children[2].parent = tree;
 
tree._root.children[0].children.push(new Node('five'));
tree._root.children[0].children[0].parent = tree._root.children[0];
 
tree._root.children[0].children.push(new Node('six'));
tree._root.children[0].children[1].parent = tree._root.children[0];
 
tree._root.children[2].children.push(new Node('seven'));
tree._root.children[2].children[0].parent = tree._root.children[2];
 
/*
 
creates this tree
 
 one
 ├── two
 │   ├── five
 │   └── six
 ├── three
 └── four
     └── seven
 
*/
```

Now, let's invoke`traverseDF(callback.`

```
tree.traverseDF(function(node) {
    console.log(node.data)
});
 
/*
 
logs the following strings to the console
 
'five'
'six'
'two'
'three'
'seven'
'four'
'one'
 
*/
```

**2 of 5:`traverseBF(callback)`**

This method uses breadth-first search to traverse a tree. 

The difference between depth-first search and breadth-first search involves the sequence that nodes of a tree visit. To illustrate this point, let's use the tree we created from`traverseDF(callback)`.

```
/*
 
 tree
 
 one (depth: 0)
 ├── two (depth: 1)
 │   ├── five (depth: 2)
 │   └── six (depth: 2)
 ├── three (depth: 1)
 └── four (depth: 1)
     └── seven (depth: 2)
 
 */
```

Now, let's pass`traverseBF(callback)`the same callback we used for`traverseDF(callback)`

```
tree.traverseBF(function(node) {
    console.log(node.data)
});
 
/*
 
logs the following strings to the console
 
'one'
'two'
'three'
'four'
'five'
'six'
'seven'
 
*/
```

The logs from the console and the diagram of our tree reveal a pattern about breadth-first search. Start with the root node; then travel one depth and visit every node in that depth from left to right. Repeat this process until there are no more depths to travel. 

Since we have a conceptual model of breadth-first search, let's now implement the code that would make our example work. 

