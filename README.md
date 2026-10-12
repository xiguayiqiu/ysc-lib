# YScript standard-library enhancements

These modules are written entirely in YScript and build higher-level operations
on top of all 43 built-in standard-library namespaces. Each function documents
its purpose, inputs, output, and relevant boundary or runtime constraints in a
named documentation block:

```yscript
@function_name HEAD
# Description, parameters, return value, and relevant behavior.
@function_name END
```

Use plain `#` comments inside the block; do not put `@description`, `@param`,
`@returns`, or `@details` directives inside it.

Flexible helpers include callback-driven list mapping/filtering/reduction,
format-dispatched encoding and hashing, configurable HTTP requests, and
callback-based text processing. Optional convenience behavior uses explicit
defaults or fallbacks; unknown format/algorithm names and invalid bounds report
errors rather than silently selecting another operation.

Modules are grouped by function. Import a module with its category in the path,
for example `import ["lib/text/string_utils" as string_utils]`.

For example:

```yscript
import ["lib/text/string_utils" as string_utils, "lib/network/net_utils" as net_utils]
import ["lib/collections/array_utils" as array_utils, "lib/network/http_utils" as http_utils]

let reversed = string_utils.reverse("YScript")
let open_ports = net_utils.scan_tcp("127.0.0.1", [22, 80, 443], 0.5)
let doubled = array_utils.array_utils_map([1, 2, 3], func(value) { return value * 2 })
let response = http_utils.http_utils_request({
    "url": "https://example.com/api",
    "method": "GET",
    "headers": {"Accept": "application/json"},
    "timeout": 5,
})
```

| Category | Built-in namespace | YScript module |
| --- | --- | --- |
| Collections | `array` | `collections/array_utils.ys` |
| Collections | `iter` | `collections/iter_utils.ys` |
| Math | Pure YScript | `math/math_utils.ys` |
| Text | `string` | `text/string_utils.ys` |
| Text | `regex` | `text/regex_utils.ys` |
| Text | `encoding` | `text/encoding_utils.ys` |
| Binary | `binary` | `binary/binary_utils.ys` |
| Binary | `compress` | `binary/compress_utils.ys` |
| Data formats | `csv` | `data/csv_utils.ys` |
| Data formats | `from` | `data/from_utils.ys` |
| Data formats | `ini` | `data/ini_utils.ys` |
| Data formats | `json` | `data/json_utils.ys` |
| Data formats | `toml` | `data/toml_utils.ys` |
| Data formats | `xml` | `data/xml_utils.ys` |
| Data formats | `yaml` | `data/yaml_utils.ys` |
| Filesystem | `io` | `filesystem/io_utils.ys` |
| Filesystem | `os` | `filesystem/os_utils.ys` |
| Filesystem | `path` | `filesystem/path_utils.ys` |
| Filesystem | `sys` | `filesystem/sys_utils.ys` |
| Network | `http` | `network/http_utils.ys` |
| Network | `net` | `network/net_utils.ys` |
| Network | `raw` | `network/raw_utils.ys` |
| Network | `socket` | `network/socket_utils.ys` |
| Network | `ssl` | `network/ssl_utils.ys` |
| Network | `url` | `network/url_utils.ys` |
| Security | `aes` | `security/aes_utils.ys` |
| Security | `crypto` | `security/crypto_utils.ys` |
| Security | `rsa` | `security/rsa_utils.ys` |
| Runtime | `errors` | `runtime/errors_utils.ys` |
| Runtime | `load` | `runtime/load_utils.ys` |
| Runtime | `reflect` | `runtime/reflect_utils.ys` |
| Concurrency | `sync` | `concurrency/sync_utils.ys` |
| Concurrency | `thread` | `concurrency/thread_utils.ys` |
| Terminal | `cli` | `terminal/cli_utils.ys` |
| Terminal | `color` | `terminal/color_utils.ys` |
| Terminal | `log` | `terminal/log_utils.ys` |
| Terminal | `stdio` | `terminal/stdio_utils.ys` |
| Terminal | `term` | Built-in Go namespace; see the YScript terminal standard-library guide |
| Terminal | `ystui` [实验性功能] [utils] | `terminal/ystui.ys` |
| Utilities | `rand` | `utilities/rand_utils.ys` |
| Utilities | `time` | `utilities/time_utils.ys` |
| Platform | `c` | `platform/c_utils.ys` |
| Platform | `cuda` | `platform/cuda_utils.ys` |
| Platform | `ffi` | `platform/ffi_utils.ys` |
| Platform | `lstd` | `platform/lstd_utils.ys` |
| Platform | `ocr` | `platform/ocr_utils.ys` |
| GUI（实验性测试） [utils] | `ysc-gui` host bindings | `gui/yui.ys` |

These are convenience/composition helpers, not replacements for the built-in
APIs. Platform-dependent or privileged features (for example CUDA, raw packet
capture, FFI, and OCR) retain the built-in runtime's availability and permission
requirements.

The pure-YScript `math/math_utils.ys` module provides the common math API,
including range operations, rounding, real powers and roots, number theory,
statistics, trigonometry, logarithms, hyperbolic functions, overflow-checked
integer arithmetic, and floating-point helpers. It does not rely on a runtime
`math` namespace.
The module API, semantics, edge cases, and limitations are documented in
[`64_math数学库`](../../doc/YScript.wiki/64_math数学库.md) [utils]. Using it requires the YScript extension library.

**[实验性测试] [utils]** The `gui/yui.ys` module is a pure-YScript facade for the
Ebiten-backed desktop host. It provides a JFrame-style window and component API
(`JFrame`, `JButton`, `JLabel`, `add`, `show`) as well as lower-level drawing and
input APIs. Importing `gui/yui` directly enables the GUI host in `ysc`; GUI
behavior and interfaces may change while the module remains experimental.

Helpers delegate to the matching built-in namespace for primitive operations.
Unless a function explicitly documents a fallback result, errors from those
operations are not converted into success-shaped values. Import declarations
merge helper symbols into the script, so helper function names are kept unique
across modules.

**[实验性功能] [utils]** `terminal/ystui.ys` is a pure-YScript terminal UI built on `term`.
It provides composable row/column/grid/split/page layouts, display and form
controls, lists/tables/tree rows, navigation/status components, progress and
modal feedback, keyboard/mouse input, themed ANSI rendering, and a
raw-mode/alternate-screen runner with diff rendering and terminal restoration.
`run()` requires an interactive TTY with cursor and alternate-screen support;
deterministic creation, rendering, and event handling can also be used in
non-interactive scripts. Code editors, charts, process monitoring, and real
filesystem browsers remain out of scope. See
[`63_ystui终端UI`](../../doc/YScript.wiki/63_ystui终端UI.md) for the API and limitations; the guide and all exposed interfaces are [实验性功能] [utils].
The module and its interfaces are marked [实验性功能] [utils]; using them requires installing the YScript extension library.

The [实验性功能] import and behavior smoke tests are `test/lib_utils_smoke.ys`,
`test/ystui_smoke.ys`, and `test/ystui_components_smoke.ys`; run them from the
YScript source checkout with `ysc ysc-model/test/lib_utils_smoke.ys`,
`ysc ysc-model/test/ystui_smoke.ys`, and
`ysc ysc-model/test/ystui_components_smoke.ys`.
Run the math library smoke test with `ysc ysc-model/test/math_utils_smoke.ys`.
