[English](README.md) | 中文

# FlatJSON

在 Scratch 中扁平化解析与字符串化 JSON 。

使用 TurboWarp 编辑器打开:  
https://turbowarp.org/editor?project_title=FlatJSON&project_url=bddjr.github.io/FlatJSON/FlatJSON.sb3

Scratch 项目链接:  
https://scratch.mit.edu/projects/1384506526/

---

## 规则

JSON 语法参考: https://www.json.org/json-zh.html

key:
- 空键表示根，非空键必须以 `/` 开头。
- 多层级的键用 `/` 分隔，例如 `/a/b/c` 。
- `/` 会被转义为 `\/` ， `\` 会被转义为 `\\` 。
- 如果有字符是控制字符 (U+0000 - U+001F) ，使用转义格式写法，例如 `\b\t\n\f\r\u001f` 。
- 建议不使用大写字母，因为 Scratch 判断字符串时会先转成小写再判断。

type:
- `object`
- `array`
- `string`
- `number`
- `boolean`
- `null`
- `rawjson` (仅字符串化)

value:
- 当 type 是 `array` 时，value 被用于保存数组的长度。
- `\b` 会被解析成 `�` (U+FFFD) ，因为 [scratch-parser](https://github.com/scratchfoundation/scratch-parser) 会移除 Scratch 作品的 `project.json` 里的所有 `\b` 。  
  旧版 scratch-parser 甚至会把 `"\\b"` 变成 `"\"` ，导致作品加载失败。

以 `FlatJSON/internal.` 开头的变量、列表、自制积木是 FlatJSON 内部用的。  
不要擅自调用或修改它们，除非你清楚自己在干什么。  

不要在同一个角色里并行执行多个 FlatJSON 自制积木。  
如果需要，请使用克隆体。  

返回的错误在 `FlatJSON.error` 的第 1 项。  
如果没有这一项，则没有发生错误。

---

## 解析

自制积木: `FlatJSON.parse`

输入变量:
- `FlatJSON.parse.input`  
  待解析的 JSON 。

输出列表:
- `FlatJSON.parse.output.key`  
  键列表，格式如“规则”所述。
- `FlatJSON.parse.output.key.basename`  
  键的基名列表，例如 `/a/b/c` 里的基名是 `c` 。
- `FlatJSON.parse.output.key.parent`  
  键的父键列表，例如 `/a/b/c` 里的父键是 `/a/b` 。  
  空键的父键是空键。
- `FlatJSON.parse.output.key.prefix`  
  键的前缀列表，例如 `/a/b/c` 里的前缀是 `/a/b/` 。
- `FlatJSON.parse.output.type`  
  类型列表，可能的类型如“规则”所述。
- `FlatJSON.parse.output.value`  
  值列表。  
  如果类型是 `object` 或 `null` 则值是空字符串。  
  如果类型是 `array` 则值是数组长度。

错误:
- `parse: Unexpected end of JSON input`
- `parse: Unexpected token '�' at position �`
- `parse: Expected double-quoted property name in JSON at position �`
- `parse: Bad control character in string literal in JSON at position �`
- `parse: Bad escaped character in JSON at position �`
- `parse: Bad Unicode escape in JSON at position �`
- `parse: Expected ':' after property name in JSON at position �`
- `parse: Expected ',' or '}' after property value in JSON at position �`
- `parse: Expected ',' or ']' after array element in JSON at position �`
- `parse: Unexpected number in JSON at position �`
- `parse: Unexpected number '�' in JSON`

---

## 字符串化

自制积木: `FlatJSON.stringify`

输入变量:
- `FlatJSON.stringify.input.root_key`  
  指定从哪个根键开始字符串化。
- `FlatJSON.stringify.input.space`  
  指定缩进字符串，例如两个空格。

输入列表:
- `FlatJSON.stringify.input.key`  
  键列表。
- `FlatJSON.stringify.input.type`  
  类型列表。
- `FlatJSON.stringify.input.value`  
  值列表。

输出变量:
- `FlatJSON.stringify.output`  
  输出的 JSON 。

错误:
- `stringify: Expected input key, type, and value lists to have the same length`
- `stringify: Expected non-empty input`
- `stringify: Expected input key list to contain the root key`
- `stringify: Expected non-empty key to start with '/' at position �`
- `stringify: Unexpected end of key input at position �`
- `stringify: Bad escaped character of key input at position �:�`
- `stringify: Bad Unicode escape of key input at position �:�`
- `stringify: Unexpected type '�' at position �`

该积木会检查输入的 `number` 是否有效，无效则变成 `null` 。

该积木不会检查输入的 `rawjson` 是否符合 JSON 规范，你需要自己保证输入的 JSON 合规。

---

## copy_parse_output_to_stringify_input

自制积木: `FlatJSON.copy_parse_output_to_stringify_input`

该积木会执行以下操作:
- 复制 `FlatJSON.parse.output.key` 到 `FlatJSON.stringify.input.key`
- 复制 `FlatJSON.parse.output.type` 到 `FlatJSON.stringify.input.type`
- 复制 `FlatJSON.parse.output.value` 到 `FlatJSON.stringify.input.value`

---

## clear

自制积木: `FlatJSON.clear`

清空 `FlatJSON.error` ，然后自动调用以下自制积木：
- `FlatJSON.clear_temporary_variables`
- `FlatJSON.clear_parse_input`
- `FlatJSON.clear_parse_output`
- `FlatJSON.clear_stringify_input`
- `FlatJSON.clear_stringify_output`

---

## clear_temporary_variables

自制积木: `FlatJSON.clear_temporary_variables`

清空 `FlatJSON/internal.temp.` 开头的所有变量和列表。

---

## clear_parse_input

自制积木: `FlatJSON.clear_parse_input`

清空 `FlatJSON.parse.input` 变量。

---

## clear_parse_output

自制积木: `FlatJSON.clear_parse_output`

清空 `FlatJSON.parse.output.` 开头的所有列表。

---

## clear_stringify_input

自制积木: `FlatJSON.clear_stringify_input`

清空 `FlatJSON.stringify.input.` 开头的所有变量和列表。

---

## clear_stringify_input_lists

自制积木: `FlatJSON.clear_stringify_input_lists`

清空 `FlatJSON.stringify.input.` 开头的所有列表。

---

## clear_stringify_output

自制积木: `FlatJSON.clear_stringify_output`

清空 `FlatJSON.stringify.output` 变量。

---

## 许可证

该作品使用 [Unlicense](https://unlicense.org) 发布到公共领域。

“Scratch” 名称是 Scratch 基金会的商标。
