# zinc-json

An experimental, pure Zinc JSON package. No subprocesses or native libraries.

## Install

From a Zinc project with a `zinc.toml` manifest:

```sh
zinc add zinc-json --git https://github.com/actualdev-here/zinc-json.git --tag v0.1.0
zinc install
```

Use a compiler built from the compatible [Zinc source](https://github.com/actualdev-here/Zinc).
The package repository contains library source, not a compiler executable.

```zn
use 'zinc-json' as json

json.Text member = json.member '{"name":"Zinc"}', 'name'
json.Text name = json.text member.value
str payload = json.quote name.value
```

`Text` carries `ok` and `value`; a missing key differs from JSON `null`.
`member` returns raw JSON, not decoded text. `text` decodes a JSON string,
including Unicode escapes and surrogate pairs. `quote` escapes a Zinc string.
`valid` checks a complete document, rejecting trailing input, malformed UTF-8,
invalid numbers, unpaired surrogates and trailing commas. Duplicate object keys
are accepted; `member` selects the first matching decoded key.
`element source, index` returns a zero-based array element as raw JSON (or
`ok = false` if missing). `kind` returns string/object/array/null/boolean/number,
or `invalid`.

`unsigned_int` returns `Integer { ok, value }` and rejects values outside
0..2147483647, fractions and exponents. `unsigned_decimal` validates an unsigned
integer without conversion, preserving larger values as text. Use strings for
Discord snowflakes; never convert those to Zinc `int`.

Limits: at most 524,288 input bytes and 32 nested containers. Raw values are
copied as strings, not stored as a DOM. Lookup validates the whole document each
time; this is not a streaming or high-throughput parser. Escaped NUL is valid
JSON, but `text` returns `ok = false` because Zinc strings cannot represent it.
`quote` escapes control characters but preserves input bytes; callers handling
untrusted bytes must validate the resulting JSON before sending it.
String operations allocate temporary strings under Zinc's current runtime;
long-running memory behavior is not yet qualified for production bot workloads.

From the Zinc checkout, run `sh tests/discord_package.sh`.
The regression harness lives in that source checkout, not the standalone package.

## License

[MIT](LICENSE).
