# Simple Serialization Protocol - Properties

## Properties file representation

### Common

	[path][$TYPE[$[attributes]]]=TYPE_DATA
	
path is:

	[prefix][path-elements]
	
path-element for object entry is:

	.name
	
path-element for array element is:

	%index

### Type tag

	TYPE_STRING     string
	TYPE_DECIMAL    decimal
	TYPE_INT*       int
	TYPE_FLOAT*     float
	TYPE_BYTE_ARRAY bytearray

### Integer types

	[path]$int[$[attributes]]=TYPE_DATA
	
### Float-point types

	[path]$float[$[attributes]]=TYPE_DATA

### String

	[path]$string[$[attributes]]=TYPE_DATA

### Big decimal
  
	[path]$decimal[$[attributes]]=TYPE_DATA

where TYPE_DATA is string representation of big decimal

### Byte array
  
	[path]$bytearray[$[attributes]]=TYPE_DATA

where TYPE_DATA is base64 representation of byte array
if attribute "hex" specified - hex representation of byte array
Multiline supported:

	[path]$bytearray[$attributes]$line%index=TYPE_DATA

### Object

Optional object header (required only for empty objects):

	[path].=
	
Object entry path:

	[path].name
	
When path is empty, leading "." can be skipped for not-empty objects.
    
### Array

Optional array header (required only for empty arrays):

	[path]%=
	
Array element path:

	[path]%index

## Example

	a$int=-1
	b$float=0.3
	c$string=string
	d$bytearray$hex$line%1=1234
	d$bytearray$hex$line%2=5678
	e$decimal=1234567
	f%1$int=3
	f%2$float=0.14
	f%3$string=15
	g.v$string=x
