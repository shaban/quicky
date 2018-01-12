# Parsing
## Data Types

The Component embeds three struct pointers

- *dom.N
- *js.Object
- *dom.C

## *dom.N

This is the representation of a Virtual DOM Node

it utilizes:

- N string = name of the tag e.g div, p etc.
- A []*T = array of pointers to Attribute Nodes T stands for Tuple
- C []*N = array of pointers to Child Nodes
- P *N = pointer to Parent Node or nil if this the Components Root Node
- T map[int]*T = map of Text Nodes, int is the position. -1 for head Text Nodes or the index of the Child Node after which this Text Node is rendered
- I int = counter to tell which TextNode will be the next to get rendered

### Source
```
//N is a VDOM Node
type N struct {
	N string
	A []*T
	C []*N
	P *N
	T map[int]string
	I int
}
```


## Illustration
![Parsing](../diagrams/parsing.png)
