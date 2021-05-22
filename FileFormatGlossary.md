# File Format Glossary

Offsets are always relative to the structure unless specified otherwise.

### Line 
A "line" is 16 bytes long. You can read the line of an offset from how many 0x10 it has. 0x00 is line 0, 0x1C is line 1, 0x200 is line 32. To "skip a line" means to go to the next one, so if you're on 0x14 skipping the line brings you to 0x20.

### Magic Byte
"Magic bytes" is a general term for a specific value in a file used to mark it as a file of "that type". It's not necessarily a byte, it's often an int (4 bytes). Usually a file reader will make sure the magic byte is present and won't read the file if it isn't. Silent Hill games tend to use a date in hex as magic bytes.

## Types
Shorthand | Full name | C# | C
-|-|-|-
s1 | 1 byte signed | sbyte | char
s2 | 2 bytes signed | short | short
s4 | 4 bytes signed | int | int
s8 | 8 bytes signed | long | long long
u1 | 1 byte unsigned | byte | unsigned char
u2 | 2 bytes unsigned | ushort | unsigned short
u4 | 4 bytes unsigned | uint | unsigned int
u8 | 8 bytes unsigned | ulong | unsigned long long
f4 | 4 bytes float | float | float
f8 | 8 bytes float | double | double
v2 | 8 bytes vector of 2 | float[2] | float[2]
v3 | 12 bytes vector of 3 | float[3] | float[3]
v4 | 16 bytes vector of 4 | float[4] | float[4]
str | string of chars alone | byte[n] | byte[n]
0str | null-terminated string | byte[n+'/0'] | char[n+'/0']
nstr | length prefixed string | s4 + byte[s4] | int + char[s4]

## Colors
Color shorthands are composed in pair of a color and how many bits this color has. for example, a red color of 8 bits will be r8, 32 bits rgba color will be rgba8888 and so on.

Shorthand | Full name 
-|-
r | red
g | green
b | blue
a | alpha
