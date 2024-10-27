# .mdl File Format
This is the 3D Model File Format used in SILENT HILL 2.

Note1: There is a _significant_ difference between the PS2 and PC versions of this file format. The PS2 version seems to
be tightly coupled to the Playstation's Vector Units, with symbols dumped from the E3 Trial Version containing data
members such as `vu0_parts_offset`, implying that the model format was designed to be DMA'd straight to the GS.

Note2: Some of the member names in this document are names from SH2 DWARF2 symbols, and hence may be slightly wonky or
not quite make sense.

<h2>Type: Mdl</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>header</td>
            <td>FileHeader</td>
        </tr>
        <tr>
            <td>model_data</td>
            <td>Model</td>
        </tr>
        <tr>
            <td>texture_data</td>
            <td>TextureData</td>
        </tr>
    </tbody>
</table>

<h2>Type: FileHeader</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>no_texture_id</td>
            <td>u1</td>
            <td>Indicates whether this model reuses a texture from another model (1),
                or if it has its own texture (0). Equal to 1 for models with "notex"
                in the filename, e.g. "hll_jms_notex.mdl".
            </td>
        </tr>
        <tr>
            <td>padding</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>character_id</td>
            <td>u4le</td>
            <td>Internal ID used by the game engine for this model.</td>
        </tr>
        <tr>
            <td>texture_count</td>
            <td>u4le</td>
            <td>Number of textures in this model.</td>
        </tr>
        <tr>
            <td>texture_header_offset</td>
            <td>u4le</td>
            <td>Absolute byte offset to the start of texture data.</td>
        </tr>
        <tr>
            <td>model_header_offset</td>
            <td>u4le</td>
            <td>Absolute byte offset to the start of general model data.</td>
        </tr>
        <tr>
            <td>kg1_header_offset</td>
            <td>u4le</td>
            <td>Absolute byte offset to the start of embedded shadow data.</td>
        </tr>
    </tbody>
</table>

<h2>Type: Model</h2>

