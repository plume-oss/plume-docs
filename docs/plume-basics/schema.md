# Schema

_Last updated: 15 Dec 2020_

The latest CPG schema as defined by ShiftLeft is extremely complex in the sense that there is a large number of unique
vertex types and possible permutations of which edges may be permitted between different vertex types. To simplify this
complexity, vertices are categorized and subdivided by base traits and may inherit properties accordingly. Additionally, 
as we will see further below, vertices can also be categorized into where they fit in regards to the program and method
structure.

The original schema can be found [here](https://github.com/ShiftLeftSecurity/codepropertygraph).

## Base Traits

![Base Traits](../assets/images/plume-basics/code-property-graph/traits.png){: align=right style="height:280px;width:380px;" }

A vertex can inherit either none or many base traits. Traits are created as abstract classes which are more formally
described in the context of the driver in the [KDoc](https://plume-oss.github.io/plume-driver/kotlindoc/za/ac/sun/plume/domain/models/). 

Base traits can also inherit properties from one another e.g. a CFG Vertex inherits properties from the AST Vertex
and Within Method base traits.

Note that column numbers are a property of CFG Vertex (and by inheritance, Call Repr and Expression) but these cannot 
be extracted from Java bytecode, only Android bytecode see [this issue](https://github.com/soot-oss/soot/issues/705) for more info.

| Vertex Label | Description | Properties |
|-|-|-|
| Declaration | Declare a variable by specifying its data type and name. | Name |
| AST Vertex | Any vertex that can exist in an abstract syntax tree. | Order |
| Within Method | Any node that can exist in a method. No properties to inherit. | None |
| Local Like | Formal input parameters, locals, and identifiers. | Name |
| CFG Vertex | Any node that can occur as part of a control flow graph. | Line number, column number, code |
| Tracking Point | Any node that can occur in a data flow. | None |
| Call Repr | Call representation. | Code, name, and signature |
| Expression | Expression as a specialisation of tracking point. | Code, order, and argument index |

## Vertex Types

| Vertex Label | Description | Traits |
|-|-|-|
| META_DATA | Vertex to save meta data about the graph on its properties. Exactly one vertex of this type per graph. | None |
| FILE | Vertex representing a source file. Often also the AST root. | AST_NODE |
| METHOD | A method/function/procedure. | DECLARATION, CFG_NODE, AST_NODE |
| METHOD_PARAMETER_IN | This vertex represents a formal parameter going towards the callee side. | DECLARATION, LOCAL_LIKE, TRACKING_POINT, AST_NODE |
| METHOD_RETURN | A formal method return. | CFG_NODE, TRACKING_POINT |
| MODIFIER | A modifier, e.g., static, public, private. | AST_NODE |
| TYPE | A type which always has to reference a type declaration and may have type argument children if the referred to type declaration is a template. | None |
| TYPE_DECL | A type declaration. | AST_NODE |
| BINDING | A binding of a METHOD into a TYPE_DECL. | None |
| TYPE_PARAMETER | Type parameter of TYPE_DECL or METHOD. | AST_NODE |
| TYPE_ARGUMENT | Argument for a TYPE_PARAMETER that belongs to a TYPE. It binds another TYPE to a TYPE_PARAMETER. | AST_NODE |
| MEMBER | Member of a class struct or union. | DECLARATION, AST_NODE |
| NAMESPACE_BLOCK | A reference to a namespace. | AST_NODE |
| LITERAL | Literal/Constant. | EXPRESSION |
| CALL | A (method)-call. | EXPRESSION, CALL_REPR |
| LOCAL | A local variable. | DECLARATION, LOCAL_LIKE, AST_NODE |
| IDENTIFIER | An arbitrary identifier/reference. | EXPRESSION, LOCAL_LIKE |
| FIELD_IDENTIFIER | A vertex that represents which field is accessed in a <operator>.fieldAccess, in e.g. obj.field. The CODE part is used for human display and matching to MEMBER vertices. The CANONICAL_NAME is used for dataflow tracking; typically both coincide. However, suppose that two fields foo and bar are a C-style union; then CODE refers to whatever the programmer wrote (obj.foo or obj.bar), but both share the same CANONICAL_NAME (e.g. GENERATED_foo_bar) | EXPRESSION |
| RETURN | A return instruction. | EXPRESSION |
| BLOCK | A structuring block in the AST. | EXPRESSION |
| ARRAY_INITIALIZER | Initialization construct for arrays. | AST_NODE |
| METHOD_REF | Reference to a method instance. | EXPRESSION |
| TYPE_REF | Reference to a type/class. | EXPRESSION |
| CONTROL_STRUCTURE | A control structure such as if, while, or for. | EXPRESSION |
| JUMP_TARGET | A jump target made explicit in the code using a label. | CFG_NODE, AST_NODE |
| UNKNOWN | A language-specific vertex. | EXPRESSION |

## Edge Types and Constraints

Part of the schema restricts which vertices can be connected by specific edge types. The following table shows
which vertices and their outgoing edges and vertex labels comply with the schema enforced by the Plume driver.
If one attempts to create an edge between two vertices and this violates the schema, a 
[PlumeSchemaViolationException](https://plume-oss.github.io/plume-driver/kotlindoc/za/ac/sun/plume/domain/exceptions/plumeschemaviolationexception/)
is thrown describing the vertices and edges in violation. This design decision is used to enforce schema 
consistency when using the driver against schemaless storage backends.

### AST

Syntax tree edge.

| Source | Valid Target(s) |
|-|-|
| METHOD | METHOD_RETURN, METHOD_PARAMETER_IN, MODIFIER, BLOCK, TYPE_PARAMETER, LOCAL |
| TYPE | TYPE_ARGUMENT |
| TYPE_DECL | TYPE_PARAMETER, MEMBER, MODIFIER |
| NAMESPACE_BLOCK | FILE, METHOD, NAMESPACE_BLOCK |
| CALL | CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE |
| RETURN | CALL, IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| BLOCK | CALL, IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, LOCAL, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| TYPE_REF | LITERAL, MODIFIER, ARRAY_INITIALIZER, CALL, LOCAL, IDENTIFIER, RETURN, BLOCK, JUMP_TARGET, UNKNOWN, CONTROL_STRUCTURE, METHOD_REF, TYPE_REF |
| CONTROL_STRUCTURE |  LITERAL, MODIFIER, ARRAY_INITIALIZER, CALL, LOCAL, IDENTIFIER, RETURN, BLOCK, JUMP_TARGET, UNKNOWN, CONTROL_STRUCTURE, METHOD_REF, TYPE_REF |
| UNKNOWN |  LITERAL, MEMBER, MODIFIER, ARRAY_INITIALIZER, CALL, LOCAL, IDENTIFIER, FIELD_IDENTIFIER, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |

### CFG

Control flow edge.

| Source | Target(s) |
|-|-|
| METHOD | CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, METHOD_RETURN, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| LITERAL | CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| CALL | CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| IDENTIFIER |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, METHOD_RETURN, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| FIELD_IDENTIFIER |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| RETURN |  METHOD_RETURN |
| BLOCK |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| METHOD_REF |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, METHOD_RETURN, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| TYPE_REF |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| CONTROL_STRUCTURE |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, METHOD_RETURN, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| JUMP_TARGET |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, METHOD_RETURN, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| UNKNOWN |  CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, METHOD_RETURN, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |

### CAPTURED_BY

Connection between a captured LOCAL and the corresponding CLOSURE_BINDING.

| Source | Target(s) |
|-|-|
| LOCAL | BINDING |

### BINDS_TO

Type argument binding to a type parameter.

| Source | Target(s) |
|-|-|
| TYPE_ARGUMENT | TYPE_PARAMETER |

### REF

A reference to e.g. a LOCAL.

| Source | Target(s) |
|-|-|
| BINDING | METHOD |
| TYPE_ARGUMENT | TYPE |
| IDENTIFIER | LOCAL, METHOD_PARAMETER_IN |

### RECEIVER

The receiver of a method call which is either an object or a pointer.

| Source | Target(s) |
|-|-|
| CALL | CALL, IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, BLOCK, CONTROL_STRUCTURE, UNKNOWN |

### CONDITION

Edge from control structure vertex to the expression that holds the condition.

| Source | Target(s) |
|-|-|
| TYPE_REF | LITERAL, ARRAY_INITIALIZER, CALL, IDENTIFIER, RETURN, BLOCK, JUMP_TARGET, UNKNOWN, CONTROL_STRUCTURE, METHOD_REF, TYPE_REF |
| CONTROL_STRUCTURE | LITERAL, ARRAY_INITIALIZER, CALL, IDENTIFIER, RETURN, BLOCK, JUMP_TARGET, UNKNOWN, CONTROL_STRUCTURE, METHOD_REF, TYPE_REF |

### BINDS

Relation between TYPE_DECL and BINDING vertex.

| Source | Target(s) |
|-|-|
| TYPE_DECL | BINDING |

### ARGUMENT

Relation between a CALL and its arguments and RETURN and the returned expression.

| Source | Target(s) |
|-|-|
| CALL | CALL, IDENTIFIER, FIELD_IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |
| RETURN | CALL, IDENTIFIER, LITERAL, METHOD_REF, TYPE_REF, RETURN, BLOCK, JUMP_TARGET, CONTROL_STRUCTURE, UNKNOWN |

### SOURCE_FILE

Source file of a vertex, in which its LINE_NUMBER and COLUMN_NUMBER are valid.

| Source | Target(s) |
|-|-|
| METHOD | FILE |

### CALL

Method invocation edge - caller/callee relationship.

| Source | Target(s) |
|-|-|
| CALL | METHOD |

## Program Categories

In addition to the categorizing vertices by their base traits, we can also categorize them with regards to in which parts of 
the program that they will occur. Note that binary expressions, e.g. the addition operator (+), are represented as CALL vertices
with operands connected by ARGUMENT edges as well as AST edges.

| Category | Vertices |
| - | - |
| Program structure | FILE, NAMESPACE_BLOCK, MEMBER |
| Type declarations | TYPE_DECL, TYPE_PARAMETER, MEMBER, TYPE, TYPE_ARGUMENT, BINDING |
| Method header | METHOD, METHOD_PARAMETER_IN, METHOD_RETURN, LOCAL, BLOCK, MODIFIER |
| Method body | LITERAL, IDENTIFIER, FIELD_IDENTIFIER, CALL, RETURN, METHOD_REF, TYPE_REF, CONTROL_STRUCTURE, JUMP_TARGET, ARRAY_INITIALIZER |
| Language Dependent | META_DATA, UNKNOWN |