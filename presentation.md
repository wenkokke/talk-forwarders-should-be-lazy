### Wen Kokke
# [fit] Forwarders<br />Should Be Lazy
## [fit] A Talk About A Tiny Detail Of Classical Processes

---

# Introducing Classical Processes

What if Classical Linear Logic<br />was the type system for a process calculus?
<br />
$$
\newcommand{\llpar}{\mathbin{\smash{⅋}}}
\begin{array}{lrlrll}
A, B, C, D
& :=
& A \otimes B
    \vphantom{\overline{A \otimes B}}
& \mid
& A \llpar B
    \vphantom{\overline{A} \llpar \overline{B}}
& \small\text{delegation}
\\
& \mid
& A \oplus B
    \vphantom{\overline{A \oplus B}}
& \mid
& A \mathbin{\&} B
    \vphantom{\overline{A} \mathbin{\&} \overline{B}}
& \small\text{choice}
\\
& \mid
& \forall X.A
    \vphantom{\overline{\forall X.A}}
& \mid
& \exists X.A
    \vphantom{\exists X.\overline{A}}
& \small\text{polymorphism}
\\
& \mid
& \ldots
&
&
& \small\text{etcetera}
\end{array}
$$

---

# Introducing Classical Processes

What if Classical Linear Logic<br />was the type system for a process calculus?
<br />
$$
\begin{array}{lrlrll}
\hphantom{A, B, C, D}
& \hphantom{:=}
& \overline{A \otimes B}
& \rlap{=}{\hphantom{\mid}}
& \overline{A} \llpar \overline{B}
& \small\text{delegation}
\\
& \hphantom{\mid}
& \overline{A \oplus B}
& \rlap{=}{\hphantom{\mid}}
& \overline{A} \mathbin{\&} \overline{B}
& \small\text{choice}
\\
& \hphantom{\mid}
& \overline{\forall X.A}
& \rlap{=}{\hphantom{\mid}}
& \exists X.\overline{A}
& \small\text{polymorphism}
\\
& \hphantom{\mid}
& \ldots
&
&
& \small\text{etcetera}
\end{array}
$$

---

# Introducing Classical Processes

What if Classical Linear Logic<br />was the type system for a process calculus?
<br />
$$
\begin{array}{lrlrll}
\hphantom{A, B, C, D}
& \hphantom{:=}
& \overline{A} \otimes \overline{B}
& \rlap{=}{\hphantom{\mid}}
& \overline{A \llpar B}
& \small\text{delegation}
\\
& \hphantom{\mid}
& \overline{A} \oplus \overline{B}
& \rlap{=}{\hphantom{\mid}}
& \overline{A \mathbin{\&} B}
& \small\text{choice}
\\
& \hphantom{\mid}
& \forall X.\overline{A}
& \rlap{=}{\hphantom{\mid}}
& \overline{\exists X.A}
& \small\text{polymorphism}
\\
& \hphantom{\mid}
& \ldots
&
&
& \small\text{etcetera}
\end{array}
$$

---

# Introducing Classical Processes

$$
\newcommand{\lnk}{\mathord{\leftrightarrow}}
\newcommand{\sel}{\mathbin{\triangleleft}}
\newcommand{\ofr}{\mathbin{\triangleright}}
\newcommand{\inl}{\mathord{\text{inl}}}
\newcommand{\inr}{\mathord{\text{inr}}}
\begin{array}{lrlrllcl}
P, Q, R
& :=
& x \lnk y
&
&
& \small\text{forwarder}
\\
& \mid
& (\nu x \bar{x})P
&
&
& \small\text{new channel creation}
\\
& \mid
& P \parallel Q
&
&
& \small\text{parallel composition}
\\
& \mid
& x[y].P
& \mid
& x(y).P
& \small\text{send/receive delegation}
\\
& \mid
& x\sel\ell.P
& \mid
& x\ofr\{\ell:P_\ell\}_{\ell\in L}
& \small\text{send/receive choice}
\\
& \mid
& x[A].P
& \mid
& x(A).P
& \small\text{send/receive type}
\\
& \mid
& \ldots
&
&
& \small\text{etcetera}
\end{array}
$$