<p>Model container. All offsets are relative to the start of this header.</p>
<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>magic</td>
            <td></td>
            <td>0x03 0x00 0xff 0xff is the start of the model header.</td>
        </tr>
        <tr>
            <td>model_version</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>initial_matrices_offset</td>
            <td>u4le</td>
            <td>Offset to initial bone matrices in model space.</td>
        </tr>
        <tr>
            <td>bone_count</td>
            <td>u4le</td>
            <td>Number of bones.</td>
        </tr>
        <tr>
            <td>skeleton_data_offset</td>
            <td>u4le</td>
            <td>Offset to skeleton data.</td>
        </tr>
        <tr>
            <td>bone_pairs_count</td>
            <td>u4le</td>
            <td>See for more info.</td>
        </tr>
        <tr>
            <td>bone_pairs_offset</td>
            <td>u4le</td>
            <td>Offset to bone pairs, specified in pairs of bytes.</td>
        </tr>
        <tr>
            <td>default_pcms_offset</td>
            <td>u4le</td>
            <td>Offset to default parent-child matrices for bone pairs.</td>
        </tr>
        <tr>
            <td>opaque_primitive_headers_count</td>
            <td>u4le</td>
            <td>Number of opaque model parts.</td>
        </tr>
        <tr>
            <td>opaque_primitive_headers_offset</td>
            <td>u4le</td>
            <td>Offset to headers describing each primitive.</td>
        </tr>
        <tr>
            <td>transparent_primitive_headers_count</td>
            <td>u4le</td>
            <td>See for more info.</td>
        </tr>
        <tr>
            <td>transparent_primitive_headers_offset</td>
            <td>u4le</td>
            <td>See for more info.</td>
        </tr>
        <tr>
            <td>texture_blocks_count</td>
            <td>u4le</td>
            <td>Number of texture blocks.</td>
        </tr>
        <tr>
            <td>texture_blocks_offset</td>
            <td>u4le</td>
            <td>Offset to the texture blocks.</td>
        </tr>
        <tr>
            <td>texture_id_count</td>
            <td>u4le</td>
            <td>Number of unique texture IDs used by the game engine.</td>
        </tr>
        <tr>
            <td>texture_id_offset</td>
            <td>u4le</td>
            <td>Offset to texture ID data.</td>
        </tr>
        <tr>
            <td>junk_padding_offset</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>cluster_node_count</td>
            <td>u4le</td>
            <td>Number of morph vertices for this object.</td>
        </tr>
        <tr>
            <td>cluster_node_offset</td>
            <td>u4le</td>
            <td>Offset to morph vertex data for this object.</td>
        </tr>
        <tr>
            <td>cluster_count</td>
            <td>u4le</td>
            <td>Number of morph targets for this object.</td>
        </tr>
        <tr>
            <td>cluster_offset</td>
            <td>u4le</td>
            <td>Offset to morph targets for this object.</td>
        </tr>
        <tr>
            <td>func_data_count</td>
            <td>u4le</td>
            <td>Unknown.</td>
        </tr>
        <tr>
            <td>func_data_offset</td>
            <td>u4le</td>
            <td>Unknown.</td>
        </tr>
        <tr>
            <td>hit_offset</td>
            <td>u4le</td>
            <td>Unknown.</td>
        </tr>
        <tr>
            <td>box_offset</td>
            <td>u4le</td>
            <td>Unknown.</td>
        </tr>
        <tr>
            <td>flag</td>
            <td>u4le</td>
            <td>Unknown flag.</td>
        </tr>
        <tr>
            <td>relative_matrices_offset</td>
            <td>u4le</td>
            <td>Unknown.</td>
        </tr>
        <tr>
            <td>relative_trans_offset</td>
            <td>u4le</td>
            <td>Unknown.</td>
        </tr>
        <tr>
            <td>reserved</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>opaque_vertex_count</td>
            <td>u4le</td>
            <td>Number of opaque vertices.</td>
        </tr>
        <tr>
            <td>opaque_vertex_data_offset</td>
            <td>u4le</td>
            <td>Offset to opaque vertex data.</td>
        </tr>
        <tr>
            <td>transparent_vertex_count</td>
            <td>u4le</td>
            <td>Number of transparent vertices.</td>
        </tr>
        <tr>
            <td>transparent_vertex_data_offset</td>
            <td>u4le</td>
            <td>Offset to transparent vertex data.</td>
        </tr>
        <tr>
            <td>opaque_triangle_index_offset</td>
            <td>u4le</td>
            <td>Offset to opaque triangle index data.</td>
        </tr>
        <tr>
            <td>transparent_triangle_index_offset</td>
            <td>u4le</td>
            <td>Offset to transparent triangle index data.</td>
        </tr>
        <tr>
            <td>opaque_cluster_map_count</td>
            <td>u4le</td>
            <td>Unknown original name.</td>
        </tr>
        <tr>
            <td>transparent_cluster_map_count</td>
            <td>u4le</td>
            <td>Unknown original name.</td>
        </tr>
        <tr>
            <td>cluster_map_offset</td>
            <td>u4le</td>
            <td>Unknown original name.</td>
        </tr>
        <tr>
            <td>pad0</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>initial_matrices</td>
            <td>TransformationMatrix</td>
            <td>Matrices that represent the pose of each bone in model space. This is
                an array of matrices where .
            </td>
        </tr>
        <tr>
            <td>skeleton_tree</td>
            <td>u1</td>
            <td>A graph having a tree structure that represents the skeleton. This is
                an array of indices where bone is the parent of
                <code>skeleton_tree[i]`. If `skeleton_tree[i]` is 255, then the bone `i</code>
                is parented to a hardcoded root node.
            </td>
        </tr>
        <tr>
            <td>pad1</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>bone_pairs</td>
            <td>BonePair</td>
            <td></td>
        </tr>
        <tr>
            <td>pad2</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>default_pcms_matrices</td>
            <td>TransformationMatrix</td>
            <td>Matrices that represent relative transformations between bones. For
                index
                equal <code>bone_pairs[i].child`. Then `default_pcms_matrices[i]</code> is equal
                to <code>inverse(initial_matrices[child]) * initial_matrices[parent]</code>.
            </td>
        </tr>
        <tr>
            <td>texture_metadata</td>
            <td>TextureMetadata</td>
            <td></td>
        </tr>
        <tr>
            <td>junk_padding</td>
            <td></td>
            <td>A sequence of copied bytes used to pad out the file in the
                PC version. It starts copying from the start of this header until it
                reaches the cluster node data. On the PS2 version, this space is used
                for a block called "texture_id_params".
            </td>
        </tr>
        <tr>
            <td>cluster_node_normals</td>
            <td>S2Vector</td>
            <td></td>
        </tr>
        <tr>
            <td>geometry</td>
            <td>Geometry</td>
            <td>Container for all model parts.</td>
        </tr>
        <tr>
            <td>cluster_nodes</td>
            <td>S2Vector</td>
            <td>Morph vertices and normals for facial animation.</td>
        </tr>
        <tr>
            <td>cluster_maps</td>
            <td>ClusterMaps</td>
            <td>Unknown original name. Describes how morphs influence geometry.</td>
        </tr>
        <tr>
            <td>clusters</td>
            <td>Cluster</td>
            <td>Morph targets for facial animation.</td>
        </tr>
    </tbody>
