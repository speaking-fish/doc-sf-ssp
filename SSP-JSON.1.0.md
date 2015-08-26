# Simple Serialization Protocol - JSON

## JSON representation

### Common

	[name]$TYPE[$[attributes]]: TYPE_DATA
	
### Type tag

	TYPE_STRING     string
	TYPE_DECIMAL    decimal
	TYPE_INT*       int
	TYPE_FLOAT*     float
	TYPE_BYTE_ARRAY bytearray
	
Why tags?

Tags allows to determine types, not represented in standard JSON: NaN, byte array, decimals.

Tags can be optional - when tag is skipped, type determines using standard JSON rules.

Also format spec can be used: "hex" for bytearray (default is base64)

### Integer types

	[name]$int[$[attributes]]: TYPE_DATA
	
where TYPE_DATA is standard json representation of integer or quoted string representation

### Float-point types

	[name]$float[$[attributes]]: TYPE_DATA

where TYPE_DATA is standard json representation of float or quoted string representation

quoted string representation used for NaN-s, because standard json doesn't allow NaN-s

### String

	[name]$string[$[attributes]]: TYPE_DATA

where TYPE_DATA is quoted string

### Big decimal
  
	[name]$decimal[$[attributes]]: TYPE_DATA

where TYPE_DATA is quoted string representation of big decimal

### Byte array
  
	[name]$bytearray[$[attributes]]: TYPE_DATA

where TYPE_DATA is base64 representation of byte array in quoted string

if attribute "hex" specified - hex representation of byte array in quoted string

### Object

Object represented as standard json object.

	{
	<object entries>
	}

Each object entry contains any type with specified name

	name$TYPE[$[attributes]]: TYPE_DATA
    
### Array

Array represented as standard json array.

	[
	<array elements>
	]

Element of non-primitive type represented as is: array or object

Element of primitive type wrapped to object with single element:

	{ $TYPE[$[attributes]]: TYPE_DATA }
	
or as is, and their types was detected by JSON rules.


## Example

	{
		a$int: -1,
		b$float: 0.3,
		c$string: "string",
		d$bytearray$hex: "313233",
		e$decimal: "1234567",
		f: [{$int: 3}, {$float: 0.14}, {$string: "15"}],
		g: {v$string: "x"}
      }
