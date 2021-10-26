---
title: Quantum Computing
author: admin
type: page
date: 2021-10-14T16:36:38+00:00

---
$$\newcommand{\bra}[1]{\left<#1\right|}\newcommand{\ket}[1]{\left|#1\right>}\newcommand{\bk}[2]{\left<#1\middle|#2\right>}\newcommand{\bke}[3]{\left<#1\middle|#2\middle|#3\right>}$$

### Classical Computation with Linear Algebra
one bit system

`$\left(\begin{array}{c} 1 \cr 0 \end{array}\right)$` `$\begin{matrix}0 \cr 1 \end{matrix}$`, this means the bit is in state 0

`$\left(\begin{array}{c} 0 \cr 1 \end{array}\right)$` `$\begin{matrix}0 \cr 1 \end{matrix}$`, this bit is in state 1

For a two bit system we consider the tensor product between two arbitrary one bit systems

$$\left(\begin{array}{c} 0 \cr 1 \end{array}\right) \otimes \left(\begin{array}{c} 1 \cr 0 \end{array}\right) = \left(\begin{array}{c} 0 \cr 0 \cr 1 \cr 0 \end{array}\right) \begin{matrix}00 \cr 01 \cr 10 \cr 11 \end{matrix}$$

#### Dirac-Notation
The base states for one bit are 

$$\ket{0} = \left(\begin{array}{c} 1 \cr 0 \end{array}\right)$$

$$\ket{1} = \left(\begin{array}{c} 0 \cr 1 \end{array}\right)$$

The tensor product between some one bit states can be written as `$\ket{0} \otimes \ket{1}$` or in short just `$\ket{01}$`. So the four different combinations of a two bit system are `$\ket{00}$`, `$\ket{01}$`, `$\ket{10}$`and `$\ket{11}$`, or `$\ket{0}$`, `$\ket{1}$`, `$\ket{2}$`, `$\ket{3}$`

#### Logic Gates as linear operators

- NOT Gate: `$\begin{pmatrix} 0 & 1 \cr 1 & 0 \end{pmatrix}$`
- AND Gate: `$\begin{pmatrix} 1 & 1 & 1 & 0 \cr 0 & 0  & 0 & 1\end{pmatrix}$`, notice the AND Operator will reduce the dimension of output (2 inputs, 1 output), this means it is irreversible, information will be "lost", or better it will be dissipated as heat for example
- NAND Gate: `$ N = \begin{pmatrix} 0 & 0 & 0 & 1 \cr 1 & 1  & 1 & 0\end{pmatrix}$`

This gates can now be used to perform computation using the rules of matrix multiplication together with the tensor product.

Compute XOR using NAND Gates:

$$N((N((N \cdot ab) \otimes b)) \otimes (N(N \cdot ab) \otimes b))$$

Where a and b are arbitrary bits (represented using a two dimensional vector) and ab is the tensor product of the two bits, resulting in a four dimensional vector

This is essential just another notation as to write the combination of NAND Gates using boolean algebra.
- TODO: insert boolean algebra formula for NAND Gate

The XOR Gate is a essential building block for the full adder. 
- TODO: insert formula for full adder, insert mathematica code example for computing

### Transition to Quantum Computing
In Quantum Computing not only the states 0 and 1 are possible, but also all infinte combinations between them. (superposition)
A Qubit can be written with two probabilites, and with the corresponding basis vectors:
$$\ket{\psi} = \left(\begin{array}{c} p_0 \cr p_1 \end{array}\right) = p_0 \ket{0} + p_1 \ket{1}$$
Note that `$p_0$` and `$p_1$` are complex numbers and the probabilites need to be normalized to one (vector with length 1).
So geometrically all possible superposition are on a circle.
Also note that `$p_0$` and `$p_1$` are in fact not probabilites, but probability amplitudes (so the modulus square gives the probability). Also they contain more information than just probabilites, thats why there are imaginary numbers.   

=> phase kickback

{{< figure src="superposition.png" width="100%" >}}


#### Bloch Sphere
Another way to view a qubit is the Bloch Sphere. 
For the Bloch Sphere it is important to note when measuring, the wave function collapses and the probability to measure the state i is `$|p_i|^2$`. (this is a fundamental statement of the copenhagen interpretation)
Futhermore the sum of all probabilites must sum to 1 `$|p_0|^2 +|p_1|^2 = 1$`
These two facts allow us to eliminate two variables (global phase and normalization), so a Qubit can be described using only two variables.

$$ \ket{\psi} = \cos{\frac{\theta}{2}}\ket{0} + e^{i\phi}\sin{\frac{\theta}{2}\ket{1}} $$

Now in fact one can see that only `$\theta$` describes the probaility to measure a state, but `$\phi$` (the relative phase) is still very important and makes a physcial difference, when performing operations using gates or multiple Qubits.

So all we did is to show that with a suitable change of coordinates (Hopf coordinates), we can clearly see the actual degrees of freedom of a Qubit.

$$p_0 = \cos{\frac{\theta}{2}} $$
$$ p_1 = e^{i\phi}\sin{\frac{\theta}{2}} $$

#### Example
Consider the two states:

$$\ket{\psi_0} = \frac{1}{\sqrt{2}}\left(\begin{array}{c} 1 \cr 1 \end{array}\right) $$
$$\ket{\psi_1} = \frac{1}{\sqrt{2}}\left(\begin{array}{c} 1 \cr -1 \end{array}\right) $$

These two states have the same probability to measure state `$\ket{0}$` or state `$\ket{1}$` (50%). But they differ in the relative phase. The representation on the bloch sphere is:

$$\ket{\psi_0} = \cos{\frac{\pi}{4}}\ket{0} + \sin{\frac{\pi}{4}\ket{1}} $$
$$\ket{\psi_1} = \cos{\frac{\pi}{4}}\ket{0} - \sin{\frac{\pi}{4}\ket{1}} $$

To make the relative phase more clear we can write

$$\ket{\psi_0} = \cos{\frac{\pi}{4}}\ket{0} + e^{i0}\sin{\frac{\pi}{4}\ket{1}} $$
$$\ket{\psi_1} = \cos{\frac{\pi}{4}}\ket{0} + e^{i\pi}\sin{\frac{\pi}{4}\ket{1}} $$
(since `$e^{i\pi} = -1$`)

Now it is clear that the two states have same probabilites (as you can see with `$\theta$`), but different relative phases, in fact they are completly different direction. So when applying the hadarmard gate, these two states have different results. Also thats the reason why applying the hadarmard gate two times in a row cancel itself out. (phase interference)

Another example state:

$$\ket{\psi} = \frac{1}{\sqrt{2}}\left(\begin{array}{c} 1 \cr i \end{array}\right) $$

Again, if we would measure the state, it has equal probabilites as the states above, but differs in the relative phase.

$$\ket{\psi_1} = \cos{\frac{\pi}{4}}\ket{0} + e^{i\frac{\pi}{2}}\sin{\frac{\pi}{4}\ket{1}} $$

(`$e^{i\frac{\pi}{2}} = i$`) 

Doing something like 
$$\ket{\psi} = \frac{1}{\sqrt{2}}\left(\begin{array}{c} i \cr i \end{array}\right) $$

doesnt change anything, since now we changed the global phase. 

$$\ket{\psi} = \left(\begin{array}{c} \frac{1}{\sqrt{2}} \cr \frac{1}{\sqrt{4}} + \frac{1}{\sqrt{4}}i \end{array}\right) $$

TODO:
- only lower half needed to reach all possible (physical important...) states?
- how to compute phi in general case?
$$a + bi$$
$$\phi = \arcsin(\frac{b}{\sin{\arcsin{\sqrt{a^2+b^2}}}})$$

#### two Qubit System
A two Qubit system is the tensor product between two Qubits, so in general
$$\left(\begin{array}{c} p_0 \cr p_1 \end{array}\right) \otimes \left(\begin{array}{c} l_0 \cr l_1 \end{array}\right) = \left(\begin{array}{c} p_0l_0 \cr p_0l_1 \cr p_1l_0 \cr p_1l_1 \end{array}\right)$$

Now there can be a state of a two-Qubit system, that can not be decomposed as a tensor product of two seperate Qubits. This state is said to be entagled. In classical computation no entaglement is possible.

- TODO: fix image, example for entaglement
- bloch sphere for two qubits?

$$ \left| \phi^+ \right> = \frac{1}{\sqrt{2}} (\left| 00 \right> + \left| 11 \right>) = \left(\begin{array}{c} \frac{1}{\sqrt{2}} \cr 0 \cr 0 \cr \frac{1}{\sqrt{2}} \end{array}\right) $$
 => one of four bell states (maximum quantum entaglement, cannot be explained with classical theories, localism or realism has to be given up) 

- Reason for only reversible Quantum Gates? Because entagled state information loss would change things?
What happens if the NAND Gate is applied to the bell state?

$$\begin{pmatrix} 0 & 0 & 0 & 1 \cr 1 & 1  & 1 & 0\end{pmatrix} \left(\begin{array}{c} \frac{1}{\sqrt{2}} \cr 0 \cr 0 \cr \frac{1}{\sqrt{2}} \end{array}\right) = \left(\begin{array}{c} \frac{1}{\sqrt{2}} \cr \frac{1}{\sqrt{2}}  \end{array}\right) $$
