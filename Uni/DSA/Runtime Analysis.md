References: CLRS Chapter 3

- Asymptotic Efficiency - Input sizes large enough to make only the order of growth of the runtime relavant

- In runtime analysis $=$ and $\in$ have the same meaning. Why?? 
### O Notation
- Gives an upper bound on the asymptotic behavior - (function grows at most as fast as a certain rate)
- Hence, O(g(n)) is the set of all functions with a lower or same order of growth as g(n) (to within a constant multiple, as n goes to infinity).
- We say that $f(n) \in O(g(n))$  if there exists a positive constant $c$ such that for all $n>n_0$ , $0\le f(n) \le cg(n)$

### $\Omega$ Notation
- Gives an upper bound on the asymptotic behavior - (function grows at most as fast as a certain rate)
- Hence, Ω(g(n)), stands for the set of all functions with a higher or same order of growth as g(n) (to within a constant multiple, as n goes to infinity).
-  We say that $f(n) \in \Omega(g(n))$  if there exists a positive constant $c$ such that for all $n>n_0$ , $0\le cg(n) \le f(n)$

### $\Theta$ Notation
- ϴ(g(n)) is the set of all functions that have the same order of growth as g(n) (to within a constant multiple, as n goes to infinity).
- We say that $f(n) \in \Theta(g(n))$  if there exists two positive constants $c_1$ and $c_2$ such that for all $n>n_0$ , $0\le c_1g(n) \le f(n) \le c_2g(n)$ (f(n) can be sandwiched between g(n))
- Note that
	- $\Theta(g(n)) \subseteq O(g(n))$


![[Pasted image 20231218192759.png]]

Common runtimes analysed

- logarithmic functions
	- $log_2(n)$ vs $ln(n)$
	- $ln(n) = log_2(n)/log_2(e)$
	- Hence $O(ln(n)) = O(log_2(n))$
	- 