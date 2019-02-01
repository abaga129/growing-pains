# Typescript Gotchas

## Casting
Typing is a thin sheet of ice over the unforgiving cold javascript waters below. Tread carefully.
Casting in typescript does not work the way that it does in most statically typed languages.  Dont be fooled.

Lets look at an example

```ts
class Foo
{
  a: number;
  b: string;
}

class Bar
{
  c: number;
}

let fooBar: Foo & Bar = {
    a: 10,
    b: "Hello intersection",
    c: 12.1255
}

let foo = <Foo>fooBar;
let bar = <Bar>fooBar;
```

If you compile the above code to javascript, we will get the following
```js
var Foo = /** @class */ (function () {
    function Foo() {
    }
    return Foo;
}());
var Bar = /** @class */ (function () {
    function Bar() {
    }
    return Bar;
}());
var fooBar = {
    a: 10,
    b: "Hello intersection",
    c: 12.1255
};

var foo = fooBar;
var bar = fooBar;
```

Something about the compiled code is troubling.  
```js
var foo = fooBar;
var bar = fooBar;
```

Whats going on here? Objects arent casted like in most languages. Type casting in typescript simply checks that the object being casted has the fields of the interface or class being casted to.  `foo` and `bar` will still have all of the fields of `fooBar`.

What if we dont want the new object to have all of these extra fields?  There is a workaround using a copy constructor.

```ts
class Foo
{
  a: number;
  b: string;
  constructor(foo: Foo)
  {
      this.a = foo.a;
      this.b = foo.b;    
  }
}

class Bar
{
    c: number;
    constructor(bar: Bar) {
        this.c = bar.c;
    }
}

let fooBar: Foo & Bar = {
    a: 10,
    b: "Hello intersection",
    c: 12.1255
}

let foo = new Foo(fooBar)
let bar = new Foo(fooBar);

console.log(foo)
console.log(bar)
console.log(fooBar)
```

Essentially this lets us create a new object and only assign the values we want from `fooBar` to our new object.  Since `fooBar` is a `Foo` and a `Bar` we the constructors dont complain.