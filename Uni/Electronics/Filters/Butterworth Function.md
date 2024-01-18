$$|H(j\omega)|^2 = \frac{1}{1+(\frac{\omega}{\Omega_c})^{2N}}$$
N - integer, order of the filter
$\Omega_c$ - 3dB frequency, half power point

This filter gives maximally flat passband response

Plotting:
$$ w = 0, \: |H(j\omega)|^2 = 1 $$
$$ w \rightarrow \infty, \: |H(j\omega)|^2 \rightarrow \infty $$
$$ w = \Omega_c, \: |H(j\omega)|^2 = \frac{1}{2} $$
![[Pasted image 20240113204203.png]]
Gain can be expressed as
$$
\begin{aligned}
A = 20\log{|H(j\omega)|} \\
A = 10\log{|H(j\omega)|^2} \\
A = 10\log{(\frac{1}{1+(\frac{\omega}{\Omega_c})^{2N}})} \\
A = 10[\log{1} - \log{(1 + \frac{\omega}{\Omega_c}^{2N})}] \\
A = - 10\log{(1 + \frac{\omega}{\Omega_c}^{2N})}]
\end{aligned}
$$


Also note that
$$|H(j\omega)|^2 \propto power$$
And when $\omega = \Omega_c$ it is called the half power point