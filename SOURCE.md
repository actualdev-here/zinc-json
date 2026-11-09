# Source and compatibility

Version: 0.1.0 (experimental).

Exported from [Zinc commit 6baeeb0025b264055766ede56cdd8246d3532891](https://github.com/actualdev-here/Zinc/commit/6baeeb0025b264055766ede56cdd8246d3532891),
directory `packages/zinc-json`. Use a compiler built from that snapshot.
This repository distributes library source, not the Zinc compiler.

Tests live in the source snapshot: `sh tests/discord_package.sh` includes
306 JSON differential/Unicode/malformed/resource-limit cases. This package
requires no native network runtime. See README for allocation/parser limits.

License: MIT, see LICENSE.
