# Mini-language for queries

## Tokens
Regex-defined tokens:
```
NAME = [A-Za-z][A-Za-z_0-9]*
INT = -?[0-9][0-9_]*
REAL = -?[0-9]+\.[0-9]*([Ee][+\-]?[0-9]+)*
STR = '[^']*'|"[^"]*"
DATE = -?[0-9][0-9_]*[dwmy]
```
Simple tokens defined by literal value(s):
```
FILTER = 'filter'
GROUP = 'group'
OP = 'or' | 'and' | '==' |  '!=' |  '<' |  '<=' |  '>' |  '>=' | 'in' | 'not in' | '+' | '-' | '*' | '/'
L_BRACE = '('
R_BRACE = ')'
DOT = '.'
COMMA = ','
TRUE = 'True'
FALSE = 'False
```
## Grammar
```
query: filter grouper
filter: 'filter' expr
grouper: 'group' expr
expr: disjunction
disjunction:
    | conjunction ('or' conjunction )+ 
    | conjunction
conjunction:
    | inversion ('and' inversion )+ 
    | inversion
inversion:
    | 'not' inversion 
    | comparison
comparison: 
    | sum op_cmp sum
    | sum
op_cmp: '==' |  '!=' |  '<' |  '<=' |  '>' |  '>=' | 'in' | 'not in'
sum: 
    | sum '+' term
    | sum '-' term
    | term
term:
    | term '*' factor
    | term '/' factor
    | term '//' factor
    | term '%' factor
    | factor
factor: 
    | '-' factor
    | '+' factor
    | atom
atom:
    | NAME
    | field_getter
    | literal 
    | func_call
    | '(' expr ')'
field_getter: '.' NAME
literal:
    | INT
    | REAL
    | STR
    | DATE
    | 'True'
    | 'False'
func_call: NAME '(' (expr (',' expr)*)? ')'
```

## Type system

The language supports has the following types:

#### Integer
Represents numeric integer value. Supports arithmetic operations and comparisons. Note that division (`/` ) generates **Real** value if integer cannot be divided without reminder. Use `//` for integer division.

#### Real
Represents numeric integer value. Supports arithmetic operations and comparisons.

####  String
Represents a sequence of characters. Can be concatenation using `+`, substring check by `in` operator. 

TODO: Probably we need some string manipulation functions like `replace()`, `format()` , `str()` 

### Date manipulation types

There are two groups of "date" types: one that represents some particular period of history (**Day**, **Week**, **Month**, **Year**) and difference types that represent quantities of items of the first group (**DayDelta**, **WeekDelta**, **MonthDelta**, **YearDelta**). 

Types of first group support subtraction that produces corresponding delta type, the addition of corresponding delta type, rich comparison operators  (`==`, `<=`, etc.), checks using `in` and `not in` for being part of a larger period. Different periods cannot be mixed (subtraction off **Day** from **Week** is not allowed).

Types of the second group support `+` and `-` and multiplication by **Integer**. They can be compared using all rich comparison operators. Different delta cannot be mixed (addition or comparison of **DayDelta** and **WeekDelta** is not allowed)

#### Location
Represents a location on the map. Can be compared using `==` and `!=`.

 TODO: Do we need any other operators? Do we need this type at all (can be replaced with **String**)?

### Bultin names

The following names and functions are supported:

- `today` and `today()` -- returns current date
- `week(day)`  returns **Week** that contains given **Day**
- `month(day)`  returns **Month** that contains given **Day**
- `year(day)`  returns **Week** that contains given **Day** or **Month**



