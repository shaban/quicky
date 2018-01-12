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

### dom.X

This is the representation of an Expression inside a TextNode or DOM Attribute Value

```
//X is an Expression
type X struct {
	//B is holding the evaluated boolean Value:

	//of a Boolean Expression
	//{{component.Expression}} for Boolean Attributes
	//or {{component.Expression ? stringIfTrue}} for regular Attributes

	//of a Reverse Boolean Expression
	//{{component.Expression : stringIfTrue}}
	//string is only set if boolean evaluates to false

	//of a ternary expression
	//{{component.Expression ? stringIfTrue : stringIfFalse}}
	//This is the composite of a reverse and a normal
	//Boolean Expression

	//the latter two Expression Types are disallowed
	//in the scope of a Boolean Attribute
	//like required etc.
	B bool
	//T is printed when B evaluates to True
	T string
	//F is printed when B evaluates to False
	F string
	//V is a placeholder for string and int expressions
	//if V is a string it will escape its output
	//if V is an int it will be converted to string
	//if V is neither an error will be thrown
	V interface{}
	//XT is the Expression Type
	//0 is a Boolean Expression
	//1 is a reverse Boolean Expression
	//2 is a ternary Expression
	//3 is a string or int printed escaped/verbatim
	//respectively into the output
	XT int
}
```


## Illustration
![Parsing](../diagrams/parsing.png)
