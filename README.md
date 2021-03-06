native-buffer-browserify
===============

The buffer module from [node.js](http://nodejs.org/), but for browsers. This is
a fork of [buffer-browserify](https://github.com/toots/buffer-browserify).

[![build status](https://secure.travis-ci.org/feross/native-buffer-browserify.png)](http://travis-ci.org/feross/native-buffer-browserify)

[![testling badge](https://ci.testling.com/feross/native-buffer-browserify.png)](https://ci.testling.com/feross/native-buffer-browserify)

When you `require('buffer')` in
[browserify](http://github.com/substack/node-browserify), this module will be
loaded.

It will also be loaded if you use the global `Buffer` variable.

## Features

- Backed by `ArrayBuffer` (not `Object`, so it's fast)
- Preserves Node API exactly
- Faster pretty much across the board (see perf results below)
- `.slice()` returns instances of the same type
- Square-bracket `buf[4]` notation works!
- Does not modify any browser prototypes.
- All tests from the original `buffer-browserify` project pass.
- Requires browsers to have `Uint8Array` and `DataView` support (all modern
  browsers, IE10+, shim for older browsers included)

## How does it work?

The `Buffer` constructor returns instances of `Uint8Array` that are augmented with function properties for all the Buffer API functions. We use `Uint8Array` so that square bracket notation works as expected -- it returns a single octet.

By augmenting the instances, we can avoid modifying the Uint8Array prototype.



## Performance

See perf tests in `/perf`.

```
# Chrome 33

NewBuffer#bracket-notation x 11,194,815 ops/sec ±1.73% (64 runs sampled)
OldBuffer#bracket-notation x 9,546,694 ops/sec ±0.76% (67 runs sampled)
Fastest is NewBuffer#bracket-notation

NewBuffer#concat x 949,714 ops/sec ±2.48% (63 runs sampled)
OldBuffer#concat x 634,906 ops/sec ±0.42% (68 runs sampled)
Fastest is NewBuffer#concat

NewBuffer#copy x 15,436,458 ops/sec ±1.74% (67 runs sampled)
OldBuffer#copy x 3,990,346 ops/sec ±0.42% (68 runs sampled)
Fastest is NewBuffer#copy

NewBuffer#readDoubleBE x 1,132,954 ops/sec ±2.36% (65 runs sampled)
OldBuffer#readDoubleBE x 846,337 ops/sec ±0.58% (68 runs sampled)
Fastest is NewBuffer#readDoubleBE

NewBuffer#new x 1,419,300 ops/sec ±3.50% (66 runs sampled)
Uint8Array#new x 3,898,573 ops/sec ±0.88% (67 runs sampled) (used internally by NewBuffer)
OldBuffer#new x 2,284,568 ops/sec ±0.57% (67 runs sampled)
Fastest is Uint8Array#new

NewBuffer#readFloatBE x 1,203,763 ops/sec ±1.81% (68 runs sampled)
OldBuffer#readFloatBE x 954,923 ops/sec ±0.66% (70 runs sampled)
Fastest is NewBuffer#readFloatBE

NewBuffer#readUInt32LE x 750,341 ops/sec ±1.70% (66 runs sampled)
OldBuffer#readUInt32LE x 1,408,478 ops/sec ±0.60% (68 runs sampled)
Fastest is OldBuffer#readUInt32LE

NewBuffer#slice x 1,802,870 ops/sec ±1.87% (64 runs sampled)
OldBuffer#slice x 1,725,928 ops/sec ±0.74% (68 runs sampled)
Fastest is NewBuffer#slice

NewBuffer#writeFloatBE x 830,407 ops/sec ±3.09% (66 runs sampled)
OldBuffer#writeFloatBE x 508,446 ops/sec ±0.49% (69 runs sampled)
Fastest is NewBuffer#writeFloatBE

# Node 0.11

NewBuffer#bracket-notation x 10,912,085 ops/sec ±0.89% (92 runs sampled)
OldBuffer#bracket-notation x 9,051,638 ops/sec ±0.84% (92 runs sampled)
Buffer#bracket-notation x 10,721,608 ops/sec ±0.63% (91 runs sampled)
Fastest is NewBuffer#bracket-notation

NewBuffer#concat x 1,438,825 ops/sec ±1.80% (91 runs sampled)
OldBuffer#concat x 888,614 ops/sec ±2.09% (93 runs sampled)
Buffer#concat x 1,832,307 ops/sec ±1.20% (90 runs sampled)
Fastest is Buffer#concat

NewBuffer#copy x 5,987,167 ops/sec ±0.85% (94 runs sampled)
OldBuffer#copy x 3,892,165 ops/sec ±1.28% (93 runs sampled)
Buffer#copy x 11,208,889 ops/sec ±0.76% (91 runs sampled)
Fastest is Buffer#copy

NewBuffer#readDoubleBE x 1,057,233 ops/sec ±1.28% (88 runs sampled)
OldBuffer#readDoubleBE x 4,094 ops/sec ±1.09% (86 runs sampled)
Buffer#readDoubleBE x 1,587,308 ops/sec ±0.87% (84 runs sampled)
Fastest is Buffer#readDoubleBE

NewBuffer#new x 739,791 ops/sec ±0.89% (89 runs sampled)
Uint8Array#new x 2,745,243 ops/sec ±0.95% (91 runs sampled)
OldBuffer#new x 2,604,537 ops/sec ±0.93% (88 runs sampled)
Buffer#new x 1,836,218 ops/sec ±0.74% (92 runs sampled)
Fastest is Uint8Array#new

NewBuffer#readFloatBE x 1,111,263 ops/sec ±0.41% (97 runs sampled)
OldBuffer#readFloatBE x 4,026 ops/sec ±1.24% (90 runs sampled)
Buffer#readFloatBE x 1,611,800 ops/sec ±0.58% (96 runs sampled)
Fastest is Buffer#readFloatBE

NewBuffer#readUInt32LE x 502,024 ops/sec ±0.59% (94 runs sampled)
OldBuffer#readUInt32LE x 1,259,028 ops/sec ±0.79% (87 runs sampled)
Buffer#readUInt32LE x 2,778,635 ops/sec ±0.46% (97 runs sampled)
Fastest is Buffer#readUInt32LE

NewBuffer#slice x 1,174,908 ops/sec ±1.47% (89 runs sampled)
OldBuffer#slice x 2,396,302 ops/sec ±4.36% (86 runs sampled)
Buffer#slice x 2,994,029 ops/sec ±0.79% (89 runs sampled)
Fastest is Buffer#slice

NewBuffer#writeFloatBE x 721,081 ops/sec ±1.10% (86 runs sampled)
OldBuffer#writeFloatBE x 4,020 ops/sec ±1.04% (92 runs sampled)
Buffer#writeFloatBE x 1,811,134 ops/sec ±0.67% (91 runs sampled)
Fastest is Buffer#writeFloatBE
```

## MIT License

Copyright (C) 2013 [Feross Aboukhadijeh](http://feross.org), Romain Beauxis, and other contributors.
