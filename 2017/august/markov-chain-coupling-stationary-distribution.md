source: [lecture notes](https://www.cc.gatech.edu/~vigoda/MCMC_Course/MC-basics.pdf)

# markov chain

## markov chain definition

- a random process is a sequence of random variables ($`X_t \in \Delta`$) . 
- markov chain is one type of random process s.t. $`P(X_{t+1}=y | X_0=x_0, ..., X_t=x_t) = P(X_{t+1}=y | X_t=x_t)`$

if the transition is independent of time ($`t`$ can be dropped), then it's *time-homogeneous*. 

  - transition matrix denoted as $`P(x, y) = Pr(X_{t+1}=y | X_t=x)`$
  - we focus on it

in summary, a markov chain is *a sequence of random variables with 1st order conditional independence*. 

## stationary distribution

a distribution $`\pi`$ is stationary if it's **invariant** w.r.t $`P(x, y)`$. 

- invariant means:  for all $`y \in \Delta`$, $`\pi(y) = \sum_z \pi(x) P(x, y)`$
- like $`\pi`$ is like the eigen vector to $`P`$ with eigen value equals to 1.
- interpretation: 
  - after one transition on $`\pi`$, the new distribution remains the unchanged
  - in other words, after arbitrary number of transitions, the distriution is fixed
- in other words, reaches *equilibrium*

## ergodicity

a MC is ergodic if there exists some $`t`$ s.t. for all $`x, y`$, $`P^t(x, y) > 0 `$ 
  - the $`t`$ is global
  - interpretation: for some $`t`$, all nodes are reachable from each other

equivalent properties to ergodicity:

1. irreducible: for all $`x, y`$, there some exists some $`t`$ such that $`P^t(x, y) > 0 `$
   - note that the $`t`$ can be different for different $`x`$ and $`y`$ pairs
2. aperiodic: $`gcd({t: P^t(x, x) > 0}) = 1`$
   - a sequence of $`t`$ like $`{2, 4, 6, 8, ...}`$ is not allowed, the gcd is 2 in this case
   - why this? imagine $`x`$ and $`y`$. one with period 2 and the other with period 3. there are some $`t`$ that $`P^t(x, x) = 0`$ while $`P^t(y, y) > 0`$ or the other way around.
     - this does not satisfy ergodicity
   - in other words, if $`P(x, x)>0`$, then it's aperiodic (sufficient but not necessary condition)

usefulness of ergodicity by theorem 2.1: regardless of the initial state, the markov chain reaches *unique* stationary distribution

in other words, when designing a markov chain to sample from a stationary distribution, make sure it is ergodic.

*questions*: why does global reachability ($`P^t(x, y)>0`$) implies stationary distribution?

  - dues to the nature of matrix multiplication?

## important theorem

For a **finite ergodic** Markov chain, there exists a **unique** stationary distrib $`\pi`$ such that:

for all $`x, y \in \Delta`$, $`\text{lim}_{t \rightarrow \infty} P^t(x, y) = \pi(y)`$

meaning, starting from **any** state $`x`$, there is probability $`\pi(y)`$ to get to $`y`$ for **any** $`y`$.


## ergodic MC with symmetric transition matrix

MC is reversible if there exists $`\pi`$ s.t. for all x and y, $`\pi(x) P(x, y) = \pi(y) P(y, x)`$
  - it's equally likely to transit from x to y or from y to x.

given reversbility, if transition matrix is symmetric $`P(x, y)=P(y, x)`$, then $`\pi(x) = \pi(y)`$, thus $`\pi`$ is **uniform**. 

- when is a MC reversible?

in summary, any irreducible and ergodic Markov chain with a symmetric transition matrix has uniform stationary distribution. 
  - [ref](http://people.math.gatech.edu/~randall/McmS10/riffle.pdf)

## example: random graph matching

graph matching: a set of edges without common edges

simple algorithm:

1. random select an edge $`e`$
2. if $`e`$ is in the current matching $`X_t`$ (a state), exclude it, otherwise include it. 
   - denote the new state as $`X'`$
3. if $`X'`$ is feasible (in $`\Delta`$), set $`X_{t+1}`$ with proba 1/2. otherwise, keep $`X_t`$ unchanged
4. follow the transition until we are close enough to the stationary distribution
   - related to convergence rate analysis

property of this algorithm:

- aperiodic because $`P(M, M) > 1/2`$
- irreducible: from empty set

also transition symmetric. thus the sampling is uniform. 

# coupling techniques

## definition

coupling $`w(x, y)`$ (on $`(x, y) \in \Delta \times \Delta`$) of $`u(x)`$ and $`v(y)`$ (both on $`\Delta`$):

- $`\sum_y w(x, y) = u(x)`$ for all $`x`$
- $`\sum_x w(x, y) = v(y)`$ for all $`y`$

intuitively, $`w`$ is a joint distribution whose marginal distribution is the appropriate distribution (u and v)

  - like a decomposition of x and y into a joint space?
  - $`w`$ is like a 2D matrix for MC

example: two independent markov chains

- easily verifiable that the marginal distribution satisfy the requirement (because of independence)


## usefulness

coupling bounds the *variation distance* between two distributions

variation distance:

- $`d_{TV}(u, v) = 1/2 \sum_{x \in \Delta} |u(x) - v(x)|`$

important theorem:

1. let $`(X, Y) ~ w`$ for *some* $`w`$, then $`d_{TV}(u, v) <= Pr(X \neq Y)`$
   - upperbound on $`d_{TV}(u, v)`$
2. there always exists  a coupling $`w`$ s.t. the above equality holds

interpretation of both sides

1. distance between two distributions 
2. probability that X and Y are different?
3. seems to be relevant.
   - if X are always equal to Y, then distance = 0, which means they are the same distribution

questions:

- how to find such coupling?
- how to evaluate $`Pr(X \neq Y)`$? 
  - sum up the the off-diagonal entries?

# proof of main theorem 1

aka, for ergodic markov chain, starting from any $`x`$, it reaches unique stationary distribution $`\pi(y)`$, where $`x, y \in \Delta`$. 

sketch of proof:

1. design a coupling for chain $`X`$ and $`Y`$ starting from arbitrary $`X_0`$ and $`Y_0`$
   - such coupling requries $`X_t`$ and $`Y_t`$ be indenpendently generated before they coelesed ($`X_t \neq Y_t`$)
   - after coelesion, output the same state ($`X_t=Y_t`$)
2. because of ergodicity, $`P(x, y)=\epsilon>0`$, therefore after coelesion at $`t`$, $`P(X_t \neq Y_t) \le 1-\epsilon^2`$
   - because $`P(X_t = Y_t) \ge \epsilon^2`$
3. after $`kt`$ steps, $`P(X_{kt} \neq Y_{kt}) \le (1-\epsilon^2)^k`$, in other words, $`P(X_{\infty} \neq Y_{\infty}) \le (1-\epsilon^2)^{\infty} = 0`$
4. using the coupling theorem, $`d(P^t(X_0,\cdot), P^t(Y_0, \cdot)) = 0`$ as $`t \rightarrow \infty`$, which means $`X`$ and $`Y`$ approaches the same distribution. 
5. in other words, we have $`\lim\limits_{t \rightarrow \infty} P^t(x, y) = \delta(y)`$ for any $`x`$ and $`y`$. $`\delta`$ is the **limiting distribution**.
   - note that the distribution is characterized by $`P^t`$


## why is such coupling a valid one?

1. before coelesion, they are generately independently, so it's a valid coupling by definition. 
2. after coelesion, $`w(x, y) = \begin{cases} \mu(x) & \text{if} y=x \\ 0 & \text{otherwise} \end{cases}`$. also satifying the definition. 
   - in other words, $`w`$ is a diagonal matrix with diagonal entries equal to $`\mu(x)`$

note: "coupling" means, outputing the same state for different chains


# learned

- basic concepts of markov chain, transition matrix, ergodicity, stationary distribution
  - a markov chain: a sequence of data
  - transition matrix defines a markov chain
  - ergodicity gives stationary distribution
- symmetric transition (with ergodicity) gives uniform stationary distribution
  - how to prove?
  - an example: uniform random graph matching
- coupling:
  - can be used to bound the mixing time
  - the definition is a bit strange in the lecture note
- proof of theorem 1

# thinking

- our goal: sample steiner trees from distribution defined by its weight/energy
- approach: design some markov chain that is ergodic and its stationary distribution is exactly the desired one

- if the transition matrix is symmetric and the MC is ergodic, then the sampling is uniform
  - this "coincides" with the problem, uniform random spanning tree
  - however, we do not want **uniform** sampling, so the chain should not be symmetric

## coupling from the past

- why the sample is a perfect sample?

consider starting from every states, after `k` steps, they all transit to the same state. 

in other words, starting from `t` (inclusive), all chains give the same distribution (it becomes stationary using the coupling technique), therefore, the sample at `t` is drawn from the stationry distribution. 

definition of stationarity: starting from **any** state, the output state is distributed according to the **same** distribution.



