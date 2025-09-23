# computing-application-prep-2026-imperial
cd "/Users/estella/git-practice/imperial-app-prep/computing-application-prep-2026-imperial"

mkdir -p algorithms maths/evidence

# README.md 
cat > README.md <<'EOF'
# Imperial MSc Computing (Conversion) – Prep Portfolio

This repository tracks my short sprint prep for Imperial: maths fundamentals and core algorithms, plus small Python snippets.

## Structure
- `Computer Science/` – CS50: Introduction to Computer Science (changed from last weeks algorithms as I need to build my fundamentals foundations)
- `algorithms/` – small Python implementations (e.g., Gram–Schmidt).
- `maths/` – concise notes and scanned handwritten evidence.
- `STUDY_LOG.md` – dated entries of what I covered and produced.

# computing-application-prep-2026-imperial
cd "/Users/estella/git-practice/imperial-app-prep/computing-application-prep-2026-imperial"

EOF

# Study log
cat > STUDY_LOG.md <<'EOF'
# Study Log

## 2025-09-23
**Focus:** Linear algebra for transformations + orthogonality, plus Gram–Schmidt (theory + code)

**Topics covered**
- Reflecting In a Plane Example, using Gram-Schmidt Process
- Reflecting In a Plane Programming Assignment
- Proofs Discrete Mathematics, Induction workthroughs

**Work produced**
- Implemented Reflecting in a plane: (Code Below)
- Handwritten evidence planned in `maths/evidence/2025-09-23/` (photos)
- Written code of an induction proof

**Time:** ~24 hours total (updated)
EOF

# Reflecting In a Plane Code
cat > algorithms/PlaneReflection.py <<'EOF'
"""
Reflecting Bear (learning exercise).
Source: my practice solution inspired by Coursera/Imperial lab prompt.
"""

E  = gsBasis(bearBasis)          # 2x2 orthonormal columns
TE = np.array([[1., 0.],
               [0.,-1.]])        # reflect across first axis
T  = E @ TE @ E.T

# Induction Proof
\textbf{Claim.} For the sequence $x_{i+1}=6x_i-8x_{i-1}$ with $x_1=4$, $x_2=13$,
we have $x_i>0$ for all $i\ge 1$.

\textbf{Proof (strong induction with strengthened IH).}
Let $P(i)$ be: $(x_i>0)$ and $(x_i \ge 2x_{i-1}\text{ for }i\ge 2)$.

\emph{Base cases.} $x_1=4>0$. Also $x_2=13>0$ and $x_2\ge 2x_1$ since $13\ge 8$.

\emph{Inductive step.} Assume $P(k)$ holds for all $2\le k\le i$ (with $i\ge 2$).
Then $x_i>0$ and $x_i\ge 2x_{i-1}$, so $x_{i-1}\le \tfrac{1}{2}x_i$.
Hence
\[
x_{i+1}=6x_i-8x_{i-1}\ \ge\ 6x_i - 8\!\left(\tfrac{1}{2}x_i\right)
= 2x_i \ >\ 0.
\]
Thus $x_{i+1}>0$ and $x_{i+1}\ge 2x_i$, so $P(i+1)$ holds.

By strong induction, $x_i>0$ for all $i\ge 1$. \qed

# Uploading PDFs as updated folders onto github
Via Terminal
cd "/Users/estella/git-practice/imperial-app-prep/computing-application-prep-2026-imperial"
mkdir -p maths/evidence/2025-09-22 maths/evidence/2025-09-23

     # Making the dated evidence folder

for d in maths/evidence/2025-09-22 maths/evidence/2025-09-23; do
  { echo "# Evidence – ${d##*/}"; echo; 
    for f in "$d"/*.pdf; do [ -f "$f" ] && echo "- [$(basename "$f")](./$(basename "$f"))"; done; 
    echo; } > "$d/README.md"
done

     # After I drop PDFs into each folder, then auto-make folder indexes

git add -A
git commit -m "docs: log Day 8–9 and add evidence folders + indexes"
git push
 
     # Commit and push.

EOF

# Maths notes
cat > maths/notes.md <<'EOF'
# Maths Notes – 2025-09-22

- **Reflection in a plane**: \( r' = E * T_E * E^{-1} * r )
- **Proof by Induction**: Proof (we'll use), Base Case(more than 1), Inductive step (suppose), By the inductive process (proofs algebra), Completes inductive step (since LHS we have RHS)

EOF


EOF


## 2025-09-22
**Focus:** Linear algebra for transformations + orthogonality, plus Gram–Schmidt (theory + code)

**Topics covered**
- Einstein summation convention; symmetry of the dot product  
- Non-square matrix multiplication as applying a transform to many vectors  
- Projections via linear transforms (`r' = Ar`)  
- Changing basis & representing transforms in a new basis  
- Orthogonal matrices (`AᵀA = I`) & properties  
- Gram–Schmidt process (manual and coded)

**Work produced**
- Implemented Gram–Schmidt (fixed-size and general): `algorithms/gram_schmidt.py`
- Notes added in `maths/notes.md`
- Handwritten evidence planned in `maths/evidence/2025-09-22/` (photos/scans)

**Time:** ~24 hours total (update)
EOF

# Gram–Schmidt code
cat > algorithms/gram_schmidt.py <<'EOF'
"""
Gram–Schmidt implementations (learning exercise).
Source: my practice solution inspired by Coursera/Imperial lab prompt.
"""

import numpy as np
import numpy.linalg as la

verySmallNumber = 1e-14

def gsBasis(A):
    B = np.array(A, dtype=np.float_)
    for i in range(B.shape[1]):
        for j in range(i):
            B[:, i] = B[:, i] - (B[:, i] @ B[:, j]) * B[:, j]
        if la.norm(B[:, i]) > verySmallNumber:
            B[:, i] = B[:, i] / la.norm(B[:, i])
        else:
            B[:, i] = np.zeros_like(B[:, i])
    return B
EOF

# Maths notes
cat > maths/notes.md <<'EOF'
# Maths Notes – 2025-09-22

- **Einstein summation**: repeated indices are summed (e.g., \( r'_i = A_{ij} r_j \)).
- **Projection via A**: \( r' = r - \hat{s}\, (r \cdot \hat{e}_3)/s_3 \) where \( s_3 = \hat{s}\cdot\hat{e}_3 \) (used for shadows).
- **Changing basis**: columns of \(A\) are the images of the old basis vectors.
- **Orthogonal matrices**: \( A^\top A = I \), preserve lengths/angles.
- **Gram–Schmidt**: subtract projections onto previous orthonormal vectors; normalize if nonzero.
EOF
