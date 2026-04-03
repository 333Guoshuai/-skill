# Symbol Style Guide for Mechanical, Robotics, and Control Papers

This reference distills the bundled notation policy used by the skill.

## Core rules

1. Scalars use italic math symbols.
2. Vectors use bold lowercase symbols.
3. Matrices use bold uppercase symbols.
4. Unit vectors use hat + bold.
5. Variable indices are italic.
6. Descriptive subscripts use upright roman.
7. Function names, operators, differential symbols, and units use upright roman.
8. Keep one consistent notation system throughout the paper.

## Typical mappings

- Scalar: `q_{i6}`, `t`, `m`, `l`
- Vector: `\vect{p}`, `\vect{r}_i`, `\vect{\rho}_i`, `\vect{a}_i`
- Matrix: `\mat{R}`, `\mat{J}`, `\mat{M}`, `\mat{K}`
- Unit vector: `\uvect{e}_x`, `\uvect{e}_z`, `\uvect{e}_{6,i}`

## Subscripts

### Variable indices
Use italic indices for running indices:
- `q_{i6}`
- `a_j`
- `J_{ij}`

### Descriptive labels
Use upright roman for labels and state tags:
- `T_{\mathrm{ref}}`
- `F_{\mathrm{ext}}`
- `q_{\max}`
- `J_{\mathrm{eq}}`

### Mixed subscripts
Keep labels upright and variable indices italic:
- `\mat{J}_{\mathrm{F},i}^{-1}`
- `\mat{T}_{\mathrm{base},i}^{\mathrm T}`

## Operators and differentials

Preferred forms:
- `\sin \theta`
- `\cos \theta`
- `\ln x`
- `\exp(x)`
- `\det(\mat{J})`
- `\mathrm{tr}(\mat{R})`
- `\frac{\mathrm{d}q}{\mathrm{d}t}`
- `\int_0^T f(t)\,\mathrm{d}t`

## Units

Always write a space between the number and the unit, and keep units upright:
- `10\ \mathrm{mm}`
- `5\ \mathrm{s}`
- `20\ \mathrm{N}`
- `3\ \mathrm{rad}`

## Domain examples

```latex
\vect{r}_i^{\,0} + q_{i1}\uvect{e}_x + q_{i2}\uvect{e}_z
= \vect{p} + \mat{R}\vect{a}_i - q_{i6}\uvect{e}_{6,i}
```

```latex
\vect{\rho}_i := \vect{r}_i - \vect{p} = \mat{R}\vect{a}_i - q_{i6}\uvect{e}_{6,i}
```

```latex
\dot{\vect{q}}_i =
\begin{bmatrix}
\dot{q}_{i1} & \dot{q}_{i2} & \dot{q}_{i6}
\end{bmatrix}^{\mathrm T}
```

## Common mistakes to fix

- Same symbol used as both scalar and vector.
- Matrix written like an ordinary scalar letter.
- Descriptive subscripts left italic.
- Function names typed as variable strings.
- Units not upright or attached directly to numbers.
- Arrow vectors mixed with bold vectors.
