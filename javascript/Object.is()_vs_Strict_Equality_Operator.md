# `Object.is()` vs. Strict Equality Operator in JavaScript

> **Reference:**
> <https://dmitripavlutin.com/object-is-vs-strict-equality-operator/>
>
> <cite>&mdash; Blog post by Front End Developer, Dmitri Pavlutin</cite>

## General Use

```javascript
1 === 1; // true
1 === true; // false

Object.is(1, 1) // true
Object.is(1, true) // false
```

## When To Use Which

### Strict Equality Check Operator

* `true` if _primatives_ are same type **and** same value
* no type coercion
  * `undefined === undefined` is true
  * `null === undefined` is false
* `false` if _objects_ have same properties and values
  * **unless** object is the exact same object
  * `myObject === myObject` is true

**NOTE:** The above scenarios work with `Object.is()`

### `Object.is()` Method

The difference between each type of equality check is how `Nan` and `-0` are treated.

* `Nan === Nan` is false but `Object.is(Nan, Nan)` is true
* `-0 === +0` is true but `Object.is(-0, +0)` is false

Also, `Number.isNan(Nan)` is using the same algorithm as `Object.is(Nan, Nan)`
