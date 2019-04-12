Here in is my setup for an Org-based poster that uses
[baposter](http://www.brian-amberg.de/uni/poster/), an amazing and intuitive poster creation system.
( A future side project will be to provide a full Org interface to baposter. )

Below are the contents of the poster.

----

How do you write an algorithm for the dot product?

This way? &#x2014;With every piece mentioned in detail.

\begin{flalign*}
   dot_{six} & \;:\; \mathbb{R} \times \mathbb{R} \times \mathbb{R} \times \mathbb{R} \times \mathbb{R} \times \mathbb{R}  \to \mathbb{R}               
\\ dot_{six} & \; x_1 \; x_2 \; x_3 \; y_1 \; y_2 \; y_3  \;\;=\;\;  x_1 * y_1 + x_2 * y_2 + x_3 * y_3
\end{flalign*}

Or with the pieces bundled up into “logically related units”?

\begin{flalign*}
   dot_{vec} & \;:\; \mathbb{R}^3 \times \mathbb{R}^3  \to \mathbb{R}               
\\ dot_{vec} & \; \vec{x} \; \vec{y} \;\;=\;\;  \vec{x}_1 * \vec{y}_1 + \vec{x}_2 * \vec{y}_2 + \vec{x}_3 * \vec{y}_3
\end{flalign*}

What if we want to ignore the second dimension?

\begin{flalign*}
   dot_{noTwo} & \;:\; \mathbb{R}^3 \times \mathbb{R}^3 \to \mathbb{R}               
\\ dot_{noTwo} & \; \vec{x} \; \vec{y} \;\;=\;\;  \vec{x}_1 * \vec{y}_1 + \vec{x}_3 * \vec{y}_3
\end{flalign*}

What if we only want to product along a given vector \(\vec{y}\)?

\begin{flalign*}
   dot_{fixed} & \;:\; \mathbb{R}^3 \to \mathbb{R}               
\\ dot_{fixed} & \; \vec{x} \;\;=\;\;  \vec{x}_1 * \vec{y}_1 + \vec{x}_2 * \vec{y}_2 + \vec{x}_3 * \vec{y}_3
\end{flalign*}

All the ‘forms’ are useful, slightly distinct, but **behave essentially the same!**

<div class="org-center">
*Let's write one form, and generate the rest!*
</div>

 \begin{posterbox}[name=dotcode,column=0, span = 1, below=whypackage]{Write once, Generate many}
 \begin{lstlisting}
// Write once,
Vector = Int[3]
Int dot_vec (Vector x, Vector y)
  =   x[0] * y[0]  +  x[1] * y[1]  +  x[2] * y[2]

// Generate many,
dot_six    =  dot_vec #flattened#
dot_noTwo  =  dot_vec #with# x[1] := 0, y[1] := 0
dot_fixed  =  dot_vec #with# y := [2, 3, 5]
 \end{lstlisting}
 \end{posterbox}

<div class="org-center">


We begin by manually writing a “package former” &#x2013;a package whose concrete form is unspecified,
akin to type constructors. &#x2013;Look at the demo.

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">**Novel Idea:** *Higher-order packages*</td>
</tr>
</tbody>
</table>

With such a generic mechanism, we could derive nearly all other forms of packaging!

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">&check; Classes</td>
<td class="org-left">&check; Records</td>
<td class="org-left">&check; Interfaces</td>
<td class="org-left">&check; Typeclasses</td>
<td class="org-left">&check; Traits</td>
<td class="org-left">&check; Dependent Products</td>
<td class="org-left">&check; Dependent Sums</td>
</tr>
</tbody>
</table>

Write a method on the package former *once* then have it apply to all deviations!

Moreover, it supports the expected features of modularisation.

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">&check; Namespacing</td>
<td class="org-left">&check; Information Hiding</td>
<td class="org-left">&check; Polymorphism</td>
<td class="org-left">&check; First- and Second-class Citizenship</td>
</tr>


<tr>
<td class="org-left">&check; Applicative modules</td>
<td class="org-left">&check; Generative modules</td>
<td class="org-left">&check; Subtyping</td>
<td class="org-left">&check; Computation Sharing</td>
</tr>
</tbody>
</table>
</div>

<div class="org-center">
There are many ways to bundle up data;
rather than write each form by hand,
we should “write one, generate many”!
</div>

The dot product example is small and could have been done *manually, by hand*;
so why not?

-   Tedious & error-prone,
-   Obscures the corresponding general principles underlying them,
-   No machine support to ensure implementation is correct,
-   Does not scale!

Without jeopardising runtime performance \cite{dtl_practical_erasure}, we want

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">*Machine support deriving mechanisms for different ‘views’ of packages.*</td>
</tr>
</tbody>
</table>

Which should be well-defined, with coherent semantic underpinning
&#x2014;such as an associated Cartesian Closed Category \cite{modules_categorically}.

Packaging forms are essentially problems of variable introduction,

\begin{align*}
         &\;\; \forall x_1, x_2, x_3, y_1, y_2, y_3  \;\;\bullet\;\; \cdots
\\ \cong \;\;&\;\; \forall \vec{x}, \vec{y}  \hspace{6.6em}\;\;\bullet\;\; \cdots
\\ \cong \;\;&\;\; \forall \vec{y} \;\;\bullet\;\; \forall \vec{x} \hspace{4.5em} \;\;\bullet\;\; \cdots
\end{align*}

Possibly with constraints,

\begin{align*}
\forall \vec{x}, \vec{y}  \;\;\with\;\; \vec{x}_2 = 0 \;\land\; \vec{y}_2 = 0 \;\;\bullet\;\; \cdots
\end{align*}

When we have a package, we can do a number of things with it \cite{tpc}:

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Flatten</td>
<td class="org-left">‘unpack’ some of its nested packages</td>
</tr>


<tr>
<td class="org-left">Instantiate</td>
<td class="org-left">‘pick values’ for some of its contents</td>
</tr>


<tr>
<td class="org-left">Parameterise</td>
<td class="org-left">‘fix’ some of its contents</td>
</tr>


<tr>
<td class="org-left">Hide</td>
<td class="org-left">‘ignore’ some of its contents</td>
</tr>


<tr>
<td class="org-left">Rename</td>
<td class="org-left">‘relabel’ some of its contents</td>
</tr>


<tr>
<td class="org-left">Mixin</td>
<td class="org-left">‘combine’ packages over a common interface</td>
</tr>


<tr>
<td class="org-left">Package-up</td>
<td class="org-left">‘bundle’ some of its contents</td>
</tr>
</tbody>
</table>

-   Emacs prototype actually producing derivatives from “package formers” \newline &#x2014;see the demo.
-   Packages, top level modules, can be treated like any other *value*.
    -
-   Prototype interface is practical and accessible.
-   Prototype appears promising to eliminate massive renaming boilerplate in
    \cite{RATH, agda_lib} developments.

-   Realise proposal in an existing industrial-strength compiler.
    -   Hence, implementations are more than just ‘research quality’ \cite{theories_as_types}
        but actually ready for a broad audience.
    
    -   Users should be able to extend it within the core language &#x2013;without resorting to inspecting
        the compiler.

-   Utilise the opportunities provided by dependent-types \cite{curry_howard, why_dependent_types_matter} to provide
    a module system suitable for dependently-typed languages
    whose constructs are orthogonal. ---*Dependent-types blur many lines!*

-   Provide denotational semantics \cite{dtl_cat_models} for the resulting module system.
    -   Ensure all proofs are machine-checked, using Agda \cite{lof_constructive_math}.



\iffalse

\parencite{RATH, agda_trains, agda_web}


<a id="org5684680"></a>

# Theory Presentation Combinators     :tpc:


<a id="orgd31703e"></a>

# A Brief Overview of Agda &#x2014; A Functional Language with Dependent Types     :agda_overview:


<a id="org889040a"></a>

# Working with Mathematical Structures in Type Theory     :math_structs_in_types:


<a id="org5909ff3"></a>

# Agda Library     :agda_lib:


<a id="org68a012f"></a>

# Constructive Mathematics and Computer Programming     :lof_constructive_math:


<a id="org7e6dbc2"></a>

# Type classes for mathematics in type theory     :typeclasses_for_maths:


<a id="orgec926d3"></a>

# Theories as Types     :theories_as_types:


<a id="org54e90ee"></a>

# A Cateogry-Theoretic Account of Program Modules     :modules_categorically:


<a id="orgbda506b"></a>

# Relation-Algebraic Theories in Agda     :RATH:


<a id="orgea0e422"></a>

# Dependent Types At Work     :curry_howard:


<a id="org0018de7"></a>

# Why dependent types matter     :why_dependent_types_matter:


<a id="org8e90a21"></a>

# Practical Erasure in Dependently Typed Languages     :dtl_practical_erasure:


<a id="org8207a03"></a>

# Categorical Models of Dependent Type Theory     :dtl_cat_models:


<a id="org4ac2fe1"></a>

# \fi

