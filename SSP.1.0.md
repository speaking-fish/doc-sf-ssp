
# Simple Serialization Protocol

## Goal

Simplest binary JSON-compatible protocol.
Simpler then CBOR and BJSON.

## Types

int: int8, int16, int32, int64
float: float32, double64
decimal: extra large decimal numbers
binary: byte array
string: string(utf8)
array: array of any type
object: map key: string value: any type

## Very simple subset way

| Restriction                 | Reason                                               | How to                   |
| --------------------------- | ---------------------------------------------------- | ------------------------ |
| No null type                | It's weird to serialize nothing                      | Do not serialize nothing |
| No boolean type             | Not all languages has boolean type: C, some SQL, etc | Use int (0 = false, 1 = true) |
| No unsigned int types       | Not all languages has unsigned types: Java           | Use signed               |
| No date/time/duration types | No uniform binary date/time/duration format        | Use strings and [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601 "https://en.wikipedia.org/wiki/ISO_8601") |
| Only one binary data type   | Not all languages has BLOB type variations           | |
| Only one type for large decimal numbers | Yes, we save your money.               | |
| No serialization optimizations | Simply implementation | Use CBOR for better serialization |


## Binary representation

### Common

	TYPE (1 byte) TYPE_DATA (count bytes depends on type)
  
### Type id

	TYPE_OBJECT      0
	TYPE_ARRAY       1
	TYPE_STRING      2
	TYPE_DECIMAL     4
	TYPE_INT_8      10
	TYPE_INT_16     11
	TYPE_INT_32     12
	TYPE_INT_64     13
	TYPE_FLOAT_32   15
	TYPE_FLOAT_64   16
	TYPE_BYTE_ARRAY 20

### Primitive types

#### Integer types

Integer types serialized from hi-order-bye to lo-order-byte
  
	TYPE_INT_8  BYTE_VALUE
	TYPE_INT_16 BYTE_VALUE_HI, BYTE_VALUE_LO
	TYPE_INT_32 BYTE_VALUE_HI, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE_LO
	TYPE_INT_64 BYTE_VALUE_HI, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE_LO

#### Float-point types

Float-point types serialized like integer type of same size
  
	TYPE_FLOAT_32 BYTE_VALUE_HI, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE_LO
	TYPE_FLOAT_64 BYTE_VALUE_HI, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE, BYTE_VALUE_LO

### Variable size types

	TYPE TYPE_INT* TYPE_INT_DATA (count) TYPE_DATA[count]

#### String

String uses UTF-8 encoding
  
    TYPE_STRING TYPE_INT* TYPE_INT_DATA (count) TYPE_DATA[count]

#### Big decimal
  
Same as string, but data contains string representation of number
    
    TYPE_DECIMAL TYPE_INT* TYPE_INT_DATA (count) TYPE_DATA[count]

#### Byte array
  
    TYPE_BYTE_ARRAY TYPE_INT* TYPE_INT_DATA (count) TYPE_DATA[count]

### Structural types

#### Object

Object header contains field count

	TYPE_OBJECT TYPE_INT* TYPE_INT_DATA (count)
  
Each object entry contains string key

	TYPE_STRING TYPE* TYPE_DATA

and value of any type

	TYPE (1 byte) TYPE_DATA (count bytes depends on type)
    
#### Array

Array header contains element count

	TYPE_ARRAY TYPE_INT* TYPE_INT_DATA

Each element is value of any type
  
	TYPE* TYPE_DATA
    