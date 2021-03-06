# `.bind` is slower

In general terms, `.bind` the context with a `Function` generate need a considerable effort.

Native method is, in general JavaScript Engines slow. Instead, you can use:

- `.call` method (when you need to call the function once).
- `var self = this` is simple and effective.

> Otherwise, exists a set of alternatives, such as libraries that try to implement a better way to do the same. In special, [Lodash](https://lodash.com) implementation is based in bitwise, that is very fast check in JavaScript. Check the [test](https://jsperf.com/bind-vs-jquery-proxy/104) benchmark.

If you are using `.bind` oriented to memoize arguments values that doesn't change around the `Function` calls, like:

```js
function sayHello (day, name) {
  console.log('Hello ' + name + ', have a good ' + day)
}

var day = 'Monday'
var sayMonday = sayHello.bind(null, day)

sayMonday('Kiko')
// => 'Hello Kiko, have a good Monday'
```

The real thing that you need is a *partial function*. The implementation depends of the library that you use but normally it's more faster than `.bind`:

```js
function greet (greeting, name) {
  return greeting + ' ' + name
}

var sayHelloTo = _.partial(greet, 'hello')
sayHelloTo('fred') // ➜ 'hello fred'

// Partially applied with placeholders.
var greetFred = _.partial(greet, _, 'fred')
greetFred('hi') // ➜ 'hi fred'
```
