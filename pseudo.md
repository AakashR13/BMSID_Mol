```
function calculate_atom_d_coordinates(A, B, C, bond_length_CD, bond_angle_BCD, torsion_angle_BC):
    bond_angle_BCD = to_radians(bond_angle_BCD)
    torsion_angle_BC = to_radians(torsion_angle_BC)

    AB = normalize(B - A)
    BC = normalize(C - B)

    perpendicular_vector = cross_product(AB, BC)

    rotation_matrix = calculate_rotation_matrix(BC, bond_angle_BCD)

    rotated_perpendicular_vector = matrix_multiply(rotation_matrix, perpendicular_vector)

    CD = scale(rotated_perpendicular_vector, bond_length_CD)

    torsion_rotation_matrix = calculate_rotation_matrix(BC, torsion_angle_BC)

    rotated_CD = matrix_multiply(torsion_rotation_matrix, CD)

    D = C + rotated_CD

    return D

function normalize(vector):
    return vector / magnitude(vector)

function calculate_rotation_matrix(axis, angle):
    norm_axis = normalize(axis)
    cos_angle = cosine(angle)
    sin_angle = sine(angle)
    x, y, z = norm_axis

    rotation_matrix = [
        [cos_angle + (1 - cos_angle) * x**2, (1 - cos_angle) * x * y - sin_angle * z, (1 - cos_angle) * x * z + sin_angle * y],
        [(1 - cos_angle) * x * y + sin_angle * z, cos_angle + (1 - cos_angle) * y**2, (1 - cos_angle) * y * z - sin_angle * x],
        [(1 - cos_angle) * x * z - sin_angle * y, (1 - cos_angle) * y * z + sin_angle * x, cos_angle + (1 - cos_angle) * z**2]
    ]

    return rotation_matrix

function scale(vector, factor):
    return vector * factor
```