</table>

<h2>Type: BonePair</h2>

<p>Each vertex specifies up to three indices of bone pairs. For each pair,
    the <code>child</code> bone can have a weighted influence on the bone, while the
    <code>parent</code> should be the vertex's parent bone.
</p>
<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>parent</td>
            <td>u1</td>
        </tr>
        <tr>
            <td>child</td>
            <td>u1</td>
        </tr>
    </tbody>
</table>

<h2>Type: SpriteHeader</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>sprite_id</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>x</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>y</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>width</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>height</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>format</td>
            <td>u1→TextureFormat</td>
        </tr>
        <tr>
            <td>unknown0</td>
            <td>u1</td>
        </tr>
        <tr>
            <td>importance</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>data_size</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>all_size</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>pad</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown1</td>
            <td>u1</td>
        </tr>
        <tr>
            <td>unknown2</td>
            <td>u1</td>
        </tr>
        <tr>
            <td>end_magic</td>
            <td>u2le</td>
        </tr>
    </tbody>
</table>

<h3>Enum: texture_format</h3>

<table>
    <tbody>
        <tr>
            <th>Name</th>
        </tr>
        <tr>
            <td>dxt1</td>
        </tr>
        <tr>
            <td>dxt2</td>
        </tr>
        <tr>
            <td>dxt3</td>
        </tr>
        <tr>
            <td>dxt4</td>
        </tr>
        <tr>
            <td>dxt5</td>
        </tr>
        <tr>
            <td>paletted</td>
        </tr>
        <tr>
            <td>rgbx8</td>
        </tr>
        <tr>
            <td>rgba8</td>
        </tr>
    </tbody>
</table>

<h2>Type: ClusterMapping</h2>

<p>Unknown original IDs for all properties.</p>
<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>source_start_index</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>target_start_index</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>count</td>
            <td>u2le</td>
        </tr>
    </tbody>
</table>

<h2>Type: OpaqueVertexData</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>x</td>
            <td>f4le</td>
            <td>The x-coordinate of the vertex.</td>
        </tr>
        <tr>
            <td>y</td>
            <td>f4le</td>
            <td>The y-coordinate of the vertex.</td>
        </tr>
        <tr>
            <td>z</td>
            <td>f4le</td>
            <td>The z-coordinate of the vertex.</td>
        </tr>
        <tr>
            <td>bone_weight_0</td>
            <td>f4le</td>
            <td>The first bone weight of the vertex.</td>
        </tr>
        <tr>
            <td>bone_weight_1</td>
            <td>f4le</td>
            <td>The second bone weight of the vertex.</td>
        </tr>
        <tr>
            <td>bone_weight_2</td>
            <td>f4le</td>
            <td>The third bone weight of the vertex.</td>
        </tr>
        <tr>
            <td>bone_weight_3</td>
            <td>f4le</td>
            <td>The fourth bone weight of the vertex.</td>
        </tr>
        <tr>
            <td>normals</td>
            <td>s2le</td>
            <td></td>
        </tr>
        <tr>
            <td>alignment</td>
            <td>u2le</td>
            <td></td>
        </tr>
        <tr>
            <td>u</td>
            <td>f4le</td>
            <td>The texture coordinate along the horizontal axis (x), from 0 to 1.</td>
        </tr>
        <tr>
            <td>v</td>
            <td>f4le</td>
            <td>The texture coordinate along the vertical axis (y), from 0 to 1.</td>
        </tr>
        <tr>
            <td>bone_index_0</td>
            <td>u1</td>
            <td>The first bone index. This indexes into the primitive bone array, not
                the overall skeleton array!
            </td>
        </tr>
        <tr>
            <td>bone_index_1</td>
            <td>u1</td>
            <td>The first bone pair index.
            </td>
        </tr>
        <tr>
            <td>bone_index_2</td>
            <td>u1</td>
            <td>The second bone pair index.
            </td>
        </tr>
        <tr>
            <td>bone_index_3</td>
            <td>u1</td>
            <td>The third bone pair index.
            </td>
        </tr>
    </tbody>
