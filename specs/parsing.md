# Parsing
## Data Types

The Component embeds three struct pointers

- *dom.N
- *js.Object
- *dom.C

## *dom.N

This is the representation of a Virtual DOM Node

**Source**
```
//N is a VDOM Node
type N struct {
	//N is the XMLName.Local of the Tag
	N string
	//A is an array of pointers to Attribute Nodes,
	//T stands for Tuple
	A []*T
	//C is an array of pointers to Child Nodes
	C []*N
	//P is a pointer to the Parent Node or nil 
	//if this the Components Root Node
	P *N
	//TN is a map of Text Nodes
	// map key int is the position
	//-1 for head Text Nodes 
	//or the index of the Child Node after which this Text Node is rendered
	TN map[int]*TN
	//I is a counter to tell which TextNode will be the next to get rendered
	I int
}
```

### dom.T

This is the representation of a DOM Attribute Key, Value pair.

**Source**
```
//T is an Attribute-Tuple
type T struct {
	//K is the Name of the Attribute
	//e.g class, style, href
	K string
	//V is the verbatim value of the attribute
	//it will only be used when
	//this is an Attribute Tuple
	//that doesn't contain Expressions
	V string
	//X is an array of pointers to Expressions
	X []*X
	//F is a map of value fragments
	//map key int is the position for rendering
	//-1 for head fragments
	//or the index of an expression after which the 
	//fragment will be rendered
	F map[int]string
	//B indicates if this Tuple represents a boolean attribute
	//e.g. disabled, required etc.
	B bool
}
```



## Illustration
![Parsing](../diagrams/parsing.png)
