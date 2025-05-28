---
theme: neversink
layout: cover
title: Error-Correction of Matrix Multiplication Algorithms
info: |
  ## Error-Correction of Matrix Multiplication Algorithms
  STOC2025

author: Nobutaka Shimizu
mdc: true
css: unocss
style: |
  @import './styles/custom.css';

fonts:
  sans: 'Roboto'
  mono: 'Fira Code'
  weights: '400,500,700'
  italic: true
favicon: 'https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6/svgs/solid/book.svg'
themeConfig:
  primary: '#1976d2'


---

# 行列積アルゴリズムに対する<br>誤り訂正

[Nobutaka Shimizu](https://sites.google.com/view/nobutaka-shimizu/home) (Institute of Science Tokyo)

<div style="position: absolute; bottom: 20px; font-size: 0.8em; width: 100%; text-align: center;">
June 5th, 2025
</div>



---
layout: top-title
color: amber-light
---

::title::

# 行列積

::content::

<div class="topic-box">

入力として与えられた二つの行列$A,B\in\mathbb{F}^{n\times n}$に対して$AB$を計算せよ ($\mathbb{F}$は有限体).

</div>

- $O(n^\omega)$-time algorithms for
  - $\omega = 2.373$ (Coppersmith-Winograd)
<!-- TODO: history of matrix multiplication-->
  
---
layout: top-title
color: amber-light
---

::title::

# 行列積の近似

::content::

<div class="topic-box">

入力として与えられた二つの行列$A,B\in\mathbb{F}^{n\times n}$に対して, 行列$C\in\mathbb{F}^{n\times n}$であって,
$AB$と$\alpha \cdot n^2$個の成分が一致するものを(何でもよいので)計算せよ.

</div>

- 有限体のサイズを$q=|\mathbb{F}|$とする.
- $\alpha = \frac{1}{q}$なら簡単 (ランダムな行列を出力すればよい)
- 「非自明なアルゴリズム」: $\alpha \ge \frac{1}{q} + \epsilon$ を達成

<div class="question">

$\widetilde{O}(n^2)$時間で非自明な$\alpha$を達成できるか? ($\widetilde{O}(\cdot)$は$\polylog(n)$倍を無視したオーダー)

</div>

---
layout: top-title
color: amber-light
---

::title::

# 動機

::content::

- 高速行列積の多くのアルゴリズムは非実用的
  - 定数倍が非常に大きい
- 物理系を利用した(実用性を意識した)行列積アルゴリズム
  - 水流
  - 熱力学系
  - 光学デバイス

<div class="topic-box">

  これらのアルゴリズムは物理系のホワイトノイズによるエラーが発生しうる (近似行列積を解く).

</div>

---
layout: top-title
color: amber-light
---

::title::

# Problem Setting (Formal)

::content::

<div class="definition">

二つの行列$C,D\in \mathbb{F}^{n\times n}$の**一致率** $\agr(C,D)$を以下で定義する:

$$
  \begin{align*}
    \agr(C,D) &:= \Pr_{i,j\sim[n]}[C(i,j) = D(i,j)]
  \end{align*}
$$

このとき, 行列$C$は$D$と**一致率$\alpha$を持つ**という.

</div>

- **近似行列積**: 与えられた$A,B\in\mathbb{F}^{n\times n}$に対して$\agr(C,AB)\ge \alpha$を満たす行列$C$を求めよ.
- アルゴリズム$M$は任意の$A,B\in\mathbb{F}^{n\times n}$に対して$\agr(M(A,B)),AB)\ge \alpha$を満たすとき, **一致率$\alpha$を持つ**という.
  - $\alpha$に寄与する成分は入力$A,B$毎に異なりうる
  - $\alpha=1$のとき, 全成分を正しく計算

---
layout: top-title
color: amber-light
---

::title::

# 主結果 (最悪時)

::content::

<div class="theorem">

任意の$\alpha\in(0,1]$を考える.
サイズが$p=\abs{\F}>10/\alpha$であるような有限体を考える.
このとき, 一致率$\alpha$を持つ$T(n)$時間アルゴリズムが存在するならば,
一致率$1$を持つ$\widetilde{O}(T(n)\poly(1/\alpha))$時間アルゴリズムが存在する.

</div>

- 非常に大きい体$\F$上では「1\%の成分さえ計算できれば全ての成分が計算できる」

