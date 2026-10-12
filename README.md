# YScript 标准库扩展

本仓库提供纯 YScript 编写的扩展模块，在 YScript 内置标准库命名空间之上封装更高层的操作。各函数通过命名文档块说明用途、参数、返回值及相关边界：

```yscript
@function_name HEAD
# 用途、参数、返回值及相关行为说明。
@function_name END
```

文档块内部使用普通 `#` 注释；不要在其中写 `@description`、`@param`、`@returns` 或 `@details` 指令。

## 安装

在已初始化的 YScript 项目根目录运行以下命令获取扩展库：

```sh
ysc mod get https://github.com/xiguayiqiu/ysc-lib.git
```

如果项目尚未初始化，先运行：

```sh
ysc mod init <项目名>
```

`ysc mod get` 会将依赖登记到项目的 `ysc.models` 和 `ysc.sum`。获取一次即可，不需要每次运行脚本时重复执行。

## 导入和使用

模块按用途分类，导入路径包含分类目录和模块名。获取扩展库后，可在脚本中按需导入多个模块：

```yscript
import [
    "lib/text/string_utils" as string_utils,
    "lib/network/net_utils" as net_utils,
    "lib/collections/array_utils" as array_utils,
    "lib/network/http_utils" as http_utils,
]

func main() {
    let reversed = string_utils.reverse("YScript")
    let open_ports = net_utils.scan_tcp("127.0.0.1", [22, 80, 443], 0.5)
    let doubled = array_utils.array_utils_map(
        [1, 2, 3],
        func(value) { return value * 2 },
    )
    let response = http_utils.http_utils_request({
        "url": "https://example.com/api",
        "method": "GET",
        "headers": {"Accept": "application/json"},
        "timeout": 5,
    })

    println(reversed)
    println(open_ports)
    println(doubled)
    println(response)
}
```

## 模块清单

| 分类 | 内置命名空间 | YScript 模块 |
| --- | --- | --- |
| 集合 | `array` | `collections/array_utils.ys` |
| 集合 | `iter` | `collections/iter_utils.ys` |
| 数学 | 纯 YScript | `math/math_utils.ys` |
| 文本 | `string` | `text/string_utils.ys` |
| 文本 | `regex` | `text/regex_utils.ys` |
| 文本 | `encoding` | `text/encoding_utils.ys` |
| 二进制 | `binary` | `binary/binary_utils.ys` |
| 二进制 | `compress` | `binary/compress_utils.ys` |
| 数据格式 | `csv` | `data/csv_utils.ys` |
| 数据格式 | `from` | `data/from_utils.ys` |
| 数据格式 | `ini` | `data/ini_utils.ys` |
| 数据格式 | `json` | `data/json_utils.ys` |
| 数据格式 | `toml` | `data/toml_utils.ys` |
| 数据格式 | `xml` | `data/xml_utils.ys` |
| 数据格式 | `yaml` | `data/yaml_utils.ys` |
| 文件系统 | `io` | `filesystem/io_utils.ys` |
| 文件系统 | `os` | `filesystem/os_utils.ys` |
| 文件系统 | `path` | `filesystem/path_utils.ys` |
| 文件系统 | `sys` | `filesystem/sys_utils.ys` |
| 网络 | `http` | `network/http_utils.ys` |
| 网络 | `net` | `network/net_utils.ys` |
| 网络 | `raw` | `network/raw_utils.ys` |
| 网络 | `socket` | `network/socket_utils.ys` |
| 网络 | `ssl` | `network/ssl_utils.ys` |
| 网络 | `url` | `network/url_utils.ys` |
| 安全 | `aes` | `security/aes_utils.ys` |
| 安全 | `crypto` | `security/crypto_utils.ys` |
| 安全 | `rsa` | `security/rsa_utils.ys` |
| 运行时 | `errors` | `runtime/errors_utils.ys` |
| 运行时 | `load` | `runtime/load_utils.ys` |
| 运行时 | `reflect` | `runtime/reflect_utils.ys` |
| 并发 | `sync` | `concurrency/sync_utils.ys` |
| 并发 | `thread` | `concurrency/thread_utils.ys` |
| 终端 | `cli` | `terminal/cli_utils.ys` |
| 终端 | `color` | `terminal/color_utils.ys` |
| 终端 | `log` | `terminal/log_utils.ys` |
| 终端 | `stdio` | `terminal/stdio_utils.ys` |
| 终端 | `term` | 内置 Go 命名空间；详见 YScript 终端标准库指南 |
| 终端 | `ystui` [实验性功能] [utils] | `terminal/ystui.ys` |
| 工具 | `rand` | `utilities/rand_utils.ys` |
| 工具 | `time` | `utilities/time_utils.ys` |
| 平台 | `c` | `platform/c_utils.ys` |
| 平台 | `cuda` | `platform/cuda_utils.ys` |
| 平台 | `ffi` | `platform/ffi_utils.ys` |
| 平台 | `lstd` | `platform/lstd_utils.ys` |
| 平台 | `ocr` | `platform/ocr_utils.ys` |
| 图形界面 [实验性测试] [utils] | `ysc-gui` 宿主绑定 | `gui/yui.ys` |

这些扩展模块是便利/组合层，不替代内置 API。CUDA、raw 抓包、FFI、OCR 等平台相关或需要权限的功能，仍受运行时可用性与权限策略约束。

纯 YScript 模块 `math/math_utils.ys` 提供区间、取整、实数幂与根、数论、统计、三角与双曲函数、对数、溢出检查整数运算和浮点辅助功能；它不依赖运行时 `math` 命名空间。接口、语义及限制见[数学库文档](../../doc/YScript.wiki/64_math数学库.md)。

## 实验性模块

**[实验性测试] [utils]** `gui/yui.ys` 是 Ebiten 桌面宿主之上的纯 YScript 门面，提供 JFrame 风格窗口和组件 API（`JFrame`、`JButton`、`JLabel`、`add`、`show`），以及底层绘制和输入接口。直接导入 `gui/yui` 会在 `ysc` 中启用 GUI 宿主；接口和行为可能变化。

**[实验性功能] [utils]** `terminal/ystui.ys` 是构建在 `term` 上的纯 YScript TUI，提供可组合布局、显示/表单控件、列表/表格/树、导航与状态组件、进度和弹窗反馈、键鼠输入、主题 ANSI 绘制，以及支持差分渲染和终端恢复的 raw-mode/alternate-screen runner。`run()` 需要支持光标和 alternate screen 的交互式 TTY。代码编辑器、图表、进程监控和真实文件浏览器不在当前范围内。详见 [ystui 终端 UI 文档](../../doc/YScript.wiki/63_ystui终端UI.md)。

## 实现约定

- 辅助函数将基础操作委托给匹配的内置命名空间。
- 除非函数明确说明 fallback，否则底层错误不会被包装成看似成功的结果。
- 格式或算法分派遇到未知名称、无效边界时会明确报错，不会静默选择其他操作。
- YScript 的 `import` 会将辅助函数符号合并进脚本，因此不同模块间的辅助函数名保持唯一。

## 冒烟测试

在 YScript 源码仓库根目录运行：

```sh
ysc ysc-model/test/lib_utils_smoke.ys
ysc ysc-model/test/math_utils_smoke.ys
ysc ysc-model/test/ystui_smoke.ys
ysc ysc-model/test/ystui_components_smoke.ys
```