[.footer: (Disclaimer: This is technically Hypersequent Classical Processes. Potato, Tomato.)]

---

# Introducing Classical Processes

$$
% Axiom
\begin{array}{c}
\\\hline
x{\leftrightarrow}y
\vdash
x:A, y:\overline{A}
\end{array}
\,\small\text{(Axiom)}
$$

[.column]

$$
% Parallel Composition
\begin{array}{c}
P
\vdash
\Gamma
\qquad
Q
\vdash
\Delta
\vphantom{\overline{A}}
\\\hline
P \parallel Q
\vdash
\Gamma \parallel \Delta
\end{array}
\,\small\text{(Branch)}
$$

$$
\begin{array}{c}
P
\vdash
\Gamma, y:A
\parallel
\Delta, x:B
\\\hline
x[y].P
\vdash
\Gamma, \Delta, x:A \otimes B
\end{array}
\,\small(\otimes)
$$

[.column]

$$
% New Channel Creation
\begin{array}{c}
P
\vdash
\Gamma, x:A
\parallel
\Delta, \bar{x}:\overline{A}
\\\hline
    (\nu x \bar{x})P
    \vdash
    \Gamma, \Delta
\end{array}
\,\small\text{(Cut)}
$$

$$
% Par
\begin{array}{c}
P
\vdash
\Gamma, y:A, x:B
\\\hline
x(y).P
\vdash
\Gamma, x:A \llpar B
\end{array}
\,\small(\llpar)
$$

[.footer: (Disclaimer: This is technically Hypersequent Classical Processes. Potato, Tomato.)]

---

# Introducing Classical Processes

$$
\begin{array}{ccc}
\small\text{(Forward)}
&\qquad&
\small\text{(Delegate)}
\\
(\nu x \bar{x})(\link{x}{w} \parallel P)
&\qquad&
(\nu x \bar{x})(x[y].P \parallel \bar{x}(\bar{y}).Q)
\\
\downarrow&\qquad&\downarrow
\\
P\{w/\bar{x}\}
&\qquad&
(\nu x \bar{x})(\nu y \bar{y})(P \parallel Q)
\\
\hphantom{(\nu x \bar{x})(x\sel\inl.P \parallel \bar{x}\ofr\{\inl:Q;\inr:R\})}&\qquad&\hphantom{(\nu x \bar{x})(x\sel\inl.P \parallel \bar{x}\ofr\{\inl:Q;\inr:R\})}%just strutting
\\
\small\text{(Choose)}
&\qquad&
\small\text{(Instantiate)}
\\
(\nu x \bar{x})(x\sel\inl.P \parallel \bar{x}\ofr\{\inl:Q;\inr:R\})
&\qquad&
(\nu x \bar{x})(x[A].P \parallel \bar{x}(X).Q)
\\
\downarrow&\qquad&\downarrow
\\
(\nu x \bar{x})(P \parallel Q)
&\qquad&
(\nu x \bar{x})(\nu y \bar{y})(P \parallel Q)
\end{array}
$$

[.footer: (Disclaimer: This is technically Hypersequent Classical Processes. However, the reduction rules are the same, so you cannot really tell.)]

---

# Introducing Classical Processes

$$
\begin{array}{ccc}
\small\text{(Forward)}
&\qquad&
\small\text{(Delegate)}
\\
(\nu x \bar{x})(\link{x}{w} \parallel P)
&\qquad&
(\nu x \bar{x})(x[y].P \parallel \bar{x}(\bar{y}).Q)
\\
\downarrow&\qquad&\downarrow
\\
P\{w/\bar{x}\}
&\qquad&
(\nu x \bar{x})(\nu y \bar{y})(P \parallel Q)
\\
\hphantom{(\nu x \bar{x})(x\sel\inl.P \parallel \bar{x}\ofr\{\inl:Q;\inr:R\})}&\qquad&\hphantom{(\nu x \bar{x})(x\sel\inl.P \parallel \bar{x}\ofr\{\inl:Q;\inr:R\})}%just strutting
\\
\\
\text{This is }\textbf{asynchronous}\text{.}
&\qquad&
\text{This is }\textbf{synchronous}\text{.}
\\
\\
&\qquad&
\text{Everything else is.}
\end{array}
$$

