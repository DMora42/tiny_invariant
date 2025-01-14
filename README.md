## Tiny Invariant

[![Build Status](https://travis-ci.org/alexreardon/tiny-invariant.svg?branch=master)](https://travis-ci.org/alexreardon/tiny-invariant)
[![npm](https://img.shields.io/npm/v/tiny-invariant.svg)](https://www.npmjs.com/package/tiny-invariant) [![dependencies](https://david-dm.org/alexreardon/tiny-invariant.svg)](https://david-dm.org/alexreardon/tiny-invariant)
![types](https://img.shields.io/badge/types-typescript%20%7C%20flow-blueviolet)
[![minzip](https://img.shields.io/bundlephobia/minzip/tiny-invariant.svg)](https://www.npmjs.com/package/tiny-invariant)
[![Downloads per month](https://img.shields.io/npm/dm/tiny-invariant.svg)](https://www.npmjs.com/package/tiny-invariant)

A tiny [`invariant`](https://www.npmjs.com/package/invariant) alternative in Dart.

## What is `invariant`?

An `invariant` function takes a value, and if the value is [falsy](https://github.com/getify/You-Dont-Know-JS/blob/bdbe570600d4e1107d0b131787903ca1c9ec8140/up%20%26%20going/ch2.md#truthy--falsy) then the `invariant` function will throw. If the value is [truthy](https://github.com/getify/You-Dont-Know-JS/blob/bdbe570600d4e1107d0b131787903ca1c9ec8140/up%20%26%20going/ch2.md#truthy--falsy), then the function will not throw.

```dart
import 'package:tiny_invariant/tiny_invariant.dart';

invariant(truthyValue, 'This should not throw!');

invariant(falsyValue, 'This will throw!');
// Error('Invariant violation: This will throw!');
```

You can also provide a function to generate your message, for when your message is expensive to create

```dart
import 'package:tiny_invariant/tiny_invariant.dart';

invariant(value, () => getExpensiveMessage());
```

## Why `tiny_invariant`?

The initial implementation from where I got the idea [`library: invariant`](https://www.npmjs.com/package/invariant) supports passing in arguments to the `invariant` function in a sprintf style `(condition, format, a, b, c, d, e, f)`. It has internal logic to execute the sprintf substitutions. The sprintf logic is not removed in production builds. `tiny_invariant` has dropped all of the sprintf logic. `tiny_invariant` allows you to pass a single string message. 

```dart
invariant(condition, `Hello, ${name} - how are you today?`);
```

## API: `(condition: T, message: String?)`

- `condition` is required and can be anything
- `message` optional `string` or a function that returns a `string` (`() => string`)

## Installation

```sh
dart pub add tiny_invariant
```
## Dropping your `message` for kb savings!

Big idea: you will want your compiler to convert this code:

```dart
invariant(condition, 'My cool message that takes up a lot of kbs');
```

Into this:

```dart
if (!condition) {
  if ('production' !== process.env.NODE_ENV) {
    invariant(false, 'My cool message that takes up a lot of kbs');
  } else {
    invariant(false);
  }
}
```

## Original implementation

- [By alexreardon](https://github.com/alexreardon/tiny-invariant)
