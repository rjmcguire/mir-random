[![Gitter](https://img.shields.io/gitter/room/libmir/public.svg)](https://gitter.im/libmir/public)
[![Bountysource](https://www.bountysource.com/badge/team?team_id=145399&style=bounties_received)](https://www.bountysource.com/teams/libmir)

[![Circle CI](https://circleci.com/gh/libmir/mir-random.svg?style=svg)](https://circleci.com/gh/libmir/mir-random)
[![Build Status](https://travis-ci.org/libmir/mir-random.svg?branch=master)](https://travis-ci.org/libmir/mir-random)
[![Build status](https://ci.appveyor.com/api/projects/status/f2n4dih5s4c32q7u/branch/master?svg=true)](https://ci.appveyor.com/project/9il/mir-random/branch/master)

[![Dub version](https://img.shields.io/dub/v/mir-random.svg)](http://code.dlang.org/packages/mir-random)
[![Dub downloads](https://img.shields.io/dub/dt/mir-random.svg)](http://code.dlang.org/packages/mir-random)
[![License](https://img.shields.io/dub/l/mir-random.svg)](http://code.dlang.org/packages/mir-random)

# mir-random
Professional Random Number Generators

Documentation: http://docs.random.dlang.io/latest/index.html

```d
import std.range, std.stdio;

import mir.random;
import mir.random.variable: NormalVariable;
import mir.random.algorithm: randomRange;

auto rng = Random(unpredictableSeed);        // Engines are allocated on stack or global
auto sample = rng                            // Engines are passed by reference to algorithms
    .randomRange(NormalVariable!double(0, 1))// Random variables are passed by value
    .take(1000)                              // Fix sample length to 1000 elements (Input Range API)
    .array;                                  // Allocates memory and performs computation

writeln(sample);                             
```

## Comparison with Phobos
 - Does not depend on DRuntime (Better C concept)

##### `random` (new implementation and API)
 - Mir Random `rand!float`/`rand!double`/`rand!real` generates saturated real random numbers in `(-1, 1)`. For example, `rand!real` can produce more then 2^78 unique numbers. In other hand, `std.random.uniform01!real` produces less then `2^31` unique numbers with default Engine.
 - Mir Random fixes Phobos integer underflow bugs.
 - Addition optization was added for enumerated types.

##### `ramdom.variable` (new)
 - Uniform
 - Exponential
 - Gamma
 - Normal
 - Cauchy
 - ...

##### `random.algorithm` (new)
 - Range API adaptors

##### `random.engine.*` (fixed, reworked)
 - `opCall` API instead of range interface is used (similar to C++)
 - No default and copy constructors are allowed for generators.
 - `@RandomEngine` UDA is used for for Engines instead of a enum flag.
 - `unpredictableSeed` has not state, returns `size_t`
 - Any unsigned generators are allowed.
 - `min` proporty was removed. Any integer generator can normalize its minimum down to zero.
 - 64-bit Mt19937 initialization is fixed
 - 64-bit Mt19937 is default for 64-bit targets
 - [WIP] additional Engines, see https://github.com/libmir/mir-random/pulls
