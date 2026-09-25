English | [中文](README-zh.md)

# FlatJSON

JSON Flattening Parser and Stringifier in Scratch.

Open with TurboWarp editor:  
https://turbowarp.org/editor?project_title=FlatJSON&project_url=bddjr.github.io/FlatJSON/FlatJSON.sb3

Scratch Project Link:  
https://scratch.mit.edu/projects/1384506526/

---

## Rules

JSON syntax reference: https://www.json.org/json-en.html

key:
- An empty key represents the root; non-empty keys must start with `/`.
- Multi-level keys are separated by `/`, for example `/a/b/c`.
- `/` will be escaped as `\/`, and `\` will be escaped as `\\`.
- If a character is a control character (U+0000 - U+001F), use the escaped format, for example `\b\t\n\f\r\u001f`.
- It is recommended not to use uppercase letters, because Scratch converts strings to lowercase before comparison.

type:
- `object`
- `array`
- `string`
- `number`
- `boolean`
- `null`
- `rawjson` (stringify only)

value:
- When the type is `array`, the value is used to store the length of the array.
- `\b` will be parsed as `�` (U+FFFD), because [scratch-parser](https://github.com/scratchfoundation/scratch-parser) removes all `\b` in a Scratch project's `project.json`.  
  Older versions of scratch-parser would even turn `"\\b"` into `"\"`, causing project loading to fail.

Variables, lists, and custom blocks starting with `FlatJSON/internal.` are for internal use by FlatJSON.  
Do not call or modify them unless you know what you are doing.  

Do not execute multiple FlatJSON custom blocks in parallel within the same sprite.  
If needed, please use clones.  

The returned error is in item 1 of `FlatJSON.error`.  
If this item does not exist, no error occurred.

---

## Parse

Custom block: `FlatJSON.parse`

Input variable:
- `FlatJSON.parse.input`  
  The JSON to be parsed.

Output lists:
- `FlatJSON.parse.output.key`  
  The key list, formatted as described in "Rules".
- `FlatJSON.parse.output.key.basename`  
  The basename list of the keys, for example, the basename in `/a/b/c` is `c`.
- `FlatJSON.parse.output.key.parent`  
  The parent key list, for example, the parent key of `/a/b/c` is `/a/b`.  
  The parent key of an empty key is an empty key.
- `FlatJSON.parse.output.key.prefix`  
  The prefix list of the keys, for example, the prefix in `/a/b/c` is `/a/b/`.
- `FlatJSON.parse.output.type`  
  The type list, possible types are described in "Rules".
- `FlatJSON.parse.output.value`  
  The value list.  
  If the type is `object` or `null`, the value is an empty string.  
  If the type is `array`, the value is the array length.

Errors:
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

## Stringify

Custom block: `FlatJSON.stringify`

Input variables:
- `FlatJSON.stringify.input.root_key`  
  Specifies which root key to start stringifying from.
- `FlatJSON.stringify.input.space`  
  Specifies the indentation string, for example, two spaces.

Input lists:
- `FlatJSON.stringify.input.key`  
  The key list.
- `FlatJSON.stringify.input.type`  
  The type list.
- `FlatJSON.stringify.input.value`  
  The value list.

Output variable:
- `FlatJSON.stringify.output`  
  The output JSON.

Errors:
- `stringify: Expected input key, type, and value lists to have the same length`
- `stringify: Expected non-empty input`
- `stringify: Expected input key list to contain the root key`
- `stringify: Expected non-empty key to start with '/' at position �`
- `stringify: Unexpected end of key input at position �`
- `stringify: Bad escaped character of key input at position �:�`
- `stringify: Bad Unicode escape of key input at position �:�`
- `stringify: Unexpected type '�' at position �`

This block checks whether the input `number` is valid, and turns it into `null` if invalid.

This block does not validate whether the input `rawjson` conforms to the JSON specification; you need to ensure the input JSON is valid yourself.

---

## clear

Custom block: `FlatJSON.clear`

Clears `FlatJSON.error`, then automatically calls the following custom blocks:
- `FlatJSON.clear_temporary_variables`
- `FlatJSON.clear_parse_input`
- `FlatJSON.clear_parse_output`
- `FlatJSON.clear_stringify_input`
- `FlatJSON.clear_stringify_output`

---

## clear_temporary_variables

Custom block: `FlatJSON.clear_temporary_variables`

Clears all variables and lists starting with `FlatJSON/internal.temp.`.

---

## clear_parse_input

Custom block: `FlatJSON.clear_parse_input`

Clears the `FlatJSON.parse.input` variable.

---

## clear_parse_output

Custom block: `FlatJSON.clear_parse_output`

Clears all lists starting with `FlatJSON.parse.output.`.

---

## clear_stringify_input

Custom block: `FlatJSON.clear_stringify_input`

Clears all variables and lists starting with `FlatJSON.stringify.input.`.

---

## clear_stringify_input_lists

Custom block: `FlatJSON.clear_stringify_input_lists`

Clears all lists starting with `FlatJSON.stringify.input.`.

---

## clear_stringify_output

Custom block: `FlatJSON.clear_stringify_output`

Clears the `FlatJSON.stringify.output` variable.

---

## License

This project is released into the public domain under the [Unlicense](https://unlicense.org).

The "Scratch" name is a trademark of the Scratch Foundation.
