---
name: formula-latex-normalizer
description: normalize, rewrite, and generate latex equations for mechanical, robotics, and control papers. use when the user wants to (1) convert natural-language math descriptions into paper-ready latex, (2) standardize existing equations to a consistent symbol system, (3) extract equations from screenshots or pdf snippets and rewrite them in latex, or (4) explain what was changed and why under an engineering-focused notation standard.
---

# Formula Latex Normalizer

Convert rough math into publication-ready LaTeX for mechanical, robotics, and control writing.

## Core behavior

Always produce output that is consistent, explicit, and easy to paste into a paper.

Default notation policy:
- Scalars: italic, e.g. `q_{i6}`, `t`, `m`
- Vectors: bold lowercase via `\vect{}`, e.g. `\vect{p}`, `\vect{\rho}_i`
- Matrices: bold uppercase via `\mat{}`, e.g. `\mat{R}`, `\mat{J}`
- Unit vectors: hat + bold via `\uvect{}`, e.g. `\uvect{e}_{6,i}`
- Variable indices: italic, e.g. `i,j,k`
- Descriptive subscripts: upright roman with `\mathrm{}`, e.g. `T_{\mathrm{ref}}`
- Operators and function names: upright, e.g. `\sin`, `\cos`, `\det`, `\mathrm{tr}`
- Differential symbol: upright, e.g. `\mathrm{d}t`
- Units: upright with a space after the number, e.g. `10\ \mathrm{mm}`

Default macro set:
```latex
\newcommand{\vect}[1]{\boldsymbol{#1}}
\newcommand{\mat}[1]{\boldsymbol{#1}}
\newcommand{\uvect}[1]{\hat{\boldsymbol{#1}}}
```

## Workflow

### 1. Determine the request type
Classify the user's request into one of these modes:

- **Generate**: the user describes a formula in natural language and wants LaTeX.
- **Normalize**: the user provides an existing formula and wants notation repaired.
- **Recognize + normalize**: the user provides a screenshot, scanned snippet, or figure containing a formula and wants LaTeX plus cleanup.
- **Explain changes**: the user wants to know what was corrected and why.

A request can combine multiple modes. In that case, do recognition first, then normalization, then explanation.

### 2. Infer symbol roles before editing
Before rewriting, infer what each symbol represents.

Use these engineering heuristics:
- Position, displacement, velocity, acceleration, force, moment, and direction quantities are often vectors.
- Rotation, Jacobian, mass, stiffness, and transformation objects are often matrices.
- Joint coordinates, scalar distances, time, masses, gains, and single generalized coordinates are usually scalars.
- A scalar multiplied by a unit vector yields a vector term.
- A matrix multiplied by a vector yields a vector term.
- If an equation is a sum of vector terms, rewrite it as a vector equation consistently.

If a symbol role is ambiguous, do not silently force a risky interpretation. State the assumption briefly in the change notes.

### 3. Normalize using the house style
Apply these corrections consistently:

#### Object classes
- Convert scalar variables to plain math italics.
- Convert vectors to `\vect{}`.
- Convert matrices to `\mat{}`.
- Convert unit vectors to `\uvect{}`.

#### Subscripts and superscripts
- Keep index variables italic: `q_{i6}`, `a_j`, `J_{ij}`.
- Use upright roman for descriptive labels: `T_{\mathrm{ref}}`, `F_{\mathrm{ext}}`, `q_{\max}`.
- In mixed subscripts, keep labels upright and indices italic: `\mat{J}_{\mathrm{F},i}^{-1}`.
- Do not bold commas or numeric indices.
- Use upright transpose: `\mat{J}^{\mathrm T}`.

#### Operators and constants
- Use built-in operator forms where possible: `\sin`, `\cos`, `\ln`, `\exp`, `\det`.
- Use `\mathrm{tr}` or `\operatorname{tr}` consistently for trace.
- Keep inverse and pseudoinverse as standard superscripts: `\mat{R}^{-1}`, `\mat{J}^{+}`.
- Use upright differential: `\mathrm{d}t`, `\mathrm{d}q`.
- Keep units upright and separated from numbers by a space.

