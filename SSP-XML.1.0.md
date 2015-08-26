# Simple Serialization Protocol - XML

## XML representation

### Common

	<TYPE [attributes]>TYPE_DATA</TYPE>
	
### Type tag

	TYPE_OBJECT     object
	TYPE_ARRAY      array
	TYPE_STRING     string
	TYPE_DECIMAL    decimal
	TYPE_INT*       int
	TYPE_FLOAT*     float
	TYPE_BYTE_ARRAY bytearray

### Integer types

	<int [attributes]>TYPE_DATA</int>
	
where TYPE_DATA is string representation of integer

### Float-point types

	<float [attributes]>TYPE_DATA</float>

where TYPE_DATA is string representation of float

### String

	<string [attributes]>TYPE_DATA</string>

where TYPE_DATA is string

### Big decimal
  
	<decimal [attributes]>TYPE_DATA</decimal>

where TYPE_DATA is string representation of big decimal

### Byte array
  
	<bytearray [attributes]>TYPE_DATA</bytearray>

where TYPE_DATA is base64 representation of byte array

### Object

	<object [attributes]>
	[object entries]
	</object>

Each object entry contains any type with specified name attribute

	<TYPE name="any-field-name">TYPE_DATA</TYPE>
    
### Array

	<array [attributes]>
	[array elements]
	</array>

Each element is value of any type without name attribute
  
	<TYPE>TYPE_DATA</TYPE>

## Example

	<object>
		<int     name="a">-1</int>
		<float   name="b">0.3</float>
		<string  name="c">string</string>
		<decimal name="d">1234567</decimal>
		<array   name="e"><int>3</int><float>0.14</float><string>15</string></array>
		<object  name="f"><string name="v">x</string></object>
	</object>
