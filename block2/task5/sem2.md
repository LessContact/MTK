
# test #

exp = int "+" exp | int
... //simple
...

exp = int:e1 "+" exp:e2 {Add(:e1, :e2)} |
    int:e {:e};
int = digit+$d {Int(s2i($d)) }; //better?
digit = '0'-'9';
// $d for use in functions and stuff
// :e is for strings ig

Add(Int(1), Add(Int(2), Int(3)))

## peg-парсер ##

=====================================

### grammar? ###

S = sum | mul | int ;

digit = '0'-'9' ;

int = digit+ \$s {ArInt(s2i(\$s))} ;

ws = (' ' | '\t' | '\n' | '\r')\* ;

sum = "(" ws S:l ws "+" ws S:r ws ")" {ArSum(:l, :r)};

mul = "(" ws S:l ws "*" ws S:r ws ")" {ArMult(:l, :r)};

ArExpr ::= ArSum, ArMult, ArInt;

   ArSum(lhs : ArExpr, rhs : ArExpr);

   ArMult(lhs : ArExpr, rhs : ArExpr);

   ArInt(var : int);

   //this is a binary tree?

в отдельном файле грамматика, в основном файле -- код

=====================================

```flow
import lingo/pegcode/driver
...
s2ar(str : string) -> ArExpr {
    e_gr = "#include arith.lingo";
    parsic(
            compilePegGrammar(e_gr),
            str
        SemanticActions(setTree(defaultPegActions.t,
        "createArInt", \s -> ArInt(s2i(s[0]))))
    )
}
```
