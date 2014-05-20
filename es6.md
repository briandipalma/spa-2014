arrow functions
---------------

* useful for functional style programming

```javascript

    [1, 2, 3, 4, 5].filter( (x) => x % 2 === 0 )
```

let and const
-------------

* makes variables less confusing
* 'let' is the new var

```javascript

    // block scoped let
    let a = 2;
    {
        let a = 3;
    }
    console.log(a); // 2

    // const
    const NUMBER_OF_COMMANDMENTS = 10
    NUMBER_OF_COMMANDMENTS = 11 // in strict mode this is a type error, in nonstrict mode it silently fails
    console.log(NUMBER_OF_COMMANDMENTS) // 10
```

parameter changes
-----------------

* stop using 'arguments'
* get rid of defaulting blocks of code

```javascript

    function example(y, x = 3, ...rest) {
        // x has a default value
        // rest is an array
        // you can turn an array into function call arguments too.
        console.log(Math.max(...rest))
        console.log(new Date(...[2014, 5, 3]))
    }
```

classes
-------

* stop religious arguments

```javascript

    class MyClass {
        constructor() {
            this.own = true;
        }
    }
```