</table>

<h2>Type: ClusterData</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>vector</td>
            <td>S2Vector</td>
        </tr>
        <tr>
            <td>vertex_index</td>
            <td>s2le</td>
        </tr>
    </tbody>
</table>

<h2>Type: TransparentPrimitiveHeader</h2>

<p>On the PS2 version, this field is called "n_vu0_parts",
    suggesting that these were handled by the VU0 coprocessor, while
    the opaque primitive headers were handled by the VU1 coprocessor.
</p>
<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>pad0</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>texture_index_count</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>texture_index_offset</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>marker_offset</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown_count</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown_section0</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>pad1</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown_floats0</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>pad2</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown_floats1</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown_section1</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>primitive_start_index</td>
            <td>u4le</td>
            <td>Offset into the triangle index array where the primitive begins.</td>
        </tr>
        <tr>
            <td>primitive_length</td>
            <td>u4le</td>
            <td>The length of the primitive in the triangle index array.</td>
        </tr>
        <tr>
            <td>primitive_index</td>
            <td>u4le</td>
            <td>Appears to be an array index for this primitive header.</td>
        </tr>
        <tr>
            <td>texture_index</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>pad3</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>sampler_states</td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

<h2>Type: OpaquePrimitiveHeaderWrapper</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>primitive_header_size</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>body</td>
            <td>OpaquePrimitiveHeader</td>
        </tr>
    </tbody>
</table>

<h2>Type: Cluster</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>node_count</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>offset</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>data</td>
            <td>ClusterDataList</td>
        </tr>
    </tbody>
</table>

<h2>Type: TextureMetadata</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>main_texture_ids</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>pad</td>
            <td></td>
        </tr>
        <tr>
            <td>texture_pairs</td>
            <td>TexturePair</td>
        </tr>
    </tbody>
</table>

<h2>Type: S2Vector</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>x</td>
            <td>s2le</td>
        </tr>
        <tr>
            <td>y</td>
            <td>s2le</td>
        </tr>
        <tr>
            <td>z</td>
            <td>s2le</td>
        </tr>
    </tbody>
</table>

<h2>Type: TexturePair</h2>


<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>texture_index</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>sprite_id</td>
            <td>u4le</td>
        </tr>
    </tbody>
</table>

<h2>Type: ClusterDataList</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>vertices</td>
            <td>ClusterData</td>
        </tr>
        <tr>
            <td>alignment</td>
            <td></td>
        </tr>
        <tr>
            <td>normals</td>
            <td>ClusterData</td>
        </tr>
    </tbody>
</table>

<h2>Type: ClusterMaps</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>opaque</td>
            <td>ClusterMapping</td>
        </tr>
        <tr>
            <td>transparent</td>
            <td>ClusterMapping</td>
        </tr>
    </tbody>
</table>

<h2>Type: TransparentPrimitiveHeaderWrapper</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>transparent_primitive_header_size</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>body</td>
            <td>TransparentPrimitiveHeader</td>
        </tr>
    </tbody>
</table>