<div class="theorem">

任意の$\epsilon\in(0,1]$を考える.
任意の要素数が素数であるような有限体$\F$に対し, 以下が成り立つ:
一致率$\alpha\ge \frac{1}{\abs{\F}}+\epsilon$を持つ$T(n)$時間アルゴリズムが存在するならば,
一致率$1$を持つ$\widetilde{O}_{p,\epsilon}(T(n))$時間アルゴリズムが存在する.

</div>

- 体のサイズ$p=\abs{\F}$が定数のときはタイトな一致率
- 隠された定数倍は非常に大きい: $2^{2^{\poly(p/\epsilon)}}$

---
layout: top-title
color: amber-light
---

::title::

# 主結果 (平均時から最悪時への帰着)

::content::

前ページの結果では, 近似アルゴリズムは**任意の**入力に対する一致率の保証が必要であったが,
ランダムな入力に対する平均的な一致率の保証からでも同様の結果が示せる.
アルゴリズム$M$の**平均一致率**を以下で定める:

$$
  \begin{align*}
    \Exp_{\substack{A,B\sim\Fp^{n\times n}}}[\agr(M(A,B),AB)]
  \end{align*}
$$


<div class="theorem">

任意に$\alpha\in(0,1]$を固定し, $p=\abs{\F}>10/\alpha$を満たす有限体$\F$を考える.
このとき, 平均一致率$\alpha$を持つ$T(n)$時間アルゴリズムが存在するならば,
任意の入力$A,B\in\F^{n\times n}$に対し


$$
  \begin{align*}
    \Pr_{M'}[M'(A,B)=AB] &\ge \frac{2}{3}
  \end{align*}
$$

を満たす$\widetilde{O}(T(n)\poly(1/\alpha))$時間アルゴリズム$M'$が存在する.

</div>

- **平均時から最悪時への帰着**: 平均的な入力でうまくいくアルゴリズムを用いて全ての入力でうまくいくアルゴリズムを設計
- 平均一致率が$\alpha = 1/\abs{\F}+\epsilon$の場合も同様の結果が成り立つ.

---
layout: top-title
color: amber-lighft
---

::title::

# 関連結果

::content::

