# 3. Simplicial Type Theory

These formalisations correspond in part to Section 3 of the RS17 paper.

This is a literate `rzk` file:

```rzk
#lang rzk-1
```

## Simplices and their subshapes

### Simplices

```rzk title="The 1-simplex"
#def Δ¹ : 2 → TOPE
  := \ t → TOP
```

```rzk title="The 2-simplex"
#def Δ² : (2 × 2) → TOPE
  := \ (t , s) → s ≤ t
```

```rzk title="The 3-simplex"
#def Δ³ : (2 × 2 × 2) → TOPE
  := \ ((t1 , t2) , t3) → t3 ≤ t2 ∧ t2 ≤ t1
```

### Boundaries of simplices

```rzk title="The boundary of a 1-simplex"
#def ∂Δ¹ : Δ¹ → TOPE
  := \ t → (t ≡ 0₂ ∨ t ≡ 1₂)
```

```rzk title="The boundary of a 2-simplex"
#def ∂Δ² : Δ² → TOPE
  :=
    \ (t , s) → (s ≡ 0₂ ∨ t ≡ 1₂ ∨ s ≡ t)
```

### The inner horn

```rzk
#def Λ : (2 × 2) → TOPE
  := \ (t , s) → (s ≡ 0₂ ∨ t ≡ 1₂)
```

### Products

The product of topes defines the product of shapes.

```rzk
#def shape-prod
  ( I J : CUBE)
  ( ψ : I → TOPE)
  ( χ : J → TOPE)
  : (I × J) → TOPE
  := \ (t , s) → ψ t ∧ χ s
```

```rzk title="The square as a product"
#def Δ¹×Δ¹ : (2 × 2) → TOPE
  := shape-prod 2 2 Δ¹ Δ¹
```

```rzk title="The total boundary of the square"
#def ∂□ : (2 × 2) → TOPE
  := \ (t ,s) → ((∂Δ¹ t) ∧ (Δ¹ s)) ∨ ((Δ¹ t) ∧ (∂Δ¹ s))
```

```rzk title="The vertical boundary of the square"
#def ∂Δ¹×Δ¹ : (2 × 2) → TOPE
  := shape-prod 2 2 ∂Δ¹ Δ¹
```

```rzk title="The horizontal boundary of the square"
#def Δ¹×∂Δ¹ : (2 × 2) → TOPE
  := shape-prod 2 2 Δ¹ ∂Δ¹
```

```rzk title="The prism from a 2-simplex in an arrow type"
#def Δ²×Δ¹ : (2 × 2 × 2) → TOPE
  := shape-prod (2 × 2) 2 Δ² Δ¹
```

### Intersections

The intersection of shapes is defined by conjunction on topes.

```rzk
#def shape-intersection
  ( I : CUBE)
  ( ψ χ : I → TOPE)
  : I → TOPE
  := \ t → ψ t ∧ χ t
```

### Unions

The union of shapes is defined by disjunction on topes.

```rzk
#def shapeUnion
  ( I : CUBE)
  ( ψ χ : I → TOPE)
  : I → TOPE
  := \ t → ψ t ∨ χ t
```

### Connection Squares

