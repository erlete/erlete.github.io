---
layout: post
title: Search algorithms
---

## What are search algorithms?

_Search algorithms are finite sequences of instructions that enable a program to explore the contents of an array and determine the shortest possible path (if existent) from one of its nodes to another one._

If you haven't quite understood the previous expression, well... that's okay! You are here to learn about this type of algorithms, so do not be overwhelmed if you find parts of this explanation a bit hard to understand. First of all, let's start with the basics, two simple concepts:

* **Array**: a collection of elements, generally of the same type: `[1, 2, 3]`, `("sun", "moon", "comet")`.
* **Node**: an array's element that contains useful information about itself: `"moon": {"index": 1, "state": "rotating"}`.

Both of these elements are actually _data structures_, which are, abstractly, specifically designed collections of data that ease their treatment process by providing with special attributes and methods.

It is worth noting that the node is the main character of this explanation, since it represents the basic element that composes the array. I am pretty sure that you know what an atom is. Yeah, that tiny little thing that you can't see. I'm going to take a leap of faith and assume that you also know what a molecule is. This way, you understand that a molecule is made out of atoms stuck together in specific shapes and quantities. The position of the atoms in the molecule is very important, as well as other properties such as their size, composition, orientation... Now, try and compare an atom with a node and you will see that they are quite alike. A node has attributes such as x and y coordinates, a state (that defines _what_ the node is) or a parent (which determines where that node came from).

This, as you can see below, can be traslated into code with ease:

```python
class Node:
	"""A class that handles collections of attributes from specific elements."""

	def __init__(self, x, y, state=None, parent=None):
		self.x, self.y = x, y
		self.state = state
		self.parent = parent
```

Just like that, we've defined the most used data structure regarding search algorithms. Easy, right? Let's head onto the next couple of terms:

* **Start/goal nodes**: special nodes that represent the origin from which the search process begins and the ending point of said process, respectively. Sometimes, there is no ending node, which results in an unsolvable problem.
* **Frontier**: special array that provides with element addition, removal and checking methods. The frontier of a node represents all the available nodes that surround the previous one, to which the algorithm can "move".

The frontier element can also be translated to code easily:

```python
class Frontier:
	"""A class that handles collections of nodes."""

	def __init__(self):
		self.nodes = []
```

But wait, the definition above said that the frontier provides with special methods... so let's add them:

```python
class Frontier:
	"""A class that handles arrays of nodes."""

	def __init__(self):
		self.nodes = []

	def add(self, node):
		"""Adds a node to the frontier."""
		self.nodes.append(node)

	def is_empty(self):
		"""Determines whether or not the frontier is empty."""
		return len(self.nodes) == 0

	def is_appended(self, node):
		"""Determines whether or not a node is in the frontier."""
		return node in self.nodes
```

If you have read the docstrings (and you should have) you might have realized that the `Node` and `Frontier` classes are quite similar: both offer a simple and compact way of handling collections of elements.

Now you have enough terminology knowledge to get started with search algorithms. Let's suppose we have an array of nodes such as:

```
(X: 0, Y: 0, S: 1)     (X: 1, Y: 0, S: 1)     (X: 2, Y: 0, S: 0)

(X: 0, Y: 1, S: 0)     (X: 1, Y: 1, S: 1)     (X: 2, Y: 1, S: 0)

(X: 0, Y: 2, S: 0)     (X: 1, Y: 2, S: 1)     (X: 2, Y: 2, S: 1)
```

Where `X` represents the column index, `Y` represents the row index and `S` represents the state of the node: 0 if the node cannot be accessed by the search algorithm and 1 if it can (you can think of this as if nodes with state 0 were walls and nodes with state 1 were paths).