<h2>Type: TextureContainer</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>texture_id</td>
            <td>u4le</td>
        </tr>
        <tr>
            <td>width</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>height</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>width2</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>height2</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>sprite_count</td>
            <td>u2le</td>
        </tr>
        <tr>
            <td>unknown_section</td>
            <td></td>
        </tr>
        <tr>
            <td>sprite_headers</td>
            <td>SpriteHeader</td>
        </tr>
        <tr>
            <td>data</td>
            <td></td>
        </tr>
    </tbody>
</table>

<h2>Type: IndexList</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>array</td>
            <td>u2le</td>
        </tr>
    </tbody>
</table>

<h2>Type: TransparentVertexData</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>x</td>
            <td>f4le</td>
            <td>The x-coordinate of the vertex.</td>
        </tr>
        <tr>
            <td>y</td>
            <td>f4le</td>
            <td>The y-coordinate of the vertex.</td>
        </tr>
        <tr>
            <td>z</td>
            <td>f4le</td>
            <td>The z-coordinate of the vertex.</td>
        </tr>
        <tr>
            <td>w</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>bone_weights</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>normal_x</td>
            <td>f4le</td>
            <td>The x-coordinate of the normal vector.</td>
        </tr>
        <tr>
            <td>normal_y</td>
            <td>f4le</td>
            <td>The y-coordinate of the normal vector.</td>
        </tr>
        <tr>
            <td>normal_z</td>
            <td>f4le</td>
            <td>The z-coordinate of the normal vector.</td>
        </tr>
        <tr>
            <td>unknown1</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>u</td>
            <td>f4le</td>
            <td>The texture coordinate along the horizontal axis (x), from 0 to 1.</td>
        </tr>
        <tr>
            <td>v</td>
            <td>f4le</td>
            <td>The texture coordinate along the vertical axis (y), from 0 to 1.</td>
        </tr>
        <tr>
            <td>unknown2</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>bone_index</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown3</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>bone_pair_index0</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown4</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>bone_pair_index1</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown5</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>bone_pair_index2</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown6</td>
            <td>u1</td>
            <td></td>
        </tr>
    </tbody>
</table>

<h2>Type: OpaquePrimitiveHeader</h2>

<p>Description for model parts. In this case, the primitives are triangle
    strips. The triangle list can contain degenerate triangles that are used
    to separate strips.
</p>
<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>pad0</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>bone_count</td>
            <td>u4le</td>
            <td>Number of bones that this primitive depends on.</td>
        </tr>
        <tr>
            <td>bone_indices_offset</td>
            <td>u4le</td>
            <td>Offset from this header to a bone list. See bone_indices.</td>
        </tr>
        <tr>
            <td>bone_pairs_count</td>
            <td>u4le</td>
            <td>Number of bone pairs that this primitive depends on.</td>
        </tr>
        <tr>
            <td>bone_pairs_offset</td>
            <td>u4le</td>
            <td>Offset to a bone pair indices list. See bone_pair_indices.</td>
        </tr>
        <tr>
            <td>texture_index_count</td>
            <td>u4le</td>
            <td>Appears to be the texture indices for this primitive?</td>
        </tr>
        <tr>
            <td>texture_index_offset</td>
            <td>u4le</td>
            <td>Appears to be the texture index offset for this primitive?</td>
        </tr>
        <tr>
            <td>sampler_states_offset</td>
            <td>u4le</td>
            <td>From FF24, this is an offset to ADDRESSU, ADDRESSV, MAGFILTER and
                MINFILTER sampler states.
            </td>
        </tr>
        <tr>
            <td>material_type</td>
            <td>u1→MaterialType</td>
            <td>See FrozenFish24's SH2MapTools/Sh2ModelMaterialEditor/Model.py#L75</td>
        </tr>
        <tr>
            <td>unknown_byte0</td>
            <td>u1</td>
            <td>Possibly material-related, </td>
        </tr>
        <tr>
            <td>pose_index</td>
            <td>u1</td>
            <td>If zero, this primitive is always visible. Otherwise, it may be
                hidden and swapped out at various times, e.g. for James's hands.
            </td>
        </tr>
        <tr>
            <td>unknown_byte1</td>
            <td>u1</td>
            <td></td>
        </tr>
        <tr>
            <td>backface_culling</td>
            <td>u4le</td>
            <td></td>
        </tr>
        <tr>
            <td>unknown_float0</td>
            <td>f4le</td>
            <td>From FF24, reported to affect diffuse color somehow.</td>
        </tr>
        <tr>
            <td>unknown_float1</td>
            <td>f4le</td>
            <td>From FF24, reported to affect ambient color somehow.</td>
        </tr>
        <tr>
            <td>specular_scale</td>
            <td>f4le</td>
            <td>From FF24, larger value = smaller specular.</td>
        </tr>
        <tr>
            <td>unknown_section0</td>
            <td>Unknown purpose.</td>
            <td></td>
        </tr>
        <tr>
            <td>pad1</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>diffuse_color</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>pad2</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>ambient_color</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>pad3</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>specular_color</td>
            <td>f4le</td>
            <td></td>
        </tr>
        <tr>
            <td>pad4</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>primitive_start_index</td>
            <td>u4le</td>
            <td>Offset into the triangle index array where the primitive begins.</td>
        </tr>
        <tr>
            <td>primitive_length</td>
            <td>u4le</td>
            <td>The length of the primitive in the triangle index array.</td>
        </tr>
        <tr>
            <td>pad5</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>bone_indices</td>
            <td>u2le</td>
            <td>The bone index array from this primitive. An important point is that
                the bone indices specified by a given vertex go into this array, not
                the overall skeleton array. Unclear why these are u2 if bones are u1?
            </td>
        </tr>
        <tr>
            <td>bone_pair_indices</td>
            <td>IndexList</td>
            <td>A list of bone pair indices.
            </td>
        </tr>
        <tr>
            <td>texture_indices</td>
            <td>IndexList</td>
            <td>A list of texture indices</td>
        </tr>
        <tr>
            <td>sampler_states</td>
            <td>[u1, u1, u1, u1]</td>
           <td></td>
        </tr>
    </tbody>
