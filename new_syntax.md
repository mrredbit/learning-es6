### Let
```
var a=2; {
	let a =3 ; 
	console.log( a ); // 3 
}
console.log( a ); // 2
```
```
var funcs = [];
for (let i = 0; i < 5; i++) {
    funcs.push(function () {
        console.log(i);
    });
}
funcs[3](); // 3
```

### const
```
{
    const a = 2;
    console.log(a);
    a = 3;
	// 2
	// TypeError!
}
```
```
{
    const a = [1, 2, 3];
    a.push(4);
    console.log(a); // [1,2,3,4]
    a = 42; // TypeError
}
```

### Spread / Rest (... operator)
#### Spread
```
function foo(x, y, z) {
    console.log(x, y, z);
}
foo(...[1, 2, 3]); // 1 2 3
```
```
var a = [2, 3, 4];
varb = [1, ...a, 5];
console.log(b); // [1,2,3,4,5]
```
#### Rest
```
function foo(x, y, ...z) {
    console.log(x, y, z);
}
foo(1, 2, 3, 4, 5); // 1 2 [3,4,5]
```

#### Default Parameter Values
```
function foo(x = 11, y = 31) {
    console.log(x + y);
}

foo(); // 42
foo(5, 6); // 11
foo(0, 42); // 42
foo(5); // 36
foo(5, undefined); // 36 <-- `undefined` is missing
foo(5, null); // 5  <-- null coerces to `0`
foo(undefined, 6); // 17 <-- `undefined` is missing
foo(null, 6); // 6  <-- null coerces to `0`
```
#### Default Value Expressions
```
function bar(val) {
    console.log("bar called!");
    return y + val;
}
function foo(x = y + 3, z = bar(x)) {
    console.log(x, z);
}

var y = 5;
foo(); // "bar called" // 8 13

foo(10); // "bar called" // 10 15

y = 6;
foo(undefined, 10); // 9 10
```

### Destructuring
#### Destructuring array
```
function foo() {
    return [1, 2, 3];
}
```
Old way
```
var tmp = foo(),
    a = tmp[0], b = tmp[1], c = tmp[2];
console.log(a, b, c); // 1 2 3
```
New way
```
var [a,b,c]=foo();
console.log(a, b, c); // 1 2 3

```
#### Destructuring object
```
function bar() {
    return {
        x: 4, y: 5, z: 6
    };
}
```
Old way
```
var tmp = bar(), x = tmp.x, y = tmp.y, z = tmp.z;
console.log(x, y, z); // 4 5 6
```
New way
```
var {x:x, y:y, z:z} = bar();
console.log(x, y, z); // 4 5 6
```
Object Property Assignment Pattern
```
var {x, y, z} = bar();
console.log(x, y, z); // 4 5 6
```
It is actually flipped from conventional assignment, assign from left to right to the :xxx part
```
var {x:bam, y:baz, z:bap} = bar();
console.log(bam, baz, bap); // 4 5 6
console.log(x, y, z); // ReferenceError
```
Not right to left like construction
```
var X = 10, Y = 20;
var o = {a: X, b: Y};
console.log(o.a, o.b); // 10 20
```
It is helpful in this case
```
var aa = 10, bb = 20;
var o = {x: aa, y: bb};
var {x:AA, y:BB} = o;
console.log(AA, BB); // 10 20
```
When doing object destructuring without using var/let/const, we need to add () to wrap it, otherwise it will be syntax error.
```
var a, b, c, x, y, z;
[a, b, c] = foo();
({x, y, z} = bar());
console.log(a, b, c); // 1 2 3
console.log(x, y, z); // 4 5 6
```
#### Destructuring Assignment Expressions
Assigned with reference
```
var o = {a: 1, b: 2, c: 3}, a, b, c, p;
p = {a, b, c} = o;
console.log(a, b, c); // 1 2 3
p === o; // true
```
```
var o = [1, 2, 3], a, b, c, p;
p = [a, b, c] = o;
console.log(a, b, c); // 1 2 3
p === o; // true
```
Chain destructuring assignment
```
var o = {a: 1, b: 2, c: 3}, p = [4, 5, 6],
    a, b, c, x, y, z;
({a} = {b, c} = o);
[x, y] = [z] = p;
console.log(a, b, c);
// 1 2 3 
// 4 5 4
```
Too many, too few, just enough
```
var [,b] = foo();
var {x, z}=bar();
console.log(b, x, z); // 2 4 6
```
```
var [,,c,d] = foo();
var {w, z}=bar();
console.log(c, z); // 3 6
console.log(d, w); // undefined undefined
```
```
var a = [2, 3, 4];
var [b, ...c] = a;
console.log(b, c); // 2 [3,4]
```
With default values
```
var [a = 3,b = 6,c = 9,d = 12]=foo();
var {x = 5, y = 10, z = 15, w = 20}=bar();
console.log(a, b, c, d); // 1 2 3 12
console.log(x, y, z, w); // 4 5 6 20
```
#### Nested Destructuring
```
var a1 = [1, [2, 3, 4], 5];
var o1 = {x: {y: {z: 6}}};
var [a,[b,c,d],e]=a1;
var {x:{y:{z:w}}}=o1;
console.log(a, b, c, d, e);
console.log(w);
// 1 2 3 4 5 
// 6
```
#### Destructuring Defaults + Parameter Defaults
```
function f6({x = 10}={}, {y}={y: 10}) {
    console.log(x, y);
}
f6(); // 10 10
f6(undefined, undefined); // 10 10
f6({}, undefined); // 10 10
f6({}, {}); // 10 undefined
f6(undefined, {}); // 10 undefined
f6({x: 2}, {y: 3}); // 2 3
```
#### Restructuring
Below is wrong as it is just a shallow clone, would clone more than 1 level.
```
config = Object.assign( {}, defaults, config );
```
A bit longer but work
```
// merge `defaults` into `config`
{
    let {
        // destructure (with default value assignments)
        options: {
            remove = defaults.options.remove,
            enable = defaults.options.enable,
            instance = defaults.options.instance
        }={},
        log: {
            warn = defaults.log.warn,
            error = defaults.log.error
        }={}
    } = config;
    // restructure
    config = {
        options: {remove, enable, instance},
        log: {warn, error}
    };
}
```
