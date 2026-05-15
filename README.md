Norm of a complex vector

  - Length: ‖v‖ = √(|v₁|² + |v₂|² + …)
  - Formula: for v = [2j; -1], ‖v‖ = √(4 + 1) = √5
  - MATLAB: norm(v)
  - Exam note: for complex entries, use squared magnitudes |vᵢ|², not vᵢ².

  Unit vector (normalization)

  - A vector divided by its own norm has length 1: ‖v/‖v‖‖ = 1
  - Example: [1;-2j;0]/√5 is the unit vector along [1;-2j;0]
  - MATLAB: v/norm(v)

  Conjugate transpose

  - vᴴ (written v' in MATLAB) = transpose + conjugate each entry
  - Example: v = [2j; -1] → vᴴ = [-2j, -1]
  - Plain transpose v.' flips shape only, no conjugation
  - Exam note: use v' for inner products; v.' for plain matrix equations

  Null space (kernel)

  - null(M) = { x : M·x = 0 } — vectors that get mapped to zero
  - MATLAB null(M): returns orthonormal basis as columns
  - Example: null(a') returns unit vector perpendicular to a
  - Dimension of null space = (columns of M) − rank(M)

  Orthonormal vectors

  - Each unit length + each pair perpendicular
  - Test: |v|² = 1 for each, vᵢᴴ vⱼ = 0 for i ≠ j
  - Matrix form: columns of V orthonormal ⟺ VᴴV = I

  Unitary matrix

  - Square matrix with orthonormal columns (and rows)
  - QᴴQ = QQᴴ = I → Q⁻¹ = Qᴴ
  - Geometrically: rotation/reflection in ℂⁿ — preserves length and angles
  - Example: Q = [a/‖a‖, null(a')] is 2×2 unitary

  Isometry (length-preserving map)

  - ‖Cx‖ = ‖x‖ for all x ⟺ CᴴC = I (columns orthonormal)
  - Can be non-square (e.g. 3×2: embeds 2-D into 3-D without stretching)
  - Difference from unitary: unitary = square isometry
  - Exam note: Ca = b is only solvable by an isometry if ‖b‖ = ‖a‖

  Matrix-vector multiplication

  - Rule: (m×n)·(n×1) → (m×1). Inner dimensions must match.
  - Entry i of result = (row i of M) · (column vector) = Σₖ M(i,k)·v(k)
  - Geometrically: linear transformation applied to a vector

  Composition of orthonormal maps

  - Product of two isometries / unitaries is still an isometry
  - Used in part (d): C = P·Q' where Q unitary, P has orthonormal columns
  - Proof: (PQ')ᴴ(PQ') = Q PᴴP Q' = Q·I·Q' = I
  - Pattern: build C from two pieces — one per side — to control both
  input/output behavior

  Construction pattern (image-fixing isometry)

  - Goal: build C with C·a = b and CᴴC = I
  - Steps:
    a. Q = orthonormal basis of input space starting with a/‖a‖
    b. P = orthonormal basis of output space starting with b/‖b‖
    c. C = P·Qᴴ
  - Works because Qᴴ·a = [‖a‖; 0; …] (puts a onto axis 1), then P maps axis
  1 to b/‖b‖, scaled by ‖a‖ = ‖b‖.


  % C is an isometry in C^(3x2) with C*a = b0
  b0 = [1; -2j; 0];
  Q  = [a/norm(a), null(a')];        % 2x2 unitary basis containing a
  Nb = null(b0');                    % 3x2 orthonormal complement of b0
  P  = [b0/norm(b0), Nb(:,1)];       % 3x2 with orthonormal columns
  C  = P * Q';                       % isometry: C'*C = I2 and C*a = b0