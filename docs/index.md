# Overview

!!! warning "Warning"

    This project was developed from 2010 to 2016 and is no longer maintained

In this work we aim at providing an effective support for reasoning about
imperative programs with data structures and aliasing, by extending the
expressiveness of more familiar type-based verification towards more informative
logical reasoning, without compromising soundness and completeness.

Currently, is hard to reason about global and shape properties of data
structures. Existing tools require too much effort, a lot of experience from the
user and rely on mechanisms that don't scale.

We develop an assertion language that is closer to a programming language, that
we call, functional assertion language, which is easier to use for programmers,
and amenable to automatic verification.

This prototype checks "logical" relations between functional assertions, that
is, if an assertion is equivalent to, or implies other assertion.

## Downloads

- [Packaged Version][prototype]

## Installation

- Download the packaged version above

- Unpack the `tar.gz` file

- Execute the `funspec` script following the instructions [below](#usage)

## Usage

```
FunSpec [version 0.0.2]. (C) Copyright 2016
Usage: sh funspec [OPTION]...

Options:

  -a, --api                Force smt api usage

  -d, --debug  <mode>      Debug mode (default = NONE)

  -r, --fileInteractive    File interactive mode

  -f, --files  <file>...   Input file list

  -i, --interactive        Interactive mode

  -o, --output  <file>     Redirect program output to a file

  -p, --process            Force smt process usage

  -v, --shortVersion       Version number

  -s, --stdin              Read from standard input

  -t, --test               Test mode

      --help               Show help message

      --version            Show version of this program
```

### Notes

For option `-f`, `--files`, if `<file>` is a directory all files with
extension `.fsp` in the directory `<file>` are executed

By default, if you don't provide arguments the program will execute in
interactive mode

A FunSpec program is composed by a sequence (possibly empty) of type
definitions, typed identifiers (that correspond to free variables occurring in
assertions), or implications/equivalences between assertions, as illustrated in
the concrete syntax [below](#syntax).

## Execution Example

<figure markdown>
<iframe width="578" height="434" src="https://www.youtube.com/embed/KB9wWof3eBI" title="FunSpec Execution Example" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<figcaption>FunSpec Execution Example</figcaption>
</figure>

## Syntax

$$
\begin{array}{l l l l} 
\mathit{program} & :: = & program\_element^* & \mbox{(Programs)}\\
\\
\mathit{program\_element} & ::= & & \mbox{(Program Elements)}\\ 
& \mid & \mathit{type\_definition} & \\
& \mid & \mathit{typed\_id} & \\
%& \mid & \mathit{predicate\_definition} & \\
& \mid & \mathit{assertion} \otimes \mathit{assertion} &\\
& \mid & \mathit{clear\_env} \mid \mathit{show\_env}\\
\\
\mathit{type\_definition} & ::= & \mathbf{type} \; x = \tau & \mbox{(Type Definition)}\\ 
\\
\mathit{typed\_id} & ::= & x:\tau & \mbox{(Typed Identifier)}\\ 
\\
\mathit{clear\_env} & ::= & & \mbox{(Clear Environments)}\\
& \mid & \mathbf{clear} & \mbox{(Clear All)}\\
& \mid & \mathbf{clear}\; \mathbf{vars} & \mbox{(Clear Variables)}\\
& \mid & \mathbf{clear}\; \mathbf{types} & \mbox{(Clear Type Definitions)}\\
\\
\mathit{show\_env} & ::= & & \mbox{(Show Environments)}\\
& \mid & \mathbf{show} & \mbox{(Show All)}\\
& \mid & \mathbf{show}\; \mathbf{vars} & \mbox{(Show Variables)}\\
& \mid & \mathbf{show}\; \mathbf{types} & \mbox{(Show Type Definitions)}\\
\\
\otimes & ::= & \mathtt{==>} \mid \mathtt{<==} \mid \mathtt{===} & \mbox{(Operations)}\\ 
\\
\mathit{assertion} & ::= & & \mbox{(Functional Assertions)}\\ 
& \mid & \mathbf{if} \; \mathit{expr} \; \mathbf{then} \; \mathit{assertion} \; \mathbf{else} \; \mathit{assertion} & \mbox{(Conditional)}\\ 
& \mid & \mathbf{let} \; x = \mathit{expr} \; \mathbf{in} \; \mathit{assertion} & \mbox{(Local Declaration)}\\ 
& \mid & \mathbf{read} \; y \; \mathbf{as} \; x \; \mathbf{in} \; \mathit{assertion} & \mbox{(Reference Lookup)}\\
%& \mid & \mathbf{rec} \; R\left(\overline{x}\right).\; \mathit{assertion} & \mbox{(Recursive Assertion)}\\
%& \mid & \mathit{assertion}\left[\overline{\mathit{expr}}\right] & \mbox{(Application)}\\
%& \mid & \mathit{assertion} \mathbin{*} \mathit{assertion} & \mbox{(Separating Conjunction)}\\
& \mid & \mathit{expr} \mathbin{\mathbf{and}} \mathit{assertion} \mid \mathit{assertion} \mathbin{\mathbf{and}} \mathit{expr}& \mbox{(Conjunction)}\\
& \mid & \mathit{expr} \mathbin{\mathbf{or}} \mathit{assertion} \mid \mathit{assertion} \mathbin{\mathbf{or}} \mathit{expr}& \mbox{(Disjunction)}\\
%& \mid & \mathit{expr} = \mathit{expr} & \mbox{(Equality)}\\
%& \mid & \mathbf{not}\; \mathit{expr} & \mbox{(Negation)}\\
%& \mid & \mathbf{emp} & \mbox{(Empty)}\\
%& \mid & \mathbf{true} \mid \mathbf{false} & \mbox{(Boolean Values)}\\
& \mid & \mathit{expr} & \mbox{(Boolean Expresions)}
\\
\mathit{expr} & ::= & & \mbox{(Expressions)}\\ 
& \mid & \left[{\mathit{label}_i = \mathit{expr}_i}^{i \in \{ 1 \dots n \}}\right] & \mbox{(Record)}\\
& \mid & \mathit{expr}.\mathit{label} & \mbox{(Projection)}\\
& \mid & \mathit{expr}\odot\mathit{expr} & \mbox{(Binary Operations)}\\
& \mid & \ominus\mathit{expr} & \mbox{(Unary Operations)}\\
%& \mid & \mathbf{fold}\left[\tau\right]\; \mathit{expr} & \mbox{(Fold)}\\
%& \mid & \mathbf{unfold}\left[\tau\right]\; \mathit{expr} & \mbox{(Unfold)}\\
& \mid & x & \mbox{(Identifier)}\\
& \mid & value & \mbox{(Value)}\\
\\
\mathit{\odot} & ::= & & \mbox{(Binary Operators)}\\ 
& \mid & \mathtt{+} \mid \mathtt{-} \mid \mathtt{*} \mid \mathtt{/} & \mbox{(Arithmetic)}\\
& \mid & \mathtt{>} \mid \mathtt{<} \mid \mathtt{>=} \mid \mathtt{<=} \mid \mathtt{=} & \mbox{(Relational)}\\
& \mid & \mathbf{and} \mid \mathbf{or} & \mbox{(Logic)}\\
\\
\mathit{\ominus} & ::= & \mathtt{-} \mid \mathbf{not} & \mbox{(Unary Operators)}\\ 
\\
\mathit{value} & ::= & & \mbox{(Values)}\\ 
& \mid & \left[{\mathit{label}_i = \mathit{value}_i}^{i \in \{ 1 \dots n \}}\right] & \mbox{(Record)}\\
& \mid & n & \mbox{(Number)}\\
& \mid & \mathbf{null} & \mbox{(Null Reference)}\\
& \mid & \mathbf{true} \mid \mathbf{false} & \mbox{(Boolean Values)}\\
\\
\mathit{\tau} & ::= & & \mbox{(Types)}\\ 
& \mid & \mathbf{int} \mid \mathbf{bool} \mid \mathbf{ref} \;\tau & \\
& \mid & \left[{\mathit{label}_i:\mathit{\tau}_i}^{i \in \{ 1 \dots n \}}\right] & \\
%& \mid & \mathbf{rec} \; X. \tau \mid X & \\
%\\
%\mathit{predicate\_definition} & ::= & \mathbf{let}\; \mathbf{rec} \; x \; \overline{typed\_id} =  \mathit{assertion} & \mbox{(Predicate Definition)}
\end{array}
$$

---

<img src="img/fctunl.png" alt="FCT">
<img src="img/logo_novalincs.png" alt="NOVA-LINCS">

[prototype]: prototype/funspec3.tar.gz
