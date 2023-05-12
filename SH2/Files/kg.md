# .kg1/.kg2 File Format

`.kg1` and `.kg2` are file formats used to store 3D models for casting shadows (kg = "kage" (影) or "shadow"). These files are paired with other 3D model files as follows:

* `.kg1`: Shadow model for characters, enemies, items and weapons (`.mdl`)
* `.kg2`: Shadow model for maps (`.map`)

The data layout is the same for both file extensions. However, there are minor differences in certain header field names and how object transforms are handled.

Both formats are the same between PS2 and PC. Some names below are borrowed from DWARF debug symbols from the [Silent Hill 2 (July 13, 2001) PS2 prototype](https://hiddenpalace.org/Silent_Hill_2_(Jul_13,_2001_prototype)).

## Layout

* `.kg1`/`.kg2`
    * 1 `Header`
    * n `ShadowObject`

## Header (`.kg1`)

Named `SHADOW_CHAR_HEAD` on PS2.

Type | Name | Offset | Comment
-|-|-|-
s2 | charId | 0x0 | Typically 0.
u2 | kind | 0x2 | Typically 0.
s2 | objectCount | 0x4
s2[5] | reserved | 0x6

## Header (`.kg2`)

Named `SHADOW_OUTDOOR_HEAD` (`.kg2`) on PS2.

Type | Name | Offset | Comment
-|-|-|-
u2 | kind | 0x0 | Typically 0.
s2 | mapId | 0x2 | Typically 0.
s2 | objectCount | 0x4
s2[5] | reserved | 0x6

## ShadowObject

### Layout

* `ShadowObject`
    * 1 `Header`
    * n `ShadowGeometry`

### Header

Named `SHADOW_CHAR_OBJ_HEAD` (`.kg1`) or `SHADOW_OUTDOOR_OBJ_HEAD` (`.kg2`) on PS2.

Type | Name | Offset | Comment
-|-|-|-
u4 | id | 0x0 | Named `charId` (`.kg1`) or `mapId` (`.kg2`) on PS2. Typically 0.
s2 | objectId | 0x4 | Bone index for `.kg1`.
s2 | geometryCount | 0x6 |
s2[4] | reserved | 0x8 |
s2[4] | unused | 0x10 |
s2 | boundaryX | 0x18 | X coordinate of bounding sphere.
s2 | boundaryY | 0x1A | Y coordinate of bounding sphere.
s2 | boundaryZ | 0x1C | Z coordinate of bounding sphere.
s2 | boundaryR | 0x1E | Radius of bounding sphere.
v4[4] | matrix | 0x20 | Only applicable to `.kg2`. For `.kg1`, this matrix will be all 0's. Instead, the object inherits the transform of the bone index given by `objectId`.

## ShadowGeometry

### Layout

* `ShadowGeometry`
    * 1 `Header`
    * n Vertex data (determined by `prim` in `Header`)

### Header

Named `SHADOW_GEOM_HEAD` on PS2.

Type | Name | Offset | Comment
-|-|-|-
s2 | vertexCount | 0x0 |
s2 | prim | 0x2 | See "Primitive types" for details.
s2 | sendDataNum | 0x4 |
s2 | eeMemorySize | 0x6 | Total size in quadwords including this header.
s2 | boundaryX | 0x8 | X coordinate of bounding sphere.
s2 | boundaryY | 0xA | Y coordinate of bounding sphere.
s2 | boundaryZ | 0xC | Z coordinate of bounding sphere.
s2 | boundaryR | 0xE | Radius of bounding sphere.

## Primitive types

Primitive Type | Name | Struct | Notes
-|-|-|-
0x1 | PLANE | `PlaneBillboardVertexData` | Used for walls.
0x2 | PLANE | `PlaneBillboardVertexData` | Used for walls.
0x3 | BILLBOARD | `PlaneBillboardVertexData` |
0x4 | BILLBOARD | `PlaneBillboardVertexData` | Reversed winding order.
0x5 | TRIANGLE_STRIP_ADV | `TriangleStripAdvVertexData` |
0x6 | TRIANGLE_STRIP_ADV | `TriangleStripAdvVertexData` | Reversed winding order.
0x7 | PLANE | `PlaneBillboardVertexData` | Only present in `bg/ps/ps105.kg2`.
0x8 | TRIANGLE_STRIP_ADV | `TriangleStripAdvVertexData` |
0x9 | TRIANGLE_STRIP_ADV | `TriangleStripAdvVertexData` | Reversed winding order.
0xA | BILLBOARD | `PlaneBillboardVertexData` |

Names are borrowed from jump labels in PS2 VU microcode for rendering shadows. Note that some names are duplicated since certain types share the same micro-subroutines. The exact behavior of each type is currently unknown.

Typically, `.kg1` will contain only primitive types 5 and 6 (triangle strips). `.kg2` can contain all types.

## PlaneBillboardVertexData

An n-gon with a single face.

### Layout

* PlaneBillboardVertexData
    * 1 `FaceNormal`
    * n `VertexPos`

## TriangleStripAdvVertexData

A triangle strip with `n` vertices and `m = n - 2` faces. Does not contain any primitive restarts.

### Layout

* PlaneBillboardVertexData
    * 2 `VertexPos`
    * Repeat `m` times:
      * 1 `FaceNormal`
      * 1 `VertexPos`

## FaceNormal

Type | Name | Offset | Comment
-|-|-|-
s2 | x | 0x0 |
s2 | y | 0x2 |
s2 | z | 0x4 |
s2 | w | 0x6 | Always 0x0.

## VertexPos

Type | Name | Offset | Comment
-|-|-|-
s2 | x | 0x0 |
s2 | y | 0x2 |
s2 | z | 0x4 |
s2 | w | 0x6 | Always 0x1.
