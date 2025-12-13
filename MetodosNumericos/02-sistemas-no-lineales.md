# Sistemas no lineales

---

## 1. Sistemas no lineales

Un sistema no lineal de orden $n$ en su forma algebraica está dado por

```math
\begin{cases}
f_1(x_1, x_2, \ldots, x_n) = 0 \\
f_2(x_1, x_2, \ldots, x_n) = 0 \\
\vdots \\
f_n(x_1, x_2, \ldots, x_n) = 0
\end{cases}
````

Un sistema no lineal de orden $n$ en su forma vectorial se escribe como

```math
\mathbf{F}(\mathbf{x}) = \mathbf{0},
```

donde

```math
\mathbf{F} : \mathbb{R}^n \to \mathbb{R}^n,
\qquad
\mathbf{x} =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix}
\in \mathbb{R}^n.
```

---

## 2. Método de Newton–Raphson

Dada una aproximación $\mathbf{x}^{(k)} = \mathbf{v} \in \mathbb{R}^n$, la siguiente aproximación
$\mathbf{x}^{(k+1)}$ satisface el sistema lineal

```math
\mathbf{F}(\mathbf{v})
+ J_{\mathbf{F}}(\mathbf{v})\,
\bigl(\mathbf{x}^{(k+1)} - \mathbf{v}\bigr)
= \mathbf{0}.
```

### Formulaciones prácticas

* **Usando la inversa del Jacobiano**

```math
\mathbf{x}^{(k+1)}
=
\mathbf{x}^{(k)}
-
\left[J_{\mathbf{F}}\bigl(\mathbf{x}^{(k)}\bigr)\right]^{-1}
\mathbf{F}\bigl(\mathbf{x}^{(k)}\bigr).
```

* **Sin usar la inversa del Jacobiano**

```math
\mathbf{x}^{(k+1)} = \mathbf{x}^{(k)} - \Delta \mathbf{x},
\qquad
J_{\mathbf{F}}\bigl(\mathbf{x}^{(k)}\bigr)\,\Delta \mathbf{x}
=
-\mathbf{F}\bigl(\mathbf{x}^{(k)}\bigr).
```

### Matriz Jacobiana

La matriz Jacobiana de $\mathbf{F}$ evaluada en $\mathbf{v}$ está dada por

```math
J_{\mathbf{F}}(\mathbf{v}) =
\begin{bmatrix}
\dfrac{\partial f_1}{\partial x_1}(\mathbf{v}) &
\dfrac{\partial f_1}{\partial x_2}(\mathbf{v}) &
\cdots &
\dfrac{\partial f_1}{\partial x_n}(\mathbf{v}) \\[2ex]
\dfrac{\partial f_2}{\partial x_1}(\mathbf{v}) &
\dfrac{\partial f_2}{\partial x_2}(\mathbf{v}) &
\cdots &
\dfrac{\partial f_2}{\partial x_n}(\mathbf{v}) \\[2ex]
\vdots & \vdots & \ddots & \vdots \\[2ex]
\dfrac{\partial f_n}{\partial x_1}(\mathbf{v}) &
\dfrac{\partial f_n}{\partial x_2}(\mathbf{v}) &
\cdots &
\dfrac{\partial f_n}{\partial x_n}(\mathbf{v})
\end{bmatrix}.
```

---

## 3. Método de punto fijo

El sistema no lineal

```math
\mathbf{F}(\mathbf{x}) = \mathbf{0}
```

se transforma en un problema de punto fijo

```math
\mathbf{x} = \mathbf{G}(\mathbf{x}).
```

### Esquema iterativo

La sucesión de aproximaciones se define mediante

```math
\mathbf{x}^{(k+1)} = \mathbf{G}\bigl(\mathbf{x}^{(k)}\bigr),
\qquad k = 0,1,2,\ldots
```

donde $\mathbf{x}^{(0)}$ es una aproximación inicial dada.

### Forma algebraica

Cada ecuación $f_i(x_1,\ldots,x_n)=0$ se despeja en la forma

```math
\begin{aligned}
x_1^{(k+1)} &= g_1\bigl(x_1^{(k)}, \ldots, x_n^{(k)}\bigr), \\
x_2^{(k+1)} &= g_2\bigl(x_1^{(k)}, \ldots, x_n^{(k)}\bigr), \\
&\;\vdots \\
x_n^{(k+1)} &= g_n\bigl(x_1^{(k)}, \ldots, x_n^{(k)}\bigr).
\end{aligned}
```

---

## 4. Convergencia

### Criterio empírico (error relativo)

Sea la sucesión
$\mathbf{x}^{(0)}, \mathbf{x}^{(1)}, \ldots, \mathbf{x}^{(k)}, \ldots$
y una tolerancia $\mathrm{Tol} > 0$.
El criterio empírico de convergencia es

```math
\delta^{(k)}
=
\frac{\|\mathbf{x}^{(k)} - \mathbf{x}^{(k-1)}\|_\infty}
{\|\mathbf{x}^{(k)}\|_\infty}
\le \mathrm{Tol}.
```

La norma infinito del vector
$\mathbf{x} = (x_1, x_2, \ldots, x_n)^\top \in \mathbb{R}^n$
está definida por

```math
\|\mathbf{x}\|_\infty = \max_{1 \le i \le n} |x_i|.
```

---

## 5. Convergencia del método de punto fijo

Sea el dominio

```math
D =
\left\{
(x_1, x_2, \ldots, x_n)
\;\middle|\;
a_i \le x_i \le b_i,\; i = 1,2,\ldots,n
\right\},
```

y una función continua
$\mathbf{G} : D \subseteq \mathbb{R}^n \to \mathbb{R}^n$.

Supongamos que se cumplen las condiciones:

* **Existencia:**
  $\mathbf{G}(\mathbf{x}) \in D$ para todo $\mathbf{x} \in D$.

* **Contracción:**
  $\mathbf{G}$ tiene derivadas parciales continuas y existe una constante
  $K < 1$ tal que

```math
\left|
\frac{\partial g_j(\mathbf{x})}{\partial x_i}
\right|
\le \frac{K}{n},
\qquad \mathbf{x} \in D,
```

para todo $i,j = 1,2,\ldots,n$.

Entonces, la sucesión

```math
\mathbf{x}^{(k+1)} = \mathbf{G}\bigl(\mathbf{x}^{(k)}\bigr),
\qquad k = 0,1,2,\ldots
```

con cualquier aproximación inicial $\mathbf{x}^{(0)} \in D$,
converge al **único punto fijo**
$\mathbf{p} \in D$.
