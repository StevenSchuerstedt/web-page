---
title: Quantum Optimization
author: admin
type: page
date: 2021-11-08T16:36:38+00:00

---
$$\newcommand{\bra}[1]{\left<#1\right|}\newcommand{\ket}[1]{\left|#1\right>}\newcommand{\bk}[2]{\left<#1\middle|#2\right>}\newcommand{\bke}[3]{\left<#1\middle|#2\middle|#3\right>}$$

Using Quantum Computing for hard (np) combinatorial problems. These problems cannot really be solved efficiently classically (only by limiting to certain problem instances, using heuristics), so the question is if there could be a significant speedup using Quantum Computing.

### Quantum algorithm paradigms
- Quantum Fourier Transformation
- Grover Operator
- Harrow-Hassidim-Lloyd 
- variational quantum eingenvalue solver (VQE)
- direct Hamiltonian simulation (SIM)

### Traveling Salesman & Orienteering Problem
- NP Hard Problems (solution can be verified in polynomial time)
- can be stated as NP-Complete Problems (as hard as any problem in NP, only decision problems)

Note: only the worst case instances are NP hard (these instance are not easy to construct), in practice a lot of instance can be solved in P time. 

Combinatorial Optimization: discrete solutions with real value function
(e.g. Knapsack, Traveling Salesman)

Formulate these combinatorial Problems as linear programming problems. (linear programs)
in case of no real numbers allowed: integer programming

Ising Hamiltonian: MaxCut is equivalent to solving Ising model interpretation

IBM cplex (optimizer for classical computers)

- Qiskit Aqua provides many algorithms (e.g. VQE and also QUAO)

https://qiskit.org/documentation/apidoc/qiskit.aqua.algorithms.html

#### Heuristic Algorithms
- most powerful algorithms are heuristcs
=> sacrifice correctness, accuracy, precision for speed
(gradient descent, deep learning, genetic algorithm)
- in contrast to classical algorithm, that are theoretically perfect but slow

### Describe Combinatorial Problems as diagonal Hamiltonian (Ising Hamiltonian)

The Hamiltonian of any physical system describes its dynamics (movement). Naturally every system converges to its minimum energy state.

Generell Strategy: convert problem to hamiltonian (hermitian operator), solution to optimization problem is the largest (smallest) eigenvalue eigenvector.

Lets consider a boolen function `$f : \{ 0, 1 \} ^n \rightarrow \mathbb{R}$`, which takes a boolean string `$\textbf{x}$` of length n (the description of a specific instance of the combinatorial problem) and outputs a score, or some real number describing the length, time, points, edges etc. 

Now lets construct a diagonal hamiltonian such that,

$$H_f \ket{\textbf{x}} = f(\textbf{x}) \ket{\textbf{x}} $$

for `$H_f$` to represent `$f(\textbf{x})$`. The size of `$H_f$` grows exponentially with the number of qubits (like the vector representation of the register-bit ket). 

In a way `$H_f$` is a gigantic look up table, with all possible values of `$f(\textbf{x})$` on its diagonal and the solution to the optimization problem is the largest eigenvalue and corresponding eigenvector (note: eigenvectors of diagonal matrix are the cartesian basis vectors, eigenvalues are the values on the diagonal).

Constructing `$H_f$` explicitly is very inefficient (np-hard...) and does not give any computationally advantage, so we will not construct it explicitly, but give a description with its fourier expansion and Pauli-Z Operators. (pseudo boolean functions??)

$$Pauli_Z = Z = \begin{pmatrix} 1 & 0 \cr 0 & -1 \end{pmatrix}$$

##### Example: MaxCut
Lets start with the QUBO (?) (quadratic unconstrained binary optimization) formulation of the MaxCut Problem.

$$ C(\textbf{s}) = \frac{1}{2} \sum_{ij} (1-s_is_j)$$

Here `$s_i$` is a spin variable `$s_i \in \{ -1, 1 \} $`, but we can achieve a change of variables to binary with:

$$ C(\textbf{x}) = \frac{1}{2} \sum_{ij} (1-(-1)^{x_i}(-1)^{x_j})$$

or even

$$ C(\textbf{x}) = \sum_{ij} (x_i + x_j - 2x_ix_j)$$

Note: the sum goes only over all existing edge connections.

Now lets look at a specific instance of the MaxCut problem as an example.

{{< figure src="/maxcut.png" title="MaxCut" width="100%" >}}

The nodes can be in group 0 or 1. A bitstring like 0101 describes that the first node is in group 0, the second node in group 1 etc. (take care with ordering)

So to evaluate the cost function we can expand the sum to only account for the relevant (existing) edges (according to problem instance, e.g. picture).

$$C(\textbf{x}) = \frac{1}{2} ( (1-(-1)^{x_1}(-1)^{x_2}) + (1-(-1)^{x_1}(-1)^{x_4}) + (1-(-1)^{x_2}(-1)^{x_4}) + (1-(-1)^{x_2}(-1)^{x_3}) + (1-(-1)^{x_3}(-1)^{x_4}))$$

(five summands, as there are five edges)

Now pluggin in values for `$\textbf{x}$` provides a according score. 

TODO: construct hamiltonian with pauli z 

construct using tensor product with identity matrix (as diagonal matrix) and pauli z-matrix (to switch sign of specific entries?)


c * ZIIIIIIII

=> Fourier expansion of boolean function? pauli z gate implements parity, 

=> cannot be efficiently computed (again np hard...) so use pseudo-boolean functions? (boolean function with boolean -> real ?)

multilinear polynomial representation (why is it called fourier?)

TODO:
- concrete example
- build hamiltonian explicitly using np.kron




Encode Problem:


Objective is now to find the maximum value of `$f(x)$` and the corresponding argument x.

$$Objective: max f(x)$$

f can be mapped to hamiltonian, with values of f for according input on diagonal.


=> solution is largest eigenvalue eigenvector

Ising (?) Spin variables <=> boolean variables (change of variables)

`$ x_S \in \{ -1, 1 \}$` or `$x_B \in \{ 0,1 \}$` with coordinate transform `$x_S = (-1)^{x_B}$`



$$H(\sigma) = \sum_{ij} J_{ij}\sigma_i \sigma_j$$

### VQE
Variational Quantum Eigensolver

=> find lowest eigenvalues of Hamiltonian (= Ising?)


### QAOA

Quantum Approximate Optimization Algorithm

- Algorithm for combinatorial optimization problems (e.g. MaxCut or TSP)

- determine starting angles `$\gamma$` and `$\beta$` ? (=> done by the algorithm?)

- integer paramter p

- problem hamiltonian? change problem description for QC?

- QAOA needs Qubit Operator (Hamiltonian) this is implemented in Qiskit using adjacency matrix for graphs 

### Physik
- Hamiltonoperator operator für Energie
- => Observable für Energiewerte?
- Eigenwerte geben Werte für Energie an