[.footer: (Disclaimer: This is technically Hypersequent Classical Processes. However, the reduction rules are the same, so you cannot really tell.)]

---

## Oh No, Is That Bad?

Not really, but...

<br />

It **complicates the metatheory** a bunch.

It **invalidates the simplest process interpretation**.

It does a third thing so this list has three items?

[.footer: (Disclaimer: I have not yet determined the third thing.)]

---

# [fit] It complicates the metatheory a bunch

It leads to a lot of special cases for forwarders...

<br />

A process is in **canonical form** when it does not contain (1) dual ready actions on the same channel or (2) **any ready forwarder**.

A process is in **canonical form** when all ready actions are blocked on external channels **in the absence of ready forwarders**.

---

# [fit] It invalidates the simplest process interpretation

What does this reduction rule require of an implementation?

$$
\begin{array}{c}
\small\text{(Forward)}
\\
(\nu x \bar{x})(\link{x}{w} \parallel P)
\\
\downarrow
\\
P\{w/\bar{x}\}
\\
\hphantom{(\nu x \bar{x})(x\sel\inl.P \parallel \bar{x}\ofr\{\inl:Q;\inr:R\})}
\end{array}
$$
The process $$P$$ isn't required to be listening on $$\bar{x}$$.
This cannot be implemented as message-passing communication.

