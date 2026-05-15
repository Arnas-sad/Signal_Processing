Norm of a complex vector

  - Length: ‖v‖ = √(|v₁|² + |v₂|² + …)
  - Plain: how long the vector "arrow" is (Pythagoras generalised to n dims).
  - For complex entries use squared magnitudes |vᵢ|², not vᵢ². Reason: |2j|² = 4, but (2j)² = -4.
  - Example: v = [2j; -1] → ‖v‖ = √(|2j|² + |-1|²) = √(4 + 1) = √5
  - MATLAB: norm(v) — single number, always ≥ 0
  - Exam note: ‖v‖ uses double bars (vector length). |x| uses single bars (scalar magnitude). Don't mix.

  Unit vector (normalization)

  - A vector divided by its own norm has length 1: ‖v/‖v‖‖ = 1
  - Plain: strip away length, keep only direction.
  - Example: a = [2j; -1] with ‖a‖ = √5 → a/√5 = [2j/√5; -1/√5]; new length = 1.
  - Dividing a vector by a scalar = each entry divided by that number. Shape stays the same.
  - MATLAB: v/norm(v)

  Conjugate transpose

  - vᴴ (written v' in MATLAB) = transpose (column→row) + conjugate each entry (j → -j).
  - Plain: turn the vector on its side AND flip the signs of imaginary parts.
  - Example: v = [2j; -1] → vᴴ = [-2j, -1] (1×2 row, imag flipped)
  - Plain transpose v.' flips shape only — same values, just rotated.
  - Why conjugate: makes the inner product land on 0 for perpendicular complex vectors.
    Check: with b = [3/√5; -6j/√5] and a = [2j; -1]:
      b'·a = (3/√5)·(2j) + (6j/√5)·(-1) = 6j/√5 − 6j/√5 = 0  ✓
      b.'·a = (3/√5)·(2j) + (-6j/√5)·(-1) = 12j/√5 ≠ 0  ✗
  - Exam note: use v' for inner products / orthogonality; v.' for plain matrix equations like B·a = 0.

  Complex number arithmetic (the rule that makes everything work)

  - j² = i² = -1.
  - Multiplication breakdown:
      real × real  → real
      real × imag  → imag
      imag × imag  → real (sign flips because j² = -1)
  - Example: (-2j)·(0.0497 + 0.4444j)
      = (-2j)·0.0497 + (-2j)·(0.4444j)
      = -0.0994j      + (-0.8888 · j²)
      = -0.0994j      + 0.8888
      = 0.8888 - 0.0994j
  - Exam note: "imag × imag → real" is the trick behind every complex inner-product cancellation.

  Absolute value vs norm

  - |x| — single bars — absolute value/magnitude of one number (real or complex).
      Example: |3 - 4j| = √(9 + 16) = 5
  - ‖v‖ — double bars — length of a whole vector.
  - abs(v) in MATLAB: element-wise magnitude → returns a vector of |vᵢ|. NOT norm(v).
      Example: b = [1+i; 3] → abs(b) = [√2; 3], but norm(b) = √(2 + 9) = √11.

  Inner product (complex dot product)

  - bᴴ·a = (row × column) → a single complex number.
  - Plain: pair entries up, multiply with conjugation on the left, add results.
  - Test for perpendicular: bᴴ·a = 0.
  - Cauchy-Schwarz: |bᴴ·a| ≤ ‖b‖·‖a‖, with equality iff b is parallel to a (b = λ·a).
  - Example (part 1b): max |cᴴ·a| with ‖c‖ = √10 → c parallel to a, value = √10·√5 = √50.

  Null space (kernel)

  - null(M) = { x : M·x = 0 } — all vectors that M sends to zero.
  - Plain: things that the matrix "kills."
  - MATLAB null(M): returns orthonormal basis as columns (always columns, regardless of input shape).
  - Dimension = (columns of M) − rank(M).
  - Examples:
      null(a)    → empty for a a 2×1 column (only x = 0 satisfies a·x = 0)
      null(a')   → 2×1 unit vector perpendicular to a in the inner-product sense
      null(a.')  → 2×1 unit vector x with a.'·x = 0 (plain product, no conjugate)
      null(b0')  → 3×2 (b0 lives in 3-D; the plane perpendicular to it needs 2 vectors of 3 entries)
  - Phase ambiguity: null returns a unit vector unique only up to a phase eʲθ — many "different-looking" answers are all correct.

  Orthonormal vectors

  - Each unit length AND each pair perpendicular.
  - Test: |vᵢ| = 1 for each, vᵢᴴ·vⱼ = 0 for i ≠ j.
  - Matrix form: columns of V orthonormal ⟺ VᴴV = I.
  - Example: { [1;0], [0;1] } is orthonormal. So is { [1;-2j]/√5, [2j;1]/√5 }.

  Unitary matrix

  - Square matrix with orthonormal columns (and rows).
  - QᴴQ = QQᴴ = I → Q⁻¹ = Qᴴ.
  - Geometrically: rotation/reflection in ℂⁿ — preserves length and angles.
  - Example: Q = [a/‖a‖, null(a')] is 2×2 unitary.
  - Trick: Q'·a = [‖a‖; 0] when a is the first column direction of Q (zeroes the perpendicular part).

  Isometry (length-preserving map)

  - ‖Cx‖ = ‖x‖ for all x ⟺ CᴴC = I (columns orthonormal).
  - Can be non-square: e.g. 3×2 embeds 2-D into 3-D without stretching.
  - Unitary = square isometry; isometry = generalisation that allows tall matrices.
  - Solvability: Cx = b only has an isometry solution if ‖b‖ = ‖x‖.
      Example part 1d: ‖a‖ = √5 and ‖[1;-2j;0]‖ = √5 → consistent.

  Matrix-vector & matrix-matrix multiplication

  - Rule: (m×n)·(n×p) → (m×p). Inner dimensions must match; outer give result size.
  - Entry (i,j) of result = (row i of left) · (column j of right) = Σₖ A(i,k)·B(k,j).
  - Examples from the assignment:
      bᴴ·a    (1×2)·(2×1) → scalar (inner product)
      B·a     (2×2)·(2×1) → 2-vector (the kernel test)
      C·a     (3×2)·(2×1) → 3-vector
      CᴴD     (2×3)·(3×2) → 2×2 matrix
  - Not commutative: A·B ≠ B·A in general.
  - Associative: (A·B)·C = A·(B·C).

  Element-wise vs matrix operations (the dot)

  - A*B   matrix multiply (rules above)
  - A.*B  element-wise multiply (same shape required)
  - A^2   matrix power: A·A
  - A.^2  element-wise square
  - A/B   matrix right-divide (≈ A·inv(B))
  - A./B  element-wise divide
  - A'    conjugate transpose
  - A.'   plain transpose (shape flip only)
  - Rule of thumb: a dot before an operator means "do it entry-by-entry."

  Composition of orthonormal maps

  - Product of two isometries / unitaries is still an isometry.
  - Used in part (d): C = P·Q' where Q unitary, P has orthonormal columns.
  - Proof: (PQ')ᴴ(PQ') = Q·PᴴP·Q' = Q·I·Q' = I.
  - Pattern: build C from two pieces — one per side — to control both input and output behaviour.

  Construction pattern (image-fixing isometry)

  - Goal: build C with C·a = b₀ and CᴴC = I.
  - Steps:
      1. Q = orthonormal basis of input space starting with a/‖a‖.
         e.g. Q = [a/‖a‖, null(a')]  (2×2 unitary)
      2. P = orthonormal basis of output space starting with b₀/‖b₀‖.
         e.g. P = [b₀/‖b₀‖, Nb(:,1)] where Nb = null(b₀')  (3×2 with orthonormal columns)
      3. C = P·Qᴴ.
  - Why it works (trace through):
      C·a = P·Q'·a
          = P·[‖a‖; 0]      ← Q' aligns a onto axis 1 (the rest is zero)
          = ‖a‖·P(:,1)       ← P pulls axis 1 to b₀/‖b₀‖
          = ‖a‖·b₀/‖b₀‖
          = b₀                ← because ‖a‖ = ‖b₀‖
  - Indexing reminder: Nb(:,1) means "all rows, first column" → picks one perpendicular direction from the 2-D plane null(b₀').

  Exploiting CᴴC = I (part 1e)

  - When CᴴC = I, to satisfy Cᴴ·D = M, just choose D = C·M.
  - Why: Cᴴ·(C·M) = (CᴴC)·M = I·M = M.
  - Pattern: whenever the left factor is an isometry, "cancel" it by left-multiplying the target with C.

  Sizes recap (assignment 1, problem 1)

  - a       : 2×1 column (ℂ²)
  - b, c    : 2×1 columns (ℂ²)
  - B       : 2×2 (ℂ²ˣ², kernel contains a)
  - C       : 3×2 isometry (ℂ³ˣ²)
  - D       : 3×2 (so that Cᴴ·D is 2×2)
  - b₀      : 3×1 target for part (d)

  Common pitfalls

  - Using a' (conjugate) when the equation is plain (e.g. B·a = 0 needs null(a.'), not null(a')).
  - Treating abs(b) as if it were norm(b) — it's element-wise magnitude.
  - Trying to "isolate" b from b'·a = 0 algebraically — you can't divide by a row vector. Use null() to parametrise the solution set instead.
  - Forgetting that null(M) returns COLUMNS even if M is a row.
  - Writing b·a (column × column) — invalid; you need bᴴ·a.
  - Forgetting that null returns a UNIT vector → multiply by the desired norm.