</table>

<h3>Enum: material_type</h3>

<table>
    <tbody>
        <tr>
            <th>Name</th>
        </tr>
        <tr>
            <td>unlit</td>
        </tr>
        <tr>
            <td>matte</td>
        </tr>
        <tr>
            <td>matte_plus</td>
        </tr>
        <tr>
            <td>glossy</td>
        </tr>
    </tbody>
</table>

<h2>Type: Geometry</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Note</th>
        </tr>
        <tr>
            <td>opaque_primitive_headers</td>
            <td>OpaquePrimitiveHeaderWrapper</td>
            <td></td>
        </tr>
        <tr>
            <td>opaque_triangle_indices</td>
            <td>IndexList</td>
            <td>List of vertex indices, which represent triangle strips.</td>
        </tr>
        <tr>
            <td>opaque_vertex_list</td>
            <td>OpaqueVertexData</td>
            <td></td>
        </tr>
        <tr>
            <td>transparent_primitive_headers</td>
            <td>TransparentPrimitiveHeaderWrapper</td>
            <td></td>
        </tr>
        <tr>
            <td>transparent_triangle_indices</td>
            <td>IndexList</td>
            <td>List of vertex indices, which represent triangle strips.</td>
        </tr>
        <tr>
            <td>transparent_vertex_list</td>
            <td>TransparentVertexData</td>
            <td></td>
        </tr>
    </tbody>
</table>

<h2>Type: TextureData</h2>

<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>magic</td>
            <td>0x01 0x09 0x99 0x19 is the start of the texture header.</td>
        </tr>
        <tr>
            <td>unknown</td>
            <td></td>
        </tr>
        <tr>
            <td>textures</td>
            <td>TextureContainer</td>
        </tr>
    </tbody>
</table>

<h2>Type: TransformationMatrix</h2>

<p>Represents a 4x4 column-major transformation matrix.
</p>
<table>
    <tbody>
        <tr>
            <th>ID</th>
            <th>Type</th>
        </tr>
        <tr>
            <td>rotation00</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation10</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation20</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>pad0</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation01</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation11</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation21</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>pad1</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation02</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation12</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>rotation22</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>pad2</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>translation_x</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>translation_y</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>translation_z</td>
            <td>f4le</td>
        </tr>
        <tr>
            <td>translation_w</td>
            <td>f4le</td>
        </tr>
    </tbody>
</table>
