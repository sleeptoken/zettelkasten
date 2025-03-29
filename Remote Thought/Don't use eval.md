
2025-03-25 10:26

Tags: [[py]]

`datamap = eval(input('Provide some data here: '))` means that you actually evaluate the code _before_ you deem it to be unsafe or not. It evaluates the code as soon as the function is called. See also [the dangers of `eval`](http://nedbatchelder.com/blog/201206/eval_really_is_dangerous.html)

[`ast.literal_eval`](https://docs.python.org/3/library/ast.html#ast.literal_eval) raises an exception if the input isn't a valid Python datatype, so the code won't be executed if it's not.

Use `ast.literal_eval` whenever you need [`eval`](https://docs.python.org/3/library/functions.html#eval). You shouldn't usually evaluate literal Python statements.

This isn't 100% correct advice since any bitwise operators (or overloaded operators) will fail. Eg. `ast.literal_eval("1 & 1")` will throw an error but `eval("1 & 1")` will not.
### References
https://stackoverflow.com/questions/15197673/using-pythons-eval-vs-ast-literal-eval