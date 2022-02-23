# Task 1:  
## Find the combination of numbers that summed gives a particular one
The job is divided into 2 python notebooks:
<ol>
  <li> <code>main.ipynb</code>: where the algorithm can be tested </li>
  <li> <code>tools.ipynb</code>: where the functions are created</li>
</ol>

  
I tried to find the simplest and most shallow circuit.

Hope everything is clear enough

(*unfortunately the equations are black so it is possible to view them in light theme*)
    
        
## Explanation of the algorithm:
The algorithm has 2 registers:
<ol>
  <li> To create combinations of items from <code>numbers</code>, which requires as many qubits as the length of the list</li> 
  <li> To collect possible sums of combinations of items from <code>numbers</code>. Its length is <img src="https://render.githubusercontent.com/render/math?math==\log_2\left(\sum_{i\in numbers} i\right)"></li> to be able to describe any possible combination.
</ol>
   
<image src="/circuit_tester.png"/>

The **SEL** gate select the set of combinations we want to test. We implemented the *Hadamard transform* $=\sum_{i=0}^{2^n}|i\rangle$ (namely all the possible combinations) the *W state* $=\sum_{\ell=0}^{n-1}|1\rangle_\ell\otimes |0\rangle_{(\ell)}$ [<sup id="fn1-back">1</sup>](#fn1 "meaning of (\ell)")(one number at a time) or *W state+X* $=\sum_{\ell=0}^{n-1}|0\rangle_\ell\otimes |1\rangle_{(\ell)}$  (neglecting one number at a time).
In this way, we create an overlap of some possible combinations of items from <code>numbers</code> on the $1^{st}$ register. 

Instead of using the *Draper adder* to sum 2 numbers, we used the second addendum to store the different sums. On the other hand, the first term is 'virtual': it is encoded in the CPs activated from the qubits of the $1^{st}$ register.

To better understand how the algorithm works, let us consider an example.

For instance if we have <code>numbers=[3,2,1]</code> the first number to implement is $3=2^1+2^0$. So, to implement the $2^1$, from the $1^{st}$ qubit of the $1^{st}$ register we apply a $CP(\pi/2)$ on the second last qubit of the $2^{nd}$ register (the $5^{th}$) and a $CP(\pi/4)$ on the $4^{th}$ qubit of the $2^{nd}$ register: $CP_{(1,5)}(\pi)CP_{(1,4)}(\pi/2)$. Likewise, to implement the $2^0$ we will apply $CP_{(1,6)}(\pi)CP_{(1,5)}(\pi/2)CP_{(1,4)}(\pi/4)$. 
In this way the first addendum is 'virtual', it is embedded in this procedure.
Then we iterate the same for the $2$ and the $1$. 

In correspondance to $|1--\rangle$, we are adding to the $2^{nd}$ register the state:
\begin{equation}
\begin{split}
\otimes_{k=0}^{n}\frac{1}{\sqrt{2}}\left(|0\rangle_k + e^{2\pi i (3/2^k)}|1\rangle_k\right) = & \frac{1}{\sqrt{2^n}}\sum_{j=0}^{2^n-1} e^{2\pi i (3j)/2^n}|j\rangle\\ 
= & \frac{1}{\sqrt{8}}\left(|000\rangle + e^{2\pi i (3/8)}|001\rangle+e^{2\pi i (6/8)}|010\rangle+e^{2\pi i (9/8)}|011\rangle\\+e^{2\pi i (12/8)}|100\rangle+e^{2\pi i (15/8)}|101\rangle+e^{2\pi i (2/8)}|110\rangle+e^{2\pi i (5/8)}|111\rangle\right)\\
= & QFT|3\rangle
\end{split}
\end{equation} 
where $n$, the number of items in the list, is $n=3$. 

In the same way, with $|-1-\rangle$, we will add $QFT|2\rangle$ and then with $|--1\rangle$, $QFT|1\rangle$.

In correspondence of the state $|101\rangle$ in the $1^{st}$ register, the second register will be in
$$\otimes_{k=0}^{n=3}\frac{1}{\sqrt{2}}\left(|0\rangle_k + e^{2\pi i \left((3/2^k)+(1/2^k)\right)}|1\rangle_k\right)= \otimes_{k=0}^{n=3}QFT(3+1)$$
therefore, by appling a $QFT^\dagger$ to the $2^{nd}$ register, we get $(3+1)$, namely the state $|101\rangle\otimes |4\rangle$.


The initial <code>QFT</code> is replaced by a **H transform** since the input on the $2^{nd}$ register is zero.

Finally, the required number (<code>solution</code>) could be converted into binary and implemented on the second register (appling <code>X</code>s in correspondence of $1$s), to filter states equal to zero.

Anyway, this procedure requires an extra layer on the quantum circuit that can be replaced at zero cost by directly filtering states equal to <code>solution</code> obtained on the $2^{nd}$ register.


[<sup id="fn1">1</sup>](#fn1-back) where $(\ell)$ means all qubits execept the $\ell^\text{th}$