[.footer: (Disclaimer: There really wasn't a third thing. I am sorry for deceiving you.)]

---

# What Can We Do?

---

## What Do? ① Make It Synchronous

[.column]

$$
\begin{array}{ccc}
\small\text{(Forward)}
\\
(\nu x \bar{x})(\link{x}{w} \parallel P)
\\
\downarrow
\\
P\{w/\bar{x}\}
\\
\text{but...}
\\
\text{only if }\mathbf{ready}(P,\bar{x})
\end{array}
$$

[.column]

<br />

Simplifies the metatheory!

Simplifies the implementation...

**A little bit...**

---

## What Do? ② Identity Expansion

[.column]

Let's use **Identity Expansion**!
<br />
Identity Expansion is the dual of Cut Elimination.

It rewrites uses of the axiom to uses of the axiom with smaller formulas.

[.column]

$$
\begin{array}{ccc}
\begin{array}{c}
\\\hline
\vdash A \otimes B, \overline{A} \llpar \overline{B}
\end{array}
\\
\Downarrow
\\
\begin{array}{c}
\begin{array}{c}
\\\hline
\vdash A, \overline{A}
\end{array}
\qquad
\begin{array}{c}
\\\hline
\vdash B, \overline{B}
\end{array}
\\\hline
\vdash A \otimes B, \overline{A}, \overline{B}
\\\hline
\vdash A \otimes B, \overline{A} \llpar \overline{B}
\end{array}
\end{array}
$$

---

## What Do? ② Identity Expansion

[.column]

On process, it rewrites forwarders to processes that explicitly do the forwarding.

But...

It is defined by recursion on the types of the endpoints—written over the arrow.

[.column]

$$
\begin{array}{c}
\overset{A \otimes B}{y \lnk x}, \overset{A \llpar B}{x \lnk y}
\\
\\
\triangleq
\\
\\
x(z).y[w].(
    z \overset{A}{\lnk} w
    \parallel
    x \overset{B}{\lnk} y
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

<br />

Expand the forwarder

**lazily**

in response to the kind of message received.

[.column]

$$
\begin{array}{c}
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\\\
\triangleq
\\\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x}(\bar{y}).w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\\\\
\downarrow
\\\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

<br />

Expand the forwarder

**lazily**

in response to the kind of message received.

[.column]

$$
\begin{array}{c}
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\\\
\vphantom{\triangleq}\hphantom{(\nu x \bar{x})( x[y].P \parallel \bar{x}(\bar{y}).w[z].( \bar{y} \lnk z \parallel \bar{x} \lnk w )}
\\\\
\downarrow
\\\\
\vphantom{\downarrow}
\\\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

<br />

Expand the forwarder

**lazily**

in response to the kind of message received.

[.column]

$$
\begin{array}{c}
\\\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\
\downarrow
\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

<br />

Expand the forwarder

**lazily**

in response to the kind of message received.

[.column]

$$
\begin{array}{c}
\\
\small\text{(Forward-Delegate)}
\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\
\downarrow
\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

<br />

Expand the forwarder

**lazily**

in response to the kind of message received.

But...
**Does this work?**

[.column]

$$
\begin{array}{c}
\\
\small\text{(Forward-Delegate)}
\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\
\downarrow
\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

$$
\begin{array}{c}
\small\text{(Forward-Delegate)}
\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\\\
\triangleq
\\\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x}(\bar{y}).w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\\\\
\downarrow
\\\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

[.column]

$$
\begin{array}{c}
\small\text{(Forward-Delegate-Receive?)}
\\
(\nu x \bar{x})(
    x(y).P
    \parallel
    \bar{x} \lnk w
)
\\\\
\triangleq
\\\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    w(z).\bar{x}[\bar{y}].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

---

## What Do? ③ Make It Lazy

[.column]

$$
\begin{array}{c}
\small\text{(Forward-Delegate)}
\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\\\
\triangleq
\\\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x}(\bar{y}).w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\\\\
\downarrow
\\\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

[.column]

$$
\begin{array}{c}
\small\text{(Forward-Delegate-Receive?)}
\\
(\nu x \bar{x})(
    x(y).P
    \parallel
    \bar{x} \lnk w
)
\\\\
\triangleq
\\\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    w(z).\bar{x}[\bar{y}].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

# Uh Oh?

---

## What Do? ③ Make It Lazy

[.column]

<br />

This reduces...

$$
\begin{array}{c}
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
\\
\downarrow
\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
\end{array}
$$

...using $$\text{(Forward-Delegate)}$$.

[.column]

<br />

This is **stuck**.

$$
\begin{array}{c}
(\nu x \bar{x})(
    x(y).P
    \parallel
    \bar{x} \lnk w
)
\\
\phantom{\downarrow}
\\
\phantom{(\nu x \bar{x})(\nu y \bar{y})( P \parallel w[z].( \bar{y} \lnk z \parallel \bar{x} \lnk w ))}
\end{array}
$$

Is that bad? No!

[.footer: (Disclaimer: That's kind of what "forwarder" means, isn't it?)]

---

# [fit] Conclusion: Make It Lazy!

Replace $$\text{(Forward)}$$ with Lazy Identity Expansion...
$$
\begin{array}{c}
&\qquad&
\hphantom{
(\nu x \bar{x})(\nu y \bar{y})( P \parallel w[z].( \bar{y} \lnk z \parallel \bar{x} \lnk w )}
\\
\small\text{(Forward-Delegate)}
&\qquad&
\small\text{(Forward-Choose)}
\\
(\nu x \bar{x})(
    x[y].P
    \parallel
    \bar{x} \lnk w
)
&\qquad&
(\nu x \bar{x})(
    x\sel\inl.P
    \parallel
    \bar{x} \lnk w
)
\\
\downarrow
&\qquad&
\downarrow
\\
(\nu x \bar{x})(\nu y \bar{y})(
    P
    \parallel
    w[z].(
        \bar{y} \lnk z
        \parallel
        \bar{x} \lnk w
    )
)
&\qquad&
(\nu x \bar{x})(
    P
    \parallel
    w\sel\inl.
    \bar{x} \lnk w
)
\end{array}
$$

...and the simplified metatheory just works!*

[.footer: (*: Mostly. The definition of dependency becomes slightly more complicated.)]