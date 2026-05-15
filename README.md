# Signal Processing — Linear Algebra Notes (5LIK0)

---

## Norm of a complex vector

**What it is:** The "length" of a vector — one number that tells you how big the vector is.

**Formula:** ‖v‖ = √(|v₁|² + |v₂|² + …)

> For complex numbers, you must take the magnitude squared (|vᵢ|²), NOT just square the entry (vᵢ²).
> Why: |2j|² = 4 ✓ but (2j)² = −4 ✗ — you'd get a negative under the square root.

**Example:**
```
v = [2j; -1]
‖v‖ = √(|2j|² + |-1|²) = √(4 + 1) = √5 ≈ 2.24
```

**MATLAB:** `norm(v)` → single positive number

**Remember:** ‖v‖ (double bars) = length of a vector. |x| (single bars) = size of one number.

---

## Unit vector (normalisation)

**What it is:** A vector that points in the same direction but has length exactly 1.
Think of it as: "same direction, but rescaled to length 1."

**Formula:** unit vector = v / ‖v‖

**Example:**
```
a = [2j; -1],  ‖a‖ = √5

unit vector = [2j/√5; -1/√5]

Check: ‖[2j/√5; -1/√5]‖ = √(4/5 + 1/5) = √1 = 1  ✓
```

**MATLAB:** `v / norm(v)`

---

## Conjugate transpose

**What it is:** You do two things at once:
1. Flip the vector from column to row (or row to column) — this is the "transpose"
2. Flip the sign of all imaginary parts — this is the "conjugate"

**Notation:** vᴴ in math, `v'` in MATLAB

**Example:**
```
v = [2j; -1]        ← 2×1 column
vᴴ = [-2j, -1]      ← 1×2 row, imaginary part flipped (2j → -2j)
```

**Plain transpose (no conjugate):** `v.'` in MATLAB — only flips shape, does NOT flip imaginary parts.

> **When to use which:**
> - `v'`  (conjugate transpose) → for inner products and orthogonality checks
> - `v.'` (plain transpose) → for plain matrix equations like B·a = 0

---

## Complex number arithmetic

**The key rule:** j² = −1

**How multiplication works:**
| Left × Right | Result type |
|---|---|
| real × real | real |
| real × imaginary | imaginary |
| imaginary × imaginary | real (sign flips!) |

**Example — full breakdown:**
```
(-2j) × (0.05 + 0.44j)
= (-2j)(0.05) + (-2j)(0.44j)
= -0.10j        + (-0.88 × j²)
= -0.10j        + (-0.88 × -1)
= -0.10j        + 0.88
= 0.88 - 0.10j
```

> **Exam note:** "imaginary × imaginary → real" is why complex inner products can cancel out to zero.

---

## Absolute value vs norm

**|x|** — single bars — size of ONE number (real or complex)
```
|3 - 4j| = √(3² + 4²) = √(9 + 16) = 5
```

**‖v‖** — double bars — length of a WHOLE vector
```
v = [3-4j; 0]
‖v‖ = √(|3-4j|² + |0|²) = √(25 + 0) = 5
```

**In MATLAB:**
- `abs(v)` → gives a vector of |vᵢ| for each entry separately
- `norm(v)` → gives one number: the total length

```matlab
b = [1+1j; 3];
abs(b)   % → [√2; 3]   (two numbers, one per entry)
norm(b)  % → √11       (one number: total length)
```

---

## Inner product (complex dot product)

**What it is:** Multiply two vectors together to get a single number.
You pair up entries, multiply, and add everything.

**Formula:** bᴴ·a = sum of (conjugate of each bᵢ) × (corresponding aᵢ)

**What it tells you:**
- = 0 → vectors are perpendicular (orthogonal)
- Large value → vectors point in similar directions

**Cauchy-Schwarz inequality:** |bᴴ·a| ≤ ‖b‖ · ‖a‖
- Equality happens only when b is parallel to a (b = λ·a for some number λ)
- Useful for: "find c that maximises |cᴴ·a|" → answer is c parallel to a

