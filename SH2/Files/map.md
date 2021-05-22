# .map File Format
The reverse engineering status of this file format is very close to 100%: I can generate new maps from scratch and load all the existing ones without problems. There are still a few minor details I'm not completely sure about, but as a whole you can use this to fully extract SH2's maps.
This format is only used for SH2, SH3's .map is different.

When the name is `unused` it is generally because it's padding or always 0. `unknown` has a value but doesn't seem to be used or change much if at all.

Note that all the names are my own, I don't know how the devs actually called everything. If you have better suggestions feel free to talk about it.

# .map File
A `.map` file has a header and contains two different subfiles: A `TextureGroup` subfile and a `GeometryGroup` subfile. There can be multiple of either, or none.

## Layout
* `.map`
    * 1 `Header`
    * 0-n `TextureGroup` SubFiles (Each preceded by a `Sub File Header`)
    * 0-n `GeometryGroup` SubFiles (Each preceded by a `Sub File Header`)

## Header
Type | Name | Offset | Comment
-|-|-|-
u4 | magicByte | 0x0 | 0x20010510
s4 | fileLength | 0x4 | 
s4 | subFileCount | 0x8 | 
s4 | unused | 0xC | 

## Sub File Header
Type | Name | Offset | Comment
-|-|-|-
s4 | subFileType | 0x0 | 1 = `Geometry` SubFile, 2 = `Texture Group` SubFile
s4 | subFileLength | 0x4 | 
s4 | unused | 0x8 | 
s4 | unused | 0xC | 

