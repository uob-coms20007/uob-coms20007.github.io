## While Language LL(1) Grammar

```text

  Prog ::= Stmt Stmts $

  Stmt ::= if BExp then Stmt else Stmt
        | while BExp do Stmt
        | skip
        | id := AExp
        | { Stmt Stmts }
  
  Stmts ::= ; Stmt Stmts
         |  

  Exp ::= BFac BExps

  BExps ::= || BFac BExps
         | 

  BFac ::= BNeg BFacs

  BFacs ::= && BNeg BFacs
         | 

  BNeg ::= ! BNeg
         | BRel

  BRel ::= AExp BRels
  
  BRels ::= < AExp BRels
         |  = AExp BRels

  AExp ::= AFac AExps
  
  AExps ::= + AFac AExps
         |  - AFac AExps
         | 

  AFac ::= Atom AFacs
  
  AFacs ::= * Atom AFacs
         | 
  
  Atom ::= num
         | true
         | false
         | ( Exp )
  


```