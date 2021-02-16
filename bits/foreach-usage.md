## ECMAScript Array.prototype.forEach usage reasons, pros and cons

#### [2021-02-15]

Oftentimes I have witnessed (and sometimes participated in) discussions about the practical appliance and theoretical advantage of one or another,
`forEach` and traditional imperative loop statements like `for`/`while` etc. to iterate over an array.

The strengths of `forEach`:

- *Uniform syntax* with other "callback-based" methods like `map`, `reduce`, `reduceRight`, `filter`, `some`, `every`. Every method from the list above takes in a function
which is invoked on every element of an array and optional second argument which means the `this` value inside.

- *Predefined counter*. For most cases we only want to get access to each element one-by-one. There is no need in additional variables, increment operations, conditions for running the loop.
So we get cleaner code and avoid variable clashes or memory overhead.

- *Methods, not only loops*. It means that you can create such useful constructions as `Object.keys(obj).forEach(callback[, thisArg])`. By the way, this pattern is a good alternative to `for...in` loop with inner check for own properties;
- *New scope*. These methods incapsulate the traversing work inside themselves, so you don't pollute the external/global scope;
- *Functional style*. I mean `map`, `reduce`, `reduceRight`, `filter`, `some`, `every`. Firstly, as explained above, we don't pollute the external environment. Secondly, we use methods with function as argument what is more flexible, readable and testable;
- *Performance*. Nowadays they fast enough and have a comparable speed as traditional loops (e.g. [jsPerf: for vs forEach](https://jsperf.com/for-vs-foreach/37)).
- *Off-by-one error*.


### Useful links

- [ECMA-262 Standard](https://tc39.es/ecma262/#sec-array.prototype.foreach)
- [Benchmark1](https://jsbench.me/7ejagsewtr)