#### Spacing and readability
- Add spaces between operators and symbols when this improves readability.
- Avoid cramped inline forms like `sin\theta` when `\sin \theta` is intended.
- Prefer structurally clear expressions over minimal keystrokes.

### 4. Handle screenshots and image formulas
When the source is an image or screenshot:
- Read the formula carefully from the image.
- Preserve the mathematical meaning first; cosmetic cleanup comes second.
- Reconstruct missing grouping symbols if they are visually obvious.
- If one or two characters are uncertain, flag them in the notes instead of guessing confidently.
- After transcription, normalize to the house style.

If the image contains multiple equations, separate them and keep their order.

### 5. Output format
Unless the user asks for something else, return exactly these two sections in this order:

#### 1) `LaTeX`
Provide the cleaned final expression in a fenced LaTeX block.

#### 2) `修改说明`
List the notation changes briefly. Focus on the highest-value fixes such as:
- scalar/vector/matrix distinction
- italic versus upright subscripts
- operator formatting
- differential symbol formatting
- unit formatting
- ambiguity assumptions

Do not bury the final formula under a long essay.

## Default conventions for this domain
Use these defaults unless the user explicitly wants a different style:

- Mechanical, robotics, and control papers prefer bold vectors and bold matrices instead of arrow notation.
- Do not mix arrow vectors and bold vectors in one answer.
- For expressions like `q_{i6}\uvect{e}_{6,i}`, treat `q_{i6}` as a scalar and the full product as a vector term.
- Typical preferred forms include:

```latex
\vect{r}_i^{\,0} + q_{i1}\uvect{e}_x + q_{i2}\uvect{e}_z
= \vect{p} + \mat{R}\vect{a}_i - q_{i6}\uvect{e}_{6,i}
```

```latex
\vect{\rho}_i := \vect{r}_i - \vect{p} = \mat{R}\vect{a}_i - q_{i6}\uvect{e}_{6,i}
```

```latex
\vect{v} + \vect{\omega} \times \vect{\rho}_i = \mat{E}_i\dot{\vect{q}}_i
```

## Response patterns

### Pattern A: natural-language to LaTeX
Input example:
- "第 i 条支链的位置向量等于平台位置加旋转矩阵乘以局部点，再减去第 6 个关节位移乘单位向量。"

Output style:
```text
LaTeX
```latex
\vect{r}_i = \vect{p} + \mat{R}\vect{a}_i - q_{i6}\uvect{e}_{6,i}
```

修改说明
- 将 `\vect{r}_i`、`\vect{p}`、`\vect{a}_i` 统一为向量粗体。
- 将 `\mat{R}` 统一为矩阵粗体大写。
- 将 `q_{i6}` 处理为标量，`\uvect{e}_{6,i}` 处理为单位向量。
```

### Pattern B: normalize an existing formula
Input example:
- `r_i^0+q_i1e_x+q_i2e_z=p+Ra_i-q_i6e_6,i`

Output style:
```text
LaTeX
```latex
\vect{r}_i^{\,0} + q_{i1}\uvect{e}_x + q_{i2}\uvect{e}_z
= \vect{p} + \mat{R}\vect{a}_i - q_{i6}\uvect{e}_{6,i}
```

修改说明
- 将位置项统一改为向量写法，旋转项改为矩阵乘向量形式。
- 规范了复合下标 `i1`、`i2`、`i6` 的写法。
- 将单位向量统一改为带帽粗体形式。
- 修正了上下标分组与间距。
```

### Pattern C: screenshot to LaTeX
When the user uploads an image with a formula:
- transcribe the formula,
- normalize it,
- then explain the changes.

If any character is uncertain, say so explicitly in `修改说明`.

## Use bundled references
Consult `references/symbol-style-guide.md` when the request involves notation policy disputes, mixed subscripts, or examples from mechanical / robotics / control notation.