<!-- This is manually adjusted diagram (hopefully fully supported in the future by rzk) -->
<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <path d="M -99.0358460500274 -31.444756515260025 L -89.86020976982607 109.04996702489628 L 78.85622687537837 109.04996702489628 Z" style="fill: purple; opacity: 0.2"><title>f</title></path>
  <text x="-36.6799429814917" y="62.21839251151084" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <path d="M -88.57160646618001 -39.70915201762215 L 98.4961027394271 -39.70915201762215 L 89.32046645922577 100.78557152253414 Z" style="fill: purple; opacity: 0.2"><title>f</title></path>
  <text x="33.08165424415762" y="7.122422495763279" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-108.73641627786154,-28.016064827507464 -100.54837539908667,97.35687983478158" stroke="purple" stroke-width="3" marker-end="url(#arrow)"><title>f</title></polyline>
  <text x="-104.6423958384741" y="34.67040750363706" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="-90.03982894447489,-47.97354751998429 90.03982894447466,-47.97354751998429" stroke="purple" stroke-width="3" marker-end="url(#arrow)"><title>f</title></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="-94.34447446980596,-35.57774791227483 83.54960825780415,104.91856291954896" stroke="purple" stroke-width="3" marker-end="url(#arrow)"></polyline>
  <text x="-5.3974331060009035" y="34.670407503637065" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="-79.24496273247331,114.31436252725841 79.24496273247308,114.31436252725841" stroke="purple" stroke-width="3"></polyline>
  <polyline points="-79.24496273247331,120.31436252725841 79.24496273247308,120.31436252725841" stroke="purple" stroke-width="3"></polyline>
  <text x="-1.1368683772161603e-13" y="117.31436252725841" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="105.73641627786131,-28.016064827507464  97.54837539908644,97.35687983478158" stroke="purple" stroke-width="3"></polyline>
  <polyline points="111.73641627786131,-28.016064827507464 103.54837539908644,97.35687983478158" stroke="purple" stroke-width="3"></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
  <text x="-99.24496273247331" y="117.31436252725841" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
</svg>

```rzk title="RS17 Proposition 3.5(a)"
#define join-square-arrow
  (A : U)
  (f : 2 → A)
  : (2 × 2) → A
  := \ (t, s) → recOR ( t ≤ s ↦ f s , s ≤ t ↦ f t )
```

<!-- This is manually adjusted diagram (hopefully fully supported in the future by rzk) -->
<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <path d="M -99.0358460500274 -31.444756515260025 L -89.86020976982607 109.04996702489628 L 78.85622687537837 109.04996702489628 Z" style="fill: purple; opacity: 0.2"><title>f</title></path>
  <text x="-36.6799429814917" y="62.21839251151084" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <path d="M -88.57160646618001 -39.70915201762215 L 98.4961027394271 -39.70915201762215 L 89.32046645922577 100.78557152253414 Z" style="fill: purple; opacity: 0.2"><title>f</title></path>
  <text x="33.08165424415762" y="7.122422495763279" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-105.73641627786154,-28.016064827507464 -97.54837539908667,97.35687983478158" stroke="purple" stroke-width="3"></polyline>
  <polyline points="-111.73641627786154,-28.016064827507464 -103.54837539908667,97.35687983478158" stroke="purple" stroke-width="3"></polyline>
  <text x="-104.6423958384741" y="34.67040750363706" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-90.03982894447489,-44.97354751998429 90.03982894447466,-44.97354751998429" stroke="purple" stroke-width="3"></polyline>
  <polyline points="-90.03982894447489,-50.97354751998429 90.03982894447466,-50.97354751998429" stroke="purple" stroke-width="3"></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-94.34447446980596,-35.57774791227483 83.54960825780415,104.91856291954896" stroke="purple" stroke-width="3" marker-end="url(#arrow)"></polyline>
  <text x="-5.3974331060009035" y="34.670407503637065" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="-79.24496273247331,117.31436252725841 79.24496273247308,117.31436252725841" stroke="purple" stroke-width="3" marker-end="url(#arrow)"></polyline>
  <text x="-1.1368683772161603e-13" y="117.31436252725841" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="108.73641627786131,-28.016064827507464 100.54837539908644,97.35687983478158" stroke="purple" stroke-width="3" marker-end="url(#arrow)"></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
  <text x="-99.24496273247331" y="117.31436252725841" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">•</text>
</svg>

```rzk title="RS17 Proposition 3.5(b)"
#define meet-square-arrow
  (A : U)
  (f : 2 → A)
  : (2 × 2) → A
  := \ (t, s) → recOR ( t ≤ s ↦ f t , s ≤ t ↦ f s )
```

<!-- Definitions for the SVG images above -->
<svg width="0" height="0">
  <defs>
    <style data-bx-fonts="Noto Serif">@import url(https://fonts.googleapis.com/css2?family=Noto+Serif&display=swap);</style>
    <marker id="arrow" viewBox="0 0 10 10" refX="5" refY="5"
      markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M 0 2 L 5 5 L 0 8 z" stroke="black" fill="black" />
    </marker>
  </defs>
  <style>
    text, textPath {
      font-family: Noto Serif;
      font-size: 28px;
      dominant-baseline: middle;
      text-anchor: middle;
    }
  </style>
</svg>