**Example:**
```
b = [3/√5; -6j/√5],  a = [2j; -1]

bᴴ·a = conj(3/√5)·(2j) + conj(-6j/√5)·(-1)
     = (3/√5)(2j)  +  (6j/√5)(-1)
     = 6j/√5 - 6j/√5
     = 0   ← perpendicular ✓
```

---

## Null space (kernel)

**What it is:** All vectors x such that M·x = 0.
Plain English: "what inputs does the matrix turn into zero?"

**Formula:** null(M) = { x : M·x = 0 }

**MATLAB:** `null(M)` → returns the answer as columns (orthonormal)

**Dimension:** number of columns of M − rank(M)

**Examples:**
```matlab
null(a')   % perpendicular to a (conjugate sense) — use for orthogonality
null(a.')  % perpendicular to a (plain sense)     — use for B·a = 0
```

```
a = [2j; -1] (2×1)

null(a') returns a 2×1 unit vector b where bᴴ·a = 0
→ e.g. b_unit = [1/√5; 2j/√5]

To get norm 3:  b = 3 × b_unit
```

> **Pitfall:** `null(M)` always returns UNIT vectors (norm = 1). If you need a different norm, multiply afterwards.
> **Phase ambiguity:** null can return any rotation of the answer (multiply by eʲθ) — all are correct.

---

## Orthonormal vectors

**What it is:** A set of vectors where:
1. Every vector has length 1 (unit length)
2. Every pair of different vectors is perpendicular

**Think of it as:** a "clean coordinate system" — like x/y/z axes, but can be rotated and work in complex space.

**Tests:**
```
Each vector: ‖vᵢ‖ = 1
Each pair:   vᵢᴴ · vⱼ = 0   (for i ≠ j)
```

**Matrix form:** Stack vectors as columns into matrix V.
Then: VᴴV = I (identity matrix) ⟺ columns are orthonormal

**Example:**
```
v1 = [1; 0],  v2 = [0; 1]   ← standard axes, orthonormal ✓

v1 = [1; -2j]/√5,  v2 = [2j; 1]/√5   ← rotated complex version, also orthonormal ✓
```

---

## Unitary matrix

**What it is:** A square matrix whose columns (and rows) are orthonormal.

**Key property:** QᴴQ = QQᴴ = I, so Q⁻¹ = Qᴴ (inverse = conjugate transpose)

**Geometrically:** A unitary matrix rotates or reflects space. It never stretches or shrinks anything — all lengths and angles are preserved.

**Useful trick:** If a is the first column direction of Q, then:
```
Q' · a = [‖a‖; 0]
```
It "aligns" a onto the first axis and zeros out everything else.

**Example:**
```matlab
a = [2j; -1];
Q = [a/norm(a), null(a')];  % 2×2 unitary matrix
% First column = a/‖a‖, second column = perpendicular unit vector
```

---

## Isometry (length-preserving map)

**What it is:** A matrix C that never changes the length of any vector it multiplies.

**Formula:** ‖Cx‖ = ‖x‖ for all x ⟺ CᴴC = I

**Difference from unitary:**
- Unitary = square (n×n), isometry can be tall (m×n with m > n)
- A 3×2 isometry takes 2D vectors and embeds them into 3D without stretching

**When is Cx = b solvable by an isometry?**
Only when ‖b‖ = ‖x‖ (lengths must match, since C preserves length)

**Example (Assignment 1d):**
```
a = [2j; -1],     ‖a‖ = √5
b₀ = [1; -2j; 0], ‖b₀‖ = √(1 + 4 + 0) = √5  ← same length ✓
→ an isometry C (3×2) with C·a = b₀ exists
```

---

## Matrix-vector & matrix-matrix multiplication

**Size rule:** (m×n) · (n×p) → (m×p)
The inner numbers must match. The outer numbers give the result size.

