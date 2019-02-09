# EvalLLib
A simple expression evaluator for [JSQLParser](https://github.com/UBOdin/jsqlparser)

## Creating an Evaluator

EvalLib consists of one abstract class: `net.sf.jsqlparser.eval.Eval` that implements nearly all of EvalLib's functionality.  
To complete EvalLib, you need to add a method to assign values to variables (i.e., you need to define a scope lookup function).
The abstract method `eval` takes a JSQLParser [Column](https://github.com/UBOdin/jsqlparser/blob/master/src/net/sf/jsqlparser/schema/Column.java) as input and must return a [PrimitiveValue](https://github.com/UBOdin/jsqlparser/blob/master/src/net/sf/jsqlparser/expression/PrimitiveValue.java) subclass.
There are two suggested ways of doing this: (1) Instantiate an Eval instance inline whenever you get a new scope, or (2) subclass Eval.

For example:
```
  Map<String,PrimitiveValue> scope = /* ... */;
  Eval eval = new Eval() { 
    public PrimitiveValue eval(Column col){
      String name = col.getColumnName();
      if(col.getTable() != null && col.getTable().getName() != null){
        name = col.getTable().getName() + "." + col
      }
      return scope.get(name);
    }
  }
```

## Using an Evaluator
Call `eval.eval()`.  For example: 
```
result =
    eval.eval(
      new GreaterThanEquals(
        new Column(new Table(null, "R"), "A"), // `R.A`
        new LongPrimitive(5)
      )
    );
```
