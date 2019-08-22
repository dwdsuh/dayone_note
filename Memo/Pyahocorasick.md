# Pyahocorasick

This python library allows users to use as a plain dict-like Trie or convert a Trie to an automation for efficient AhoCorasick search

### 1. Trie

- digital tree, radix tree, prefix tree

- an ordered tree data structure used to store a dynamic set or associative array where the keys are usually strings. 

  ![image](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png)

- no node in the tree stores the key associated with that node.

- Its position in the tree defines the key with which it is associated

#### 1.1. Algorithms

```python
class Node():
  def __init__(self):
    self.children: Dict[str, Node] = {} 
#mapping from ch to Node
    self.value : Any = None
# children is a dictionary of characters to a node's children
# it is saild that a terminal node is one which represents a complete string

def find(node: Node, key: str)-> Any:
  for char in key:
    if char in node.children:
      node = node.children[char]
    else:
      return None
   return node.value

def insert(node: Node, key: str, value: Any)->None:
  for char in key:
    if char not in node.children:
      node.children[char] = Node()
    node = node.children[char]
  node.value = value
```





### 2. Aho-Corasick Automation

+ search all occurrences of multiple strings in an input string making a single pass over the input string. 
+ can build large automations and pickle them to reuse them over an over as an indexed structure for fast multi pattern string matching

#### 2.1 Aho-Corasick Algorithm

+ dictionary-matching algorithm that locates elements of a finite set of strings within an input text. It matches all strings simultaneously. 

+ Complexity: linear in the length of the strings plus the length of the searched text plus the number of output matches. 

+ the algorithm constructs a finite-state machine that resembles a trie with additional links between the various internal nodes.

+ These extra internal links allow fast transitions between failed string matches to other branches of the trie that share a common prefix. 

+ automation to transition between string matches without the need for backtracking

+ Example

  ![image](https://upload.wikimedia.org/wikipedia/commons/thumb/6/62/Ahocorasick.svg/220px-Ahocorasick.svg.png)
  + a dictionary consisting of the following words: {a,ab,bab,bc,bca,c,caa}

  + each row in the table representing **a node in the trie**, with the colimn path indicating **the unique sequence of characters** from the root to the node. 

  + blue node: exist in the dictionary | grey node: non-exsit in the dictionary

  + black arc: directs child node 
    blue arc: directs the node that is **the longest possible strict suffix** of it in the graph 

    > this can be computed in linear time by traversing the blue arcs of a node's parent until the traversing node has a childe matching the character of the target node

    green arc:  dictionary suffix. directs the next node in the dictionary that can be reached by following blue arcs.  

  + At each step, the current node is extended by finding its child. and if that doesn't exist, finding its suffix's child, and if that doesn't work. finding its suffix's suffi's child. 