# Mini-language for queries

## Tokens
Regex-defined tokens:
```
NAME = [A-Za-z][A-Za-z_0-9]*
INT = -?[0-9][0-9_]*
REAL = -?[0-9]?\.[0-9]*([Ee][+\-]?[0-9]+)*
STR = '[^']*'|"[^"]*"
DATE = -?[0-9][0-9_]*[dwmy]
```
Simple tokens defined by literal value(s):
```
FILTER = 'filter'
GROUP = 'group'
OP = 'not' | 'or' | 'and' | '==' |  '!=' |  '<' |  '<=' |  '>' |  '>=' | 'in' | 'not in' | '+' | '-' | '*' | '/'
L_BRACE = '('
R_BRACE = ')'
DOT = '.'
COMMA = ','
TRUE = 'true'
FALSE = 'false'
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
field_getter: ('.' NAME)+
literal:
    | INT
    | REAL
    | STR
    | DATE
    | 'true'
    | 'false'
func_call: NAME '(' (expr (',' expr)*)? ')'
```

## Type system

The language supports has the following types:

#### Integer
Represents numeric integer value.
Supports arithmetic operations and comparisons.
Note that division (`/` ) generates **Real** value.
Use `//` for integer division.

#### Real
Represents numeric integer value.
Supports arithmetic operations and comparisons.

####  String
Represents a sequence of characters. 
Can be concatenation using `+`, substring check by `in` operator. 

TODO: Probably we need some string manipulation functions like `replace()`, `format()` , `str()` 

### Date manipulation types

There are two groups of "date" types: one that represents some particular
period of history (**Day**, **Week**, **Month**, **Year**) and difference 
types that represent deltas of items of the first group 
(**DayDelta**, **WeekDelta**, **MonthDelta**, **YearDelta**). 

Types of first group support subtraction that produces corresponding delta type, 
the addition of corresponding delta type, comparison operators (`==`, `<=`, etc),
checks using `in` and `not in` for being part of a larger period (check details below). 
Different periods cannot be mixed (subtraction off **Day** from **Week** is not allowed).

Types of the second group support `+` and `-` and multiplication by **Integer**.
They can be compared using all rich comparison operators.
Different delta cannot be mixed (addition or comparison of **DayDelta** and **WeekDelta** is not allowed)


Logic of `in` and `not in` operators:
**Day** can be part of **Week**, **Month**, **Year**.
**Week** can be part of **Month** and **Year**. 
If week is only partially contained by month or year, the `in` will return **false**.
**Month** can be part of **Year**.


#### Location
Represents a location on the map.
Can be compared using `==` and `!=`.

TODO: Add info about addition functions for measuring distance and extracting some info (like `city()`)
when it will be clear what exactly we will store for each **Location**.

### Bultin names

The following names and functions are supported:

- `today` and `today()` -- returns current date as **Day**
- `week(day)`  returns **Week** that contains given **Day**
- `month(day)`  returns **Month** that contains given **Day**
- `year(day)`  returns **Week** that contains given **Day** or **Month**
- `ceil(num)` returns **Integer** that is ceiling of given **Real**
- `round(num)` returns **Integer** that is rounding of given **Real**
- `regexp(pattern, str)` checks that str matches given pattern. 
  Pattern is PCRE regex.
  Returns **Bool**, both arguments should be **String**.

