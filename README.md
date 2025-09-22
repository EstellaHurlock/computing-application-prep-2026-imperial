# computing-application-prep-2026-imperial
cd "/Users/estella/git-practice/imperial-app-prep/computing-application-prep-2026-imperial"

mkdir -p algorithms maths/evidence

# README.md 
cat > README.md <<'EOF'
# Imperial MSc Computing (Conversion) – Prep Portfolio

This repository tracks my short sprint prep for Imperial: maths fundamentals and core algorithms, plus small Python snippets.

## Structure
- `algorithms/` – small Python implementations (e.g., Gram–Schmidt).
- `maths/` – concise notes and scanned handwritten evidence.
- `STUDY_LOG.md` – dated entries of what I covered and produced.

EOF

# Study log
cat > STUDY_LOG.md <<'EOF'
# Study Log

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