# Texture Group SubFile
The `Texture Group` SubFile is a collection of textures in the [BC1](https://www.khronos.org/registry/DataFormat/specs/1.1/dataformat.1.1.html#_bc1_with_no_alpha) and [BC2](https://www.khronos.org/registry/DataFormat/specs/1.2/dataformat.1.2.html#_bc2) formats.

There's no `BC Texture` count, read until the first int of the line is 0, and then skip that line.

## Layout
* `TextureGroup` SubFile
    * 1 `Header`
    * n BC Textures

## Header
Type | Name | Offset | Comment
-|-|-|-
u4 | magicByte | 0x0 | 0x19990901
s4 | unused | 0x4 | 
s4 | unused | 0x8 | 
s4 | unknown | 0xC | Always 1

## BC Texture
It's important to understand that this is a texture compressed in BC format, _not_ a DXT texture. The actual pixel data and compression information is contained within each sprites.
### Layout
* `BC Texture`
    * 1 `Header`
    * n `Sprite`

### Header
Type | Name | Offset | Comment
-|-|-|-
s4 | textureId | 0x0 | Max 0xFFFF
s2 | width | 0x4 | 
s2 | height | 0x6 | 
s2 | width2 | 0x8 | Always the same as width
s2 | height2 | 0xA | Always the same as height
s4 | spriteCount | 0xC |
s2 | unknown | 0x10 | Saw it as 1 2 3 4 5 6 7 8 9 A B C D E F 10 28
s2 | textureId2 | 0x12 | Always textureId
s4 | unused | 0x14 |
s4 | unused | 0x18 | 
s4 | unused | 0x1C | 

## Sprite
Sprites are weird. They seem to be the leftover from a cut feature. The way it is now, the last sprite is _always_ the actual texture, and the other sprites are leftover that does nothing and has no pixels, but are still read, so they still need to be valid. They can be deleted however, since the game itself never looks for them.

The compression of the pixel data is BC1 if the format is 100 or 103, BC2 if 102 or 104.  

### Layout
* `Sprite`
    * 1 `Header`
    * 1 `Pixel Header`
    * n Pixels

### Header
Type | Name | Offset | Comment
-|-|-|-
s4 | spriteId | 0x0 | Max 0xFFFF
s2 | x | 0x4 | X position of the sprite in the texture
s2 | y | 0x6 | Y position of the sprite in the texture
s2 | width | 0x8 | Width of the sprite
s2 | height | 0xA | Height of the sprite
u4 | format | 0xC | 100 102 103 104

### Pixel Header
Type | Name | Offset | Comment
-|-|-|-
s4 | pixelDataLength | 0x0 | Size of all the pixels
s4 | pixelHeaderAndDataLength | 0x4 | Size of this header plus all the pixels
s4 | unused | 0x8 | 
u4 | unknown | 0xC | Always 0x99000000

# GeometryGroup SubFile
The `GeometryGroup` subfile is where the actual 3D geometry of the map is located, along with some other information such as `Material`s.

## Layout
* `GeometryGroup` SubFile
    * 1 `Header`
    * n `Geometry`
    * n `Material`

## Header
Type | Name | Offset | Comment
-|-|-|-
u4 | magicBytes | 0x0 | 0x20010730
s4 | geometryCount | 0x4 | 
s4 | geometrySize | 0x8 | 
s4 | materialCount | 0xC | 

## Geometry
Each `Geometry` generally represent a logical object in the map. The main map geometry is one, as well as some object like maps on the wall or object removed in cutscenes like the double door of the closet of the Pyramid Head cutscene in the apartment. Most other items like ammo, guns or health are not part of the map itself.

### Layout
* `Geometry`
    * 1 `Header`
    * 0-1 `MeshGroup` Opaque Group
    * 0-1 `MeshGroup` Transparent Group
    * 0-1 `MapDecalGroup`

### Header
Offsets are calculated from the start of the header.
Type | Name | Offset | Comment
-|-|-|-
s4 | geometryId | 0x0 | Used by the game code to identify a part of the map.
s4 | meshGroupSize | 0x4 | 
s4 | offsetToOpaqueGroup | 0x8 | No opaque mesh group if offset is 0
s4 | offsetToTransparentGroup | 0xC | No transparent mesh group if offset is 0
s4 | offsetToDecals | 0x10 | No decals if offset is 0

## MeshGroup
A `MeshGroup` is a small structure that only contains a bunch of `MapMesh`. Each `MapMesh` is aligned to line.

### Layout
* `MeshGroup`
    * 1 `Header`
    * n `MapMesh`

### Header
Type | Name | Offset | Comment
-|-|-|-
s4 | mapMeshCount | 0x0 | 
s4[] | mapMeshOffsets | 0x4 | The length of this array is determined by mapMeshCount. Each offset is an s4.

## MapMesh
This is where the main vertex data is located. It doesn't have the information that actually puts them into shapes though, that's in `MeshPart`, but it does contains all the vertices and indices needed.

### Layout
* `MapMesh`
    * 1 `Header`
    * n `MeshPartGroup`
    * 1 `VertexSectionsHeader`
    * n `VertexSectionHeader`
    * n groups of n vertices bytes
    * n indices

### Header
Type | Name | Offset | Comment
-|-|-|-
v4 | boundingBoxA | 0x0 | One corner of a bounding box
v4 | boundingBoxB | 0x10 | The other corner of the bounding box
s4 | offsetToVertexSectionHeader | 0x20 | 
s4 | offsetToIndices | 0x24 | 
s4 | indicesLength | 0x28 | 
s4 | unknown | 0x2C | Looks like a length or offset, setting to garbage does nothing
s4 | meshPartGroupCount | 0x30 | 

## MeshPartGroup

### Layout
* `MeshPartGroup`
    * 1 `Header`
    * n `MeshPart`

### Header
Type | Name | Offset | Comment
-|-|-|-
s4 | materialIndex | 0x0 | The material to use from the `GeometryGroup` materials
s4 | sectionId | 0x4 | Vertex section to use
s4 | meshPartCount | 0x8 | 

### MeshPart
Type | Name | Offset | Comment
-|-|-|-
u2 | stripLength | 0x0 | 
u1 | invertReading | 0x2 |
u1 | stripCount | 0x3 | 
u2 | firstVertex | 0x4 |
u2 | lastVertex | 0x6 | 

## MapDecalGroup

### Layout
* `MapDecalGroup`
    * 1 `Header`
    * n `Decal`

### Header
Type | Name | Offset | Comment
-|-|-|-
s4 | decalCount | 0x0 | 
s4[] | decalOffsets | 0x4 | 

## Decal
Decals are similar to `MapMesh`, but they are simpler, and generally used for simple transparent things like fake shadows.

### Layout
* `Decal`
    * 1 `Header`
    * n `SubDecal`
    * 1 `VertexSectionsHeader`
    * n `VertexSectionHeader`
    * n groups of n vertices bytes
    * n indices

### Header
Type | Name | Offset | Comment
-|-|-|-
v4 | boundingBoxA | 0x0 | One corner of a bounding box
v4 | boundingBoxB | 0x10 | The other corner of the bounding box
s4 | unused | 0x20 | 
s4 | offsetToIndices | 0x24 | 
s4 | indicesLength | 0x28 | 
s4 | decalCount | 0x2C | 

### SubDecal
Type | Name | Offset | Comment
-|-|-|-
s4 | materialIndex | 0x0 | 
s4 | sectionId | 0x4 | 
s4 | stripLength | 0x8 | 
s4 | stripCount | 0xC | 

### VertexSectionsHeader
Type | Name | Offset | Comment
-|-|-|-
s4 | verticesLength | 0x0 | 
s4 | vertexSectionCount | 0x4 | 

### VertexSectionHeader
Type | Name | Offset | Comment
-|-|-|-
s4 | sectionStarts | 0x0 | 
s4 | vertexSize | 0x4 | 14, 18, 20, 24, see below
s4 | sectionLength | 0x8 | 

The following structures are based on their size from the `vertexSize` of `VertexSectionHeader`.

### Vertex14
Type | Name | Offset | Comment
-|-|-|-
v3 | position | 0x0 | 
v2 | uv | 0xC | 

### Vertex18
Type | Name | Offset | Comment
-|-|-|-
v3 | position | 0x0 | 
rgba8888 | color | 0xC | 
v2 | uv | 0x10 | 

### Vertex20
Type | Name | Offset | Comment
-|-|-|-
v3 | position | 0x0 | 
v3 | normal | 0xC | 
v2 | uv | 0x18 | 

### Vertex20
Type | Name | Offset | Comment
-|-|-|-
v3 | position | 0x0 | 
v3 | normal | 0xC | 
rgba8888 | color | 0x18 | 
v2 | uv | 0x1C | 

## Material

Type | Name | Offset | Comment
-|-|-|-
s2 | mode | 0x0 | 0 = emissive, 1 = cutout, 2 = specular, 4 = diffuse (use opaque or transparent varieties depending on mesh group
s2 | textureID | 0x2 | maps to the id of the BC Texture header
bgra8888 | materialColor | 0x4 | not sure ocmpletely how it's used, it changes per mode
bgra8888 | overlayColor | 0x8 | same things, changes per mode
f4 | specularity | 0xC | 0.0f to 100.0f I think