- $\F=\F_2$かつ**片側エラー**の設定下における同様の結果 <a class="cite-reference" href="https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.APPROX/RANDOM.2024.34">\[Gola, Shinkar, Singh, RANDOM'24\]</a>
  - 片側エラー: $(AB)_{i,j}=1$ならば必ず$M(A,B)_{i,j}=1$だが, $(AB)_{i,j}=0$のときは$M(A,B)_{i,j}=1$になりうる.

- 行列積に対する最悪時から平均時への帰着
  - 平均的な入力上で全成分が計算できる$\Rightarrow$ 任意の入力上で全成分が計算できる
  - <a href="https://www.sciencedirect.com/science/article/pii/002200009390044W?via%3Dihub" class="cite-reference">\[Blum, Luby, Rubinfeld, JCSS'93\]</a>
  - <a href="https://dl.acm.org/doi/10.1145/3519935.3520041" class="cite-reference">\[Asadi, Golovnev, Gur, Shinkar, STOC'22\]</a>
  - <a href="https://dl.acm.org/doi/10.1145/3564246.3585189" class="cite-reference">\[Hirahara, Shimizu, STOC'23\]</a>



---
layout: top-title
color: amber-light
---

::title::

# Our Result (3/3): Nonuniform Reduction

::content::

<div class="theorem">

If there exists an algorithm $M$ that runs in time $T(n)$ and and satisfies

$$
  \begin{align*}
    \Exp_{\substack{A,B\sim\Fp^{n\times n} \\ M}}[\agr(M(A,B),AB)] &\ge \frac{1}{p}+\varepsilon,
  \end{align*}
$$

then, in time $O_{p,\varepsilon}(n^3)$, we can construct a size-$T(n)\cdot\poly(\log n,p,1/\varepsilon)$ circuit $M'$ that satisfies

$$
  \begin{align*}
    {}^{\forall}A,B\in\Fp^{n\times n}, \quad \Pr_{M'}[M'(A,B)=AB] &\ge \frac{2}{3}.
  \end{align*}
$$

</div>

- Reduction with poly-time preprocessing
- Optimal agreement at the cost of nonuniformity
- Proof is based on XOR Lemma (average-case complexity)

---
layout: top-title
color: amber-light
---

::title::

# Related work

::content::

- <a href="https://arxiv.org/abs/2305.13945" class="cite-reference">\[Gola, Shinkar, Singh, RANDOM'24\]</a> gave the same reduction for $\alpha > 7/8$
  - They also gave a reduction for $p=2$ and $\alpha=1/2+\varepsilon$ under the assumption that $M$ has **one-sided** error (it satisfies $M(A,B)_{i,j}=1$ whenever $(AB)_{i,j}=1$)
  - Our reduction: $M$ can have two-sided error but is need a preprocessing
- <a href="https://link.springer.com/article/10.1007/s00453-016-0202-3" class="cite-reference">\[Gąsieniec, Levcopoulos, Lingas, Pagh, Tokuyama, Algorithmica'17 \]</a> showed how to compute $AB$ given three matrices $A,B,C\in\Fp^{n\times n}$ such that $\agr(AB,C)\ge 1-1/n$.
  - $n$ entries can be wrong (our reduction allows, say, $0.99n^2$ errors)
  - Their setting is more restrictive than ours (we are allowed to query the product of random matrices for many times)

---
layout: top-title
color: amber-light
---

::title::

# Additional Remark

::content::

- We believe that our nonuniform reduction is **practical** if $\abs{\Fp}$ is small
  - running time overhead is $p\cdot \poly(1/\varepsilon) \cdot \log n$
  - simple and thus hidden constant factor is reasonably small
- Our uniform reductions are based on **efficiently encodable/list-decodable codes** with linear rate
  - specifically, we use Reed-Solomon codes and expander-based codes
  - If the code admit a practical list-decoding algorithm, then our reduction is also practical
- In our follow-up work (to appear in ICALP25), we obtained **uniform** reduction with **optimal** agreement $\alpha = 1/p + \varepsilon$
  
---
layout: section
color: amber-light
---

# Proof of Uniform Reduction

---
layout: top-title
color: amber-light
---

::title::

# Idea: Matrix Encoding

::content::

- Suppose we have an algorithm $M$ that computes an $\alpha$-agreement of $AB$ for **arbitrary** $A,B\in\Fp^{n\times n}$.
  - We would like to compute $AB$ for any $A,B\in\Fp^{n\times n}$
- Given two matrices $A,B \in \Fp^{n\times n}$, we **encode** them using $\Enc\colon \Fp^{n\times n} \to \Fp^{N\times N}$
- Compute $\widetilde{C} = M(\Enc(A),\Enc(B))$. Note that $\widetilde{C}$ an $\alpha$-agreement of $\Enc(A)\cdot \Enc(B)$.

- **Suppose that $\Enc(A)\cdot \Enc(B) \in \Fp^{N\times N}$ is an encoding of $AB$ of a list-decodable ECC**
  - i.e., we can write $\Enc(A)\cdot \Enc(B) = \Enc'(AB)$ for some nice $\Enc'\colon\Fp^{n\times n}\to\Fp^{N\times N}$
- Then, we can obtain a list containing $AB$ by list-decoding $M(\Enc(A),\Enc(B))$
  - We can identify $AB$ from the list by checking if $AB=C$ using Freivalds' randomized algorithm

<div class="question">

  Is there such $\Enc$ and $\Enc'$?

</div>

---
layout: top-title
color: amber-light
---

::title::

# Basic of Error-Correcting Codes

::content::

- An **encoding function** is a linear map $\Enc\colon \Fp^n \to \Fp^N$ for $N\ge n$
  - The image $C = \Enc(\Fp^k)$ is called a **code** and $x\in C$ is called a **codeword**
  - **distance** = $\min_{x,y\in C,x\ne y} \dist(x,y)$
    - $\dist(\cdot,\cdot)$ is the normalized Hamming distance
- For $x\in \Fp^N$ and $\rho\in[0,1]$, let $\ball(x,\rho)=\{ y\in\Fp^N \colon \dist(x,y)\le\rho \}$

<div class="topic-box">

  **list-decoding.**
  Given $x\in\Fp^N$, compute all vectors in $C\cap \ball(x,\rho)$.

</div>

- $\#$ of codewords in $\ball(x,\rho)$?
- fast algorithm?