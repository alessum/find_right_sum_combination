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
   
![scheme light](./circuit_scheme_w.png#gh-light-mode-only)
![scheme dark](./circuit_scheme_b.png#gh-dark-mode-only)

The **SEL** gate select the set of combinations we want to test. We implemented the *Hadamard transform* <image src="/eqs/circuit_tester(2).png" width="9%"/> (namely all the possible combinations) the *W state* <image src="/eqs/circuit_tester(1).png" width="15%"/> [<sup id="fn1-back">1</sup>](#fn1 "meaning of (\ell)")(one number at a time) or *W state+X* <image src="/eqs/circuit_tester(3).png" width="15%"/> (neglecting one number at a time).
In this way, we create an overlap of some possible combinations of items from <code>numbers</code> on the <image src="/eqs/circuit_tester(4).png" width="3%"/> register. 

Instead of using the *Draper adder* to sum 2 numbers, we used the second addendum to store the different sums. On the other hand, the first term is 'virtual': it is encoded in the CPs activated from the qubits of the <image src="/eqs/circuit_tester(4).png" width="3%"/> register.

To better understand how the algorithm works, let us consider an example.

For instance if we have <code>numbers=[3,2,1]</code> the first number to implement is <image src="/eqs/circuit_tester(5).png" width="8%"/>. So, to implement the <image src="/eqs/circuit_tester(6).png" width="3%"/>, from the <image src="/eqs/circuit_tester(4).png" width="3%"/> qubit of the  <image src="/eqs/circuit_tester(4).png" width="3%"/> register we apply a <image src="/eqs/circuit_tester(9).png" width="7%"/> on the second last qubit of the  <image src="/eqs/circuit_tester(8).png" width="3.5%"/> register (the <image src="/eqs/circuit_tester(10).png" width="3.5%"/>) and a <image src="/eqs/circuit_tester(11).png" width="7%"/> on the <image src="/eqs/circuit_tester(12).png" width="3.5%"/> qubit of the <image src="/eqs/circuit_tester(8).png" width="3.5%"/> register: <image src="/eqs/circuit_tester(13).png" width="17%"/>. Likewise, to implement the <image src="/eqs/circuit_tester(14).png" width="3%"/> we will apply <image src="/eqs/circuit_tester(15).png" width="25%"/>. The first qubit of the <image src="/eqs/circuit_tester(4).png" width="3%"/> register will then control 3 <code>c-P</code>s: <image src="/eqs/circuit_tester(7)ooo.png" width="26%"/>. 

In this way the first addendum is 'virtual', it is embedded in this procedure.
Then we iterate the same for the 2 and the 1. 

In correspondance to <image src="/eqs/circuit_tester(16).png" width="6%"/>, we are adding to the <image src="/eqs/circuit_tester(8).png" width="3.5%"/> register the state:
<image src="/eq1.png" alt="drawing" width="500"/>

where n, the number of items in the list, is <image src="/eqs/circuit_tester(19).png" width="4.5%"/>. 

In the same way, with <image src="/eqs/circuit_tester(17).png" width="6%"/>, we will add <image src="/eqs/circuit_tester(20).png" width="6%"/> and then with <image src="/eqs/circuit_tester(18).png" width="6%"/>, <image src="/eqs/circuit_tester(21).png" width="6%"/>.

In correspondence of the state <image src="/eqs/circuit_tester(22).png" width="6%"/> in the  <image src="/eqs/circuit_tester(4).png" width="3%"/> register, the second register will be in

<image src="/eqs/circuit_tester(23).png" width="90%"/>

Therefore, by appling a <image src="/eqs/circuit_tester(24).png" width="5%"/> to the <image src="/eqs/circuit_tester(8).png" width="3.5%"/> register, we get (3+1), namely the state <image src="/eqs/circuit_tester(25).png" width="9%"/>.


The initial <code>QFT</code> is replaced by the **H transform** since the input on the <image src="/eqs/circuit_tester(8).png" width="3.5%"/> register is zero.

Finally, the required number (<code>solution</code>) could be converted into binary and implemented on the second register (appling <code>X</code>s in correspondence of <code>1</code>s), to filter states equal to zero.

Anyway, this procedure requires an extra layer on the quantum circuit that can be replaced at zero cost by directly filtering states equal to <code>solution</code> obtained on the <image src="/eqs/circuit_tester(8).png" width="3.5%"/> register.


[<sup id="fn1">1</sup>](#fn1-back) where <image src="/eqs/circuit_tester(26).png" width="3%"/> means all qubits execept the <image src="/eqs/circuit_tester(27).png" width="3.5%"/>
