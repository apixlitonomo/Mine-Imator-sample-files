##This is a guide on the mineimator code that loads complex rigs like Jenny belle and other rigs. (made incase someone wants to make an importer)


##Priority 1 — Model loading & hierarchy (start here)


model_file_load/ (and siblings like model_file_load_part, model_file_load_shape)

Parses .mimodel JSON into parts/shapes; sets position, rotation, scale, bend inheritance



model_file_load_shape

Explicit shape loading (from/to, UV, position, rotation, scale, bend flags) — see the raw GML that matches your sample planes


model_part_* (any scripts starting with model_part_)

Part hierarchy, matrices, bend matrix helpers



model_shape_* (especially model_shape_generate_plane, model_shape_generate_block, model_shape_get_bend, model_part_get_bend_matrix)

How shapes are generated after bend + transform


##Priority 2 — Bend system (very common cause of displaced geometry)

model_shape_generate_plane

Full bend slicing logic (axis, offset, size, upper/lower/left/right, sharp vs realistic)



Any script containing bend in the name (e.g. bend matrix, bend angle, bend part)

Offset, size, part side, invert, axis list



Related helpers that call model_part_get_bend_matrix or model_shape_get_bend

Weighted rotation around the bend pivot



##Priority 3 — Transforms, matrices, pivots / rotation points

scripts with matrix_ in the name (matrix_create, matrix_build, matrix_multiply, etc.)

Exact composition order: position × rotation × scale × −rot_point



MatrixType.cpp + Matrix.hpp

Engine matrix implementation


Scripts that read/write rot_point / rotation point on timelines or parts

Default pivot (0, −8, 0) for shapes vs custom pivots on bodyparts


Priority 4 — Shape / primitive generators

Any vbuffer_create_* or vbuffer_add_triangle scripts

Winding, normals, UV layout, invert flag


Priority 5 — Supporting structure (only if the above is insufficient)

anything with timeline_, bodypart_, inherit

Parent/child links and inherit.position/rotation/scale


MathFunc.cpp + RenderFunc.cpp

Math and render helpers used by the generated code









