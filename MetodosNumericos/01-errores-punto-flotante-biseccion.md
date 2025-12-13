# Teoría de errores · Punto flotante (IEEE-754) · Bisección

---

## 1. Errores

**Definiciones básicas.** Sea $a$ la aproximación del valor (exacto) $A$.

- **Error absoluto:** $\xi = |A-a|$.
- **Error relativo:** $\delta_r=\dfrac{|A-a|}{|A|}$, $A\neq 0$.
- **Cifras significativas:** menor entero $n\ge 0$ tal que

```math
5\times 10^{-(n-1)} < \delta_r \le 5\times 10^{-n}
\quad \Longleftrightarrow \quad
n=\left\lfloor \dfrac{\ln(5/\delta_r)}{\ln 10} \right\rfloor.
````

### Propagación de errores

Sean las magnitudes con sus incertidumbres $x=\tilde{x}\pm \Delta x$ y $x_i=\tilde{x}_i\pm \Delta x_i$.

La incertidumbre (absoluta) de $q=f(x_1,x_2,\dots,x_n)$ se estima con:

```math
\Delta f \approx
\left|\frac{\partial f}{\partial x_1}\right|\Delta x_1+
\left|\frac{\partial f}{\partial x_2}\right|\Delta x_2+\cdots+
\left|\frac{\partial f}{\partial x_n}\right|\Delta x_n.
```

**Reglas típicas:**

* Si $q=f(x)$:

```math
\xi_q=\Delta f \approx \left|f'(\tilde{x})\right|\Delta x.
```

* Si $q=x\pm y$:

```math
\xi_q\approx \xi_x+\xi_y.
```

* Si $q=xy$ o $q=\dfrac{x}{y}$:

```math
\frac{\Delta q}{|q|}\approx \frac{\Delta x}{|x|}+\frac{\Delta y}{|y|}.
```

---

## 2. Punto flotante (IEEE-754)

Todo número real $x\neq 0$ se aproxima como:

```math
x = (-1)^{b_s}\,2^{E_{\mathrm{ext}}}\,(1+f),
```

donde:

* $b_s$ es el **signo** (0 para positivo y 1 para negativo),
* $f$ es la **parte fraccionaria** (mantisa sin 1 implícito),
* $E_{\mathrm{ext}}$ es el **exponente externo**.

Así, un sistema punto flotante $F(2,p,L,U)$ cumple:

```math
f=0.m_1 m_2 \cdots m_p,\qquad L\le E_{\mathrm{ext}}\le U.
```

### Estándar IEEE-754

Se almacena la representación punto flotante como una cadena con el signo $b_s$, el exponente interno/sesgado
$E_{\mathrm{int}}=E_{\mathrm{ext}}+\mathrm{bias}$ y la mantisa $f$ en base 2:

```math
\boxed{
\begin{array}{c|c|c}
\text{signo }(1) & \text{exponente interno }(k) & \text{mantisa }(p)\\ \hline
s & e_{k-1}\ldots e_0 & m_1 m_2 \ldots m_p
\end{array}}
```

### Precisiones comunes

```math
\begin{array}{c|c|c|c}
\text{Formato} & \text{signo} & \text{exponente interno} & \text{mantisa}\\ \hline
\text{Simple (32b)} & 1 & 8\ (\mathrm{bias}=127) & 23\\
\text{Doble (64b)} & 1 & 11\ (\mathrm{bias}=1023) & 52
\end{array}
```

### Casos especiales

```math
\begin{array}{cl}
E_{\mathrm{int}}=0 &\Rightarrow \text{subnormal (sin 1 implícito)}\\
E_{\mathrm{int}}=2^k-1,\ m=0 &\Rightarrow \pm\infty\\
E_{\mathrm{int}}=2^k-1,\ m\neq 0 &\Rightarrow \text{NaN}
\end{array}
```

---

## 3. Método de bisección

Una raíz de una función $f$ es un número $x^\star \in\mathbb{R}$ tal que $f(x^\star)=0$.

### Algoritmo de bisección

Dado $k=0$. Iniciar con un intervalo $[a_k,b_k]=[a,b]$ tal que $f(a)f(b)<0$, definir:

```math
c_k=\frac{a_k+b_k}{2}.
```

Actualizar:

```math
\begin{cases}
a_{k+1}=a_k,\ \ b_{k+1}=c_k & \text{si } f(a_k)f(c_k)<0,\\[4pt]
a_{k+1}=c_k,\ \ b_{k+1}=b_k & \text{si } f(c_k)f(b_k)<0,\\[4pt]
k=k+1 &
\end{cases}
```

### Existencia y convergencia

Si $f$ es continua en $[a,b]$ y $f(a)f(b)<0$, entonces:

* Existe $a<c<b$ tal que $f(c)=0$.
* La sucesión ${c_k}$ converge a una raíz de $f$ en $[a,b]$.

### Error absoluto y criterio de parada

**Error absoluto** después de $k$ iteraciones:

```math
|x^\star-c_k|\le \frac{b-a}{2^{k+1}}.
```

**Criterio de parada:** número mínimo de iteraciones para tolerancia $\tau$:

```math
\frac{b-a}{2^{k+1}}\le \tau
\quad \Longleftrightarrow \quad
n\ge \frac{\ln\!\left(\dfrac{b-a}{2\tau}\right)}{\ln 2}.
```

---

## 4. Aproximación de Taylor

El polinomio de Taylor aproxima una función $f(x)$ cerca de un punto $a$ usando sus derivadas.

**Teorema.** Si $f$ tiene $n+1$ derivadas continuas en un intervalo que contiene $a$, entonces:

```math
f(x)=f(a)+f'(a)(x-a)+\frac{f''(a)}{2!}(x-a)^2+\cdots+\frac{f^{(n)}(a)}{n!}(x-a)^n+R_n(x),
```

donde el **residuo de Lagrange** es:

```math
R_n(x)=\frac{f^{(n+1)}(\zeta)}{(n+1)!}(x-a)^{n+1},
```

y $\zeta$ está entre $a$ y $x$.

---

## 5. Notación $O(\cdot)$

```math
g(x)=O(h(x))\quad \text{si existe } M>0 \text{ tal que } |g(x)|\le M|h(x)|.
```

**Interpretación.** Un error es $O(h^p)$ si decrece proporcionalmente a $h^p$ cuando $h\to 0$.
