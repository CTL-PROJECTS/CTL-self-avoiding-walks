General information about the CTL course available at https://ctl.polyphys.mat.ethz.ch/

# :wave: PROJECT Self-avoiding walks

Growing a random walk with $N$ steps (constant step length $b$) on a regular one-, two- or three-dimensional grid (grid constant $b$) is easily achieved, as such a walk can revisit grid nodes that have already been visited before. On a regular grid in one dimension, the walk can only grow to the left or right, in two and three dimensions, there are four and six possible directions, respectively. For a random walk, the mean squared end-to-end distance of the walk behaves as $\langle R^2\rangle = Nb^2$, where $b$ is the step length, and the average taken over an ensemble of random walks. Random walks are excellent models for macromolecules under certain conditions, or for studying scattering events or Brownian motion. 

Real macromolecules (self-avoiding walks) exhibit excluded volume and do not overlap. If one is interested in the statistics of real macromolecules, all random walks that revisit at least one grid node have to be disregarded. Unfortunately, the fraction of configurations that have to be disregarded grows quickly with increasing $N$. 

<img src="https://www.complexfluids.ethz.ch/images/PROJECT-self-avoiding-walk.png">

## Task I

Determine the fraction of random walks that have to be disregarded if you are interested in determining the mean squared end-to-end distance of a self-avoiding walk in two and three dimensions. Set $b=1$ and plot the fraction versus $N$. 

## Task II

Estimate the exponent $\nu$ in the relationship $\langle R^2\rangle = (N^\nu b)^2$ for an enxemble of $X$ self-avoiding walk. To be able to reliably estimate the exponent, you need a way to study long chains, with sufficiently large $N$ (like $N>100$). Just disregarding random walks that do not overlap is not a solution to this problem (why?). Naively choosing one of the still unoccupied grid nodes during the growth of the otherwise random walk is also not a solution to the problem (why?). There are however solutions to the problem. The most relevant methods are
1. Rosenbluth algorithm
2. Pivot algorithm
3. Wall+Erpenbeck enriched sampling algorithm

You are free to choose any of these algorithms or develop a new one. 

### Rosenbluth algorithm
0. Set $x=0$. 
1. Increase $x$ by one, set $W(x)=1$, and let the first node be at the origin, position ${\bf r}$<sub>0</sub>=**0**. 
2. Determine the position ${\bf r}$<sub>1</sub> of the 2nd node by randomly choosing one of the nearest neighbors of ${\bf r}$<sub>0</sub>.
3. Recursively determine ${\bf r}$<sub>j</sub> by choosing one of the $\sigma$<sub>j-1</sub> unoccupied neighbor sites of ${\bf r}$<sub>j-1</sub>. The probability of choosing ${\bf r}$<sub>j</sub> is $1/\sigma$<sub>j-1</sub>. 
4. If $\sigma$<sub>j-1</sub>=0, then the walk is rejected, set $W(x)=0$. Start a new walk from step 1.
5. Update the weight $W(x)\rightarrow \sigma$<sub>j-1</sub>$W(x)$.
6. If $j$ < $N$, add a new node by going to step 2. If $j=N$, then a self-avoiding walk of length $Nb$ and with a weight $W$ has been generated.
7. Start a new walk at step 1 until $x=X$, i.e., until $X$ walks have been generated.

Because we are interested in the mean squared end-to-end distance of the self-avoiding walks, we have to measure, each time a walk had been generated, the end-to-end distance, and denote it by $R^2(x)$. If a walk had been rejected, $R^2(x)$ can be set to an arbitrary value, because the mean squared end-to-end distance with the proper statistical weights can now be obtained via $\langle R^2\rangle =  \sum_{x} R^2(x) W(x) / \sum_{x} W(x) $.

### Pivot algorithm

0. Set $x=0$. Begin with a straight path, node-positions ${\bf r}$<sub>j</sub> = *j*$\hat{\bf x}$ for *j=0,1,..,N* (assuming unit step length *b=1*).
1. Increase $x$ by one. 
2. Randomly choose one of the *N* bonds, and rotate the path about this bond by a random angle (out of the possible angles on the regular grid).
3. Accept the rotation, if the walk is still self-avoiding. If there is overlap, reject the rotation.
4. Start a new walk at step 1 until $x=X$, i.e., until $X$ walks have been generated.

Again, because we are interested in the mean squared end-to-end distance of the self-avoiding walks, we have to measure $R^2(x)$. The mean squared end-to-end distance can now be obtained, for sufficiently larger $X$, via $\langle R^2\rangle =  \sum_{x} R^2(x) / X$, because the configurations can be considered (almost) independent. 

<img src="https://www.complexfluids.ethz.ch/images/PROJECT-self-avoiding-walk-2b.png" width="60%">

### Wall-Erpenbeck enriched sampling algorithm

This algorithm has two adjustable parameters *s* (number of steps between branchings) and *p* (functionality of a branch). They have to be optimized, once for all *N*, to come up with a fast algorithm. 

0. Set $X=0$ 
1. Grow *s* steps randomly, starting from the origin. Repeat growing *s* steps from the origin until there is no overlap. Now you have created a self-avoiding walk with *s* steps (the above figure shows the situation for *s=4*). 
2. Introduce a *p*-functional branch at the end (lateron: all the alive ends) of the existing path (the above figure shows the case of two-functional branches, *p=2*). 
3. Grow *s* steps randomly from each side of the branch (lateron: all the existing branches), i.e., grow *ps* bonds at each branch. Regard a new strand with *s* bonds as 'dead', if it produces overlap (the red bond in the above figure terminates the growth, the corresponding branch is not followed up further). 
4. Continue with 2. until *N* steps have been grown without overlap. 
5. The number $x$ of generated self-avoiding-walks is the number of terminal nodes of the tree-like structure. Increase $X$ by $x$.
6. Repeat all steps 1.-5. until the desired value for $X$ has been reached. 

Monitor the number of alive ends during the process. The parameters *s* and *p* are suitably chosen if this number is positive and smaller than 100. Determine the mean squared end-to-end distance $\langle R^2\rangle$ as an average over the $X$ squared distances between terminal nodes and the origin. 



## Task III

Compare your results with the Flory theory. This theory predicts that the exponent $\nu$ equals *3/(2+D)*, where *D* is the space dimension.  
