# Chapter 8: Fitting models of discrete character evolution

## Biological motivation: The evolution of limbs and limblessness



![



### Key Biological Questions

-	How do we calculate the likelihoods of Mk and extended-Mk models on phylogenetic trees?
-	How can we use these approaches to test hypotheses about character evolution?

## Fitting Mk models to comparative data









When we have comparative data the situation is more complex. If we knew the ancestral character states and states at every node in the tree, then calculation of the overall likelihood would be straightforward – we could just apply the approach above many times, once for each branch of the tree. However, there are two problems with this approach. First, we don’t know the starting state of the character at the root of the tree, and must treat that as an unknown. Second, we are modeling a process that is happening independently on many branches in a phylogenetic tree, and only observe the states at the end of these branches. All of the character states at internal nodes of the tree are unknown. The likelihood that we want to calculate has to be summed across all of these unknown character state possibilities on the internal branches of the tree.



---

### Box 8.1: Felsenstein's pruning algorithm

Felsenstein’s pruning algorithm is an example of dynamic programming, a type of algorithm that has many applications in comparative biology. In dynamic programming, we break down a complex problem into a series of simpler steps that have a nested structure. This allows us to reuse computations in an efficient way and speeds up the time required to make calculations.

<<< Figure 8.2 (four panels) >>>
Panel A: 
Panel B:
Panel C:


The best way to illustrate Felsenstein’s algorithm is through an example, which is presented in the figure above. We are trying to calculate the likelihood for a three-state character on a phylogenetic tree that includes six species. In the figure, each tip and internal node in the tree has four boxes, which will contain the probabilities for the three character states at that point in the tree (Figure 8.2 A).


2.	Next, we identify a node where all of its immediate descendants are tips. There will always be at least one such node; often, there will be more than one, in which case we will arbitrarily choose one. For this example, we will choose the node that is the most recent common ancestor of species A and B, labeled as node 1 in Figure 8.2 B.


and
finally

We can use a similar approach to find that:

and




Where  is the prior probability of that character state at the root of the tree. We will take these prior probabilities to be equal for each state, or uniform (); two other possibilities are given in the text. The likelihood for our example, then, is:

---



## Using maximum likelihood to estimate parameters of the Mk model



If we apply the pruning algorithm across a range of different values of q, the likelihood changes. To find the ML estimate of q, we simply need to try a range of q-values, and stop at the value of q that has the highest log-likelihood.



XXX Lizard example

The example above considers maximization of a single parameter, which is a relatively simple problem. When we extend this to a multi-parameter model – for example, the extended Mk model will all rates different (ARD) – maximizing the likelihood becomes much more difficult. A large number of algorithms exist to solve this problem (multivariate optimization methods); this is a bit outside the scope of this chapter. The R exercise associated with this chapter will allow you to explore some R functions for optimization. 

We can also analyze this model using a Bayesian MCMC framework. We can modify the standard approach to Bayesian MCMC (see chapter 2):


2.	Given the current parameter value, select new proposed parameter values using the proposal density . For example, we might use a uniform proposal density with width 0.2, so that .
3.	Calculate three ratios:
4.	Find the product of the prior odds, proposal density ratio, and the likelihood ratio. In this case, both the prior odds and proposal density ratios are 1, so: 





## Exploring Mk: the "total garbage" test



One problem that arises sometimes in maximum likelihood optimization happens when instead of a peak, the likelihood surface has a long flat “ridge” of equally likely parameter values. In the case of the Mk model, it is common to find that all values of q greater than a certain value have the same likelihood. This is because above a certain rate, evolution has been so rapid that all traces of the history of evolution of that character have been obliterated. After this point, character states of each lineage are random, and have no relationship to the shape of the phylogenetic tree. Our optimization techniques will not work in this case because there is no value of q that has a higher likelihood than other values. Once we get onto the ridge, all values of q have the same likelihood.







This equation gives the likelihood of the “total garbage” model for any value of p. Equation 8.2 is related to a binomial distribution (lacking only the factorial term). We know from probability theory that the ML estimate of p is k / n, with likelihood given by the above formula.








## Testing for differences in the forwards and backwards rate of character change


I have been referring to an example of flower evolution throughout this chapter, but we have not yet tested the hypothesis that I stated in the introduction: that transition rates from actinomorphy to zygomorphy are much higher than the reverse.



(eq. 8.3)		

Notice that model one has one parameter, while the other has two. One can compare them using standard methods discussed in previous chapters – that is, a likelihood-ratio test, AIC, BIC, or other similar methods.

We can apply all of the above methods to analyze the evolution of limblessness in squamates. We can use the tree and character state data from Brandley et al. (2008), which is plotted with ancestral state reconstructions as Figure 8.2.

![Figure 8.2. Reconstructed patterns of the evolution of limbs and limblessness across squamates. Tips show states of extant taxa (here, I classified species with neither fore- nor hindlimbs as limbless, which is conservative given the variation across this clade (see chapter 7). Pie charts on internal nodes show proportional marginal likelihoods for ancestral state reconstruction. Data from Brandley et al. 2008.]({{ site.baseurl }}/images/figure8-3.png)

If we fit an Mk model to these data assuming equal state frequencies at the root of the tree, we obtain a lnL of -80.5 and an estimate of the Q matrix as:





A Bayesian analysis of the ARD model gives similar conclusions (Figure 8.3). We can see that the posterior distribution for the backwards rate (q21) is higher than the forwards rate (q12), but that the two distributions are broadly overlapping.



You might wonder about how we can reconcile these results, which suggest that squamates gain limbs at least as frequently as they lose them, with our biological intuition that limbs should be much more difficult to gain than they are to lose. But keep in mind that our comparative analysis is not using any information other than the states of extant species to reconstruct these rates. In particular, identifying irreversible evolution using comparative methods is a problem that is known to be quite difficult, and might require outside information in order to resolve conclusively. For example, if we had some information about the relative number of mutational steps required to gain and lose limbs, we could use an informative prior – which would, I suspect, suggest that limbs are more difficult to gain than they are to lose. Such a prior could dramatically alter the results presented in Figure 8.3. We will return to the problem of irreversible evolution later in the book (Chapter 13).

## Chapter summary

In this chapter I describe how Felsenstein’s pruning algorithm can be used to calculate the likelihoods of Mk and extended-Mk models on phylogenetic trees. I have also described both ML and Bayesian frameworks that can be used to test hypotheses about character evolution. This chapter also includes a description of the “total garbage” test, which will tell you if your data has information about evolutionary rates of a given character.

Analyzing our example of lizard limbs shows the power of this approach; we can estimate transition rates for this character over macroevolutionary time, and we can say with some certainty that transitions between limbed and limbless have been asymmetric. In the next chapter, we will build on the Mk model and further develop our comparative toolkit for understanding the evolution of discrete characters.