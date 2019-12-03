# Nat
[![Build Status][circleci-svg]][circleci-url]
[![dependency status][deps-svg]][deps-url]
[![dev dependency status][dev-deps-svg]][dev-deps-url]
[![License][license-image]][license-url]

`Nat(value)` returns its argument if it represents a non-negative
integer (i.e. a "natural number") that can be accurately represented
as a JavaScript
[`BigInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt),
a built-in object that can be used to represent arbitrarily large integers.
If the argument is not a `BigInt` or the argument is negative, an exception is thrown. This makes
it easy to use `Nat()` on incoming arguments, or as an assertion on generated
values.

You can think of `Nat()` as a type enforcement.

## How to use

`Nat()` can be used to enforce desired properties on account balances, where precision is important.

For instance, in a deposit scenario, you would want to defend against someone "depositing" a negative value. Use `Nat` to validate the amount to be deposited before proceeding:

```
deposit: function(amount) {
  amount = Nat(amount);
  ...
}
```

Any addition or subtraction expressions dealing with monetary amounts should protected with `Nat()` to guard against overflow/underflow errors. Without this check, the two balances might both be safe, but their sum might be too large to represent accurately, causing precision errors in subsequent computation:

```
Nat(myOldBal + amount);
const srcNewBal = Nat(srcOldBal - amount);
```

## Non-monetary usage

Array indexes can be wrapped with `Nat()`, to guard against the surprising string coercion of non-integral index values:

```
const a = [2,4,6]
function add(index, value) {
  a[Nat(index)] = value;
}
add(3, 8); // works
add(2.5, 7); // throws rather than add a key named "2.5"
```

Nat can be used even in cases where it is not strictly necessary, for extra protection against human error.

## Bounds

Because `Nat` uses JavaScript's upcoming [`BigInt` standard](https://tc39.github.io/proposal-bigint/), the range of accurately-representable integers is effectively unbounded.

## History

Nat comes from the Google Caja project, which tested whether a number was a primitive integer within the range of continguously representable non-negative integers.

For more, see the [discussion in TC39 notes](https://github.com/rwaldron/tc39-notes/blob/master/es6/2013-07/july-25.md#59-semantics-and-bounds-of-numberisinteger-and-numbermax_integer)


[circleci-svg]: https://circleci.com/gh/Agoric/nat.svg?style=svg
[circleci-url]: https://circleci.com/gh/Agoric/nat
[deps-svg]: https://david-dm.org/Agoric/Nat.svg
[deps-url]: https://david-dm.org/Agoric/Nat
[dev-deps-svg]: https://david-dm.org/Agoric/Nat/dev-status.svg
[dev-deps-url]: https://david-dm.org/Agoric/Nat?type=dev
[license-image]: https://img.shields.io/badge/License-Apache%202.0-blue.svg
[license-url]: LICENSE