**Each entry:** result(i,j) = (row i of left) · (column j of right) = Σₖ A(i,k)·B(k,j)

**Examples from Assignment 1:**
```
bᴴ · a    → (1×2)·(2×1) = scalar       (inner product)
B · a     → (2×2)·(2×1) = 2×1 vector  (kernel test: should = 0)
C · a     → (3×2)·(2×1) = 3×1 vector  (apply isometry)
Cᴴ · D   → (2×3)·(3×2) = 2×2 matrix
```

**Important:** NOT commutative — A·B ≠ B·A in general.

---

## Element-wise vs matrix operations (the dot)

A dot before an operator means "do it to each entry separately."

| Operator | Meaning |
|---|---|
| `A * B` | matrix multiply |
| `A .* B` | element-wise multiply (entry × entry) |
| `A ^ 2` | matrix power: A·A |
| `A .^ 2` | element-wise square each entry |
| `A / B` | matrix right-divide (≈ A·inv(B)) |
| `A ./ B` | element-wise divide |
| `A'` | conjugate transpose (Aᴴ) |
| `A.'` | plain transpose (shape flip only) |

---

## Composition of orthonormal maps

**Rule:** Multiplying two isometries/unitaries together gives another isometry.

**Proof:**
```
C = P · Qᴴ
CᴴC = (PQᴴ)ᴴ(PQᴴ) = Q·PᴴP·Qᴴ = Q·I·Qᴴ = I  ✓
```

**Used in:** Building C from two pieces P and Q to control both input and output behaviour.

---

## Construction pattern — image-fixing isometry

**Goal:** Build a matrix C (isometry) such that C·a = b₀

**Steps:**
```
1. Build Q: orthonormal basis of input space, first column = a/‖a‖
   Q = [a/‖a‖,  null(a')]        ← 2×2 unitary

2. Build P: orthonormal basis of output space, first column = b₀/‖b₀‖
   Nb = null(b₀')                ← 3×2 (perpendicular plane to b₀)
   P  = [b₀/‖b₀‖,  Nb(:,1)]     ← 3×2 with orthonormal columns

3. C = P · Qᴴ
```

**Why it works — step by step:**
```
C · a = P · Qᴴ · a
      = P · [‖a‖; 0]     ← Qᴴ aligns a onto axis 1
      = ‖a‖ · P(:,1)     ← P maps axis 1 to b₀/‖b₀‖
      = ‖a‖ · b₀/‖b₀‖
      = b₀                ← because ‖a‖ = ‖b₀‖  ✓
```

---

## Exploiting CᴴC = I (Assignment 1e)

**Problem:** Given CᴴD = M, find D.

**Solution:** D = C · M

**Why:** Cᴴ·(C·M) = (CᴴC)·M = I·M = M ✓

Plain English: when the left factor is an isometry, you can "undo" it by multiplying with C on the left.

---

## Sizes recap (Assignment 1, Problem 1)

| Variable | Size | Type |
|---|---|---|
| a | 2×1 | column vector ℂ² |
| b, c | 2×1 | column vectors ℂ² |
| B | 2×2 | matrix, kernel contains a |
| C | 3×2 | isometry |
| D | 3×2 | so that Cᴴ·D is 2×2 |
| b₀ | 3×1 | target vector for part (d) |

---

## Common pitfalls

| Mistake | Fix |
|---|---|
| Using `a'` when equation is plain (B·a = 0) | Use `null(a.')` not `null(a')` |
| Treating `abs(b)` as `norm(b)` | `abs` is element-wise; `norm` gives one number |
| Trying to solve b'·a = 0 algebraically | Use `null()` to get the solution set |
| Forgetting `null()` returns UNIT vectors | Multiply by desired norm afterwards |
| Writing b·a (column × column) | Must be bᴴ·a (row × column) |
| Forgetting `null()` returns COLUMNS always | Result columns even if input was a row |
