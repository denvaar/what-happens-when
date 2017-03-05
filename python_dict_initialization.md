## What happens when... `d = {'hello': 'world'}` entered into a Python interactive shell?

### Assumptions
- Python3.5
- CPython

### Events

### The code, `d = {'hello': 'world'}`, is entered into the Python console.

### Lexical Analysis is performed

[1](https://docs.python.org/3.1/reference/lexical_analysis.html)

The string of text is read as unicode code points and first undergoes [lexical analysis](), or tokenization, where it is split up into different tokens. There are five different lexical categories in total:
  - *Identifiers*: Variable names
  - *Operators*: Symbols that are used to operate on data (+, -, /, etc.)
  - *Delimiters*: Grouping, punctuation, and assignment/binding symbols
  - *Literals*: Values callsigied by types (numbers, booleans, text, etc.)
  - *Comments*: Text that is used to document the code.
`d = {'hello': 'world'}` is scanned from left-to-right. White space characters are used to separate tokens where appropriate:
`d` is tokenized as `NAME`, which is defined as `1`
`=` is tokenized as `EQUAL`, which is defined as `22`
`{` is tokenized as `LBRACE`, which is defined as `25`
`'hello'` is tokenized as `STRING`, which is defined as `3`
`:` is tokenized as `COLON`, which is defined as `11`
`'world'` is also tokenized as `STRING`
`}` is tokenized as `RBRACE`, which is defined as `26`

`PyParser_ParseString` function kicks off the tokenization and parsing cycle.
`tok_state` is a C structure is used to represent the state of the tokenizer. It is initialized within the `PyParser_ParseStringObject` function.

### The tokens are parsed into an abstract syntax tree (AST)

Based on the rules of the Python grammar, the tokens from the lexical analyzer are used to build an [abstract syntax tree]().

`PyTokenizer_Get` (found in `Parser/tokenizer.c`) is repeatedly called from inside the `parsetok` function, which is found in `Parser/parsetok.c`. The `parsetok` function takes each token and determines which type it is. 

### The Abstract Syntax Tree is compiled into bytecode

[](https://tomlee.co/wp-content/uploads/2012/11/108_python-language-internals.pdf)
The AST that was created is then used to generate a `PyCodeObject`, which contains all of the data and code neccessary to be executed as bytecode by the CPython interpreter.

### Interpreting

