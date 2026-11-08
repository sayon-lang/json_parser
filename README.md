# json_parser

JSON the way configuration files and APIs actually use it, for [Sayon](https://github.com/sayon-lang/sayon), written in Sayon. `standard/json.sayon` reads and writes strict JSON. This package adds the rest: JSON with comments and trailing commas, JSON Pointer for finding values, and JSON Patch and JSON Merge Patch for changing documents.

## Using it

Add it to your project's `sayon.toml` and run `sayon install`:

```toml
[dependencies]
json_parser = "0.1"
```

Then import the modules you need through the dependency:

```sayon
json = import("standard/json.sayon")
pointer = import("dependencies/json_parser/src/pointer.sayon")
relaxed = import("dependencies/json_parser/src/relaxed.sayon")

settings = relaxed.read("settings.jsonc")
port = pointer.get(settings, "/server/port")
if port != nothing
    print(json.encode(port))
```

## What it has

| Module | What it does |
| --- | --- |
| `src/relaxed.sayon` | `parse` and `read` take JSON with `//` and `/* */` comments and trailing commas, as VS Code's settings write it; `strict` turns such text into strict JSON. Errors name the line and column of the text as written. |
| `src/pointer.sayon` | JSON Pointer ([RFC 6901](https://www.rfc-editor.org/rfc/rfc6901)): `get` and `has` find a value, `add`, `replace`, and `remove` give the changed document, and `parts`, `from_parts`, and `escape` read and write pointers. |
| `src/patch.sayon` | JSON Patch ([RFC 6902](https://www.rfc-editor.org/rfc/rfc6902)): `apply` makes `add`, `remove`, `replace`, `move`, `copy`, and `test` operations, whole or not at all; JSON Merge Patch ([RFC 7396](https://www.rfc-editor.org/rfc/rfc7396)): `merge`; and `equal`, which compares values as both do, numbers by their value and objects whatever their members' order. |

Values are `standard/json.sayon`'s `Json`, so they go straight to `json.encode`, `json.pretty`, and `json.from_json`.

## Examples

`sayon run` in this folder takes a tour of the package. The other examples run with `sayon run examples/<name>.sayon`:

- `settings.sayon` reads `settings.jsonc`, a settings file with comments, finds three settings, and patches it.
- `changes.sayon` changes an order with a JSON Patch, shows a failed `test` leaving it as it was, and merges a change into it.

## Tests

`sayon test` runs the tests in `src` and in `tests`, where `rfc.sayon` checks the examples of RFC 6901, RFC 6902, and RFC 7396.

## License

[Apache License 2.0](LICENSE).
