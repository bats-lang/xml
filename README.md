# xml

Cursor-based XML/HTML reader. Parsing runs on the host (via DOMParser);
the result is a compact binary SAX buffer you walk with a cursor.

## Opcodes

| Opcode | Value | Description |
|--------|-------|-------------|
| ELEMENT_OPEN | 1 | Opening tag with attributes |
| ELEMENT_CLOSE | 2 | Closing tag |
| TEXT | 3 | Text node |

## API

```
#use wasm.bats-packages.dev/xml as X
#use array as A

(* Parse untrusted HTML via the host.
   Returns the SAX buffer length, or 0 on failure. *)
$X.parse_html{lb:agz}{n:nat}
  (html: !A.borrow(byte, lb, n), len: int n) : int

(* Pull the SAX buffer into a fresh array.
   Call once after parse_html returns len > 0. *)
$X.get_result{len:pos}(len: int len) : [l:agz] A.arr(byte, l, len)

(* Read the opcode byte at a cursor position *)
$X.opcode{lb:agz}{n:nat}
  (buf: !A.borrow(byte, lb, n), pos: int) : int

(* Read an element-open record.
   Returns (tag_offset, tag_length, attribute_count, next_position). *)
$X.element_open{lb:agz}{n:nat}
  (buf: !A.borrow(byte, lb, n), pos: int, len: int n)
  : @(int, int, int, int)

(* Read one attribute from the current position.
   Returns (name_offset, name_length, value_offset, value_length, next_position). *)
$X.read_attr{lb:agz}{n:nat}
  (buf: !A.borrow(byte, lb, n), pos: int, len: int n)
  : @(int, int, int, int, int)

(* Read a text node.
   Returns (text_offset, text_length, next_position). *)
$X.read_text{lb:agz}{n:nat}
  (buf: !A.borrow(byte, lb, n), pos: int, len: int n)
  : @(int, int, int)
```

## Dependencies

- **array**
