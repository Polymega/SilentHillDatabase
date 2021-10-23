# .mdl File Format
This is the 3D Model File Format used in SILENT HILL 2. It's mostly been reverse engineered by Mike M (the creator of the Silent Hill 2 Level Viewer). Most of the on disk structures have been identified and reverse engineered to the point where it is possible to draw a model in its entirety, as well as animate it, if it contains animations. Most of this work was done by Mike M (perdedork), the creator of the Silent Hill Level Viewer. Further reversing and confirmation was done by myself and [Kneesnap](). The file contains all mesh data required to draw the model correctly in-engine, including but no limited to: skeleton/bone data (including vertex weights), Blinn-Phong lighting data, index/face data, embedded textures etcetera. 

Note1: There is a _significant_ difference between the PS2 and PC versions of this file format. The PS2 version seems to be tightly coupled to the Playstation's Vector Units, with symbols dumped from the E3 Trial Version containing data members such as `vu0_parts_offset`, implying that the model format was designed to be DMA'd straight to the GS.

Note2: Some of the member names in this document are names from SH2 DWARF2 symbols, and hence may be slightly wonky or not quite make sense.

## Layout (WIP)
* `.mdl`
    * 1 `Model3_FileHeader`
    * 1 `Model3_ModelHeader`
    * 1 `Model3_VertexOffsetHeader`
    * 1-n `Initial Matrices`
    * 1-n `Skeletons`
    * 1-n `Primitive Descriptor Block`(???)
    * 1 `Index Data Block`
    * 1 `Vertex Data Block`
    * 0-n `Textures`

## Model3_FileHeader
Type | Name | Offset | Comment
-|-|-|-
u4 | unknown1 | 0x00 | Called "NoTextureID" on the PS2 Version.
u4 | character_id | 0x04 | Internal ID used by engine for this model
u4 | number_of_textures | 0x08 |
u4 | texture_header_offset | 0x0C | Offset to (first) texture header
u4 | model_header_offset | 0x10 | Offset to model data header
u4 | kg1_data_offset | 0x14 | Embedded `KG1` data. Doesn't look like this is used to often

## Model3_ModelHeader
NOTE: All offsets are relative to the beginning of _THIS_ header!
Type | Name | Offset | Comment
-|-|-|-
u4 | header_magic | 0x00 | Always `0xffff0003`
u4 | model_version | 0x04 | Model3 version number. 0x03 for PS2, 0x04 for PC
u4 | initial_matrices_offset | 0x08 | Initial pose matrices(???). See image below in the "Research" section of this file
u4 | number_of_skeletons | 0x0C | Number of skeletons in this model. Seems to line up with the the number of `initial_matrices`
u4 | skeleton_data_offset | 0x10 |
u4 | number_of_skeleton_pairs | 0x14 | ???
u4 | skeleton_pairs_offset | 0x18 |
u4 | default_pcms_offset | 0x1C | ???
u4 | number_of_primitive_headers | 0x20 | Seems to be the number of main vertex primitive headers
u4 | primitive_headers_offset | 0x24 |
u4 | number_of_primitive_headers_b | 0x28 | Mike has this marked as "alt_vert_sects". Not sure what that means yet
u4 | primitive_headers_offset_b | 0x2C |
u4 | number_of_texture_blocks | 0x30 |
u4 | texture_block_offset | 0x34 |
u4 | number_of_texture_ids | 0x38 |
u4 | texture_id_offset | 0x3C |
u4 | texture_id_param_offset | 0x40 | This seems to point to a copy of this header on the PC version
u4 | number_of_cluster_nodes | 0x44 | Name from PS2. Not sure what a "cluster node" is yet. Quaternion/Rotation data?
u4 | cluster_nodes_offset | 0x48 |
u4 | number_of_clusters | 0x4C | Same as above
u4 | clusters_offset | 0x50 |
u4 | unknown1 | 0x54 | Called `number_of_function_data` on PS2 version
u4 | unknown2 | 0x58 | Called `offset_to_function-data` on PS2 version
u4 | unknown3 | 0x5C | Called `hit_offset` on PS2 version
u4 | unknown4 | 0x60 | Called `box_offset` on PS2 version
u4 | unknown5 | 0x64 | Called `flag` on PS2 version
u4 | unknown6 | 0x68 | Called `relative_matrices_offset` on PS2 version
u4 | unknown6 | 0x6C | Called `relative_transpositions_offset` on PS2 version
u4 | unused4 | 0x70-0x80 |

## Model3_VertexOffsetHeader
Type | Name | Offset | Comment
-|-|-|-
u4 | vertex_count | 0x00 | Number of vertices
u4 | vertex_data_offset | 0x04 | Offset to vertex data relative to `Model3_ModelHeader`
u4 | unknown1 | 0x08 |
u4 | unknown2 | 0x0C |
u4 | index_data_offset | 0x10 | Offset to index data relative to `Model3_ModelHeader`
u4 | unknown3 | 0x14 |
u4 | unknown4 | 0x18 |
u4 | unknown5 | 0x1C |
u4 | unknown6 | 0x20 |

## Skeletons
No idea how this is laid out yet. Looks to be like some kind of node-tree or linked list. All values look 16-bit. First value always seems to be `0xFF`

## ModelSection Descriptor Blocks
This data type is varying in size, and describes the different "Vertex Parts" of the model. I'm not entirely sure of the exact purpose and layout of these, as Mike's code doesn't really describe too much. I've managed to reverse the structure somewhat, in terms of how to actually read it into memory.

## Model3_ModelSectionSizeHeader
Type | Name | Offset | Comment
-|-|-|-
u4 | header_size | 0x00 | Size of this header. Usually at least `0xB0` bytes long
u4 | pad1 | 0x04 |

## Model3_ModelSectionOffsetHeader
Type | Name | Offset | Comment
-|-|-|-
u4 | unknown_count_1 | 0x00 | Looks to be some kind of count for 16-bit data
u4 | offset_to_unknown_count_1 | 0x04 |
u4 | unknown_count_2 | 0x08 | Looks to be some kind of count for 16-bit data
u4 | offset_to_unknown_count_2 | 0x0C |
u4 | unknown_count_3 | 0x10 | Looks to be some kind of count for 16-bit data
u4 | offset_to_unknown_count_3 | 0x14 |
u4 | offset_to_footer_marker | 0x18 | Offset to the magic number `0x02020303` which signifies the end of this header

There is more data contained within this header, however it's currently unknown.
Offset 0x78 and 0x80 looks like start offset of indices and number of indices (all 16-bit) for the given section

## Texture
See `tex.md` (WIP)