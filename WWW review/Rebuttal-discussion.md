# Answers to the first reviewer

**W1**:
Thank you for your comment! In the literature, the terms "restart vector" and "jump vector" are often used interchangeably. By "restarting at vertices in group $V_k$" we mean setting the jump (restart) vector $v$ such that $v[j]=\frac{1}{V_k}$ if node $j$ belongs to group $k$, and $v[j]=0$ otherwise.  We will make sure this is clarified in the revised version of the paper.

***ARIS:*** Shouldn't we also normalize v?
***Honglian*** Yes, you are right. Thank you for pointing it out!

**W2**:
Thank you for your comment. We will ensure that this is clarified and made explicit in the revised version of the paper.

**W3**:
Although both the transition matrix and the jump vector are inputs to the PageRank algorithm, the jump vector is more of a theoretical construct, representing probabilities that are largely beyond our control.

Consider a video recommendation graph as an example. Here, each node represents a video, and an edge $(u,v)$ indicates that while a user is watching video $u$, video $v$ is recommended as the next video to watch. Typically, there is a list of recommended next-to-watch videos, and the ranking of each video can be influenced by the edge weights. For instance, a recommendation list $[v_1, v_2, \cdots, v_k]$ corresponds to edge weights $w(u,v_1) \geq w(u,v_2), \cdots, \geq w(u,v_k)$. In this scenario, a moderator can adjust the rankings of the recommended videos, which in the graph translates to modifying the edge weights. 

The situation is different for the jump vector. In this context, the jump vector could represent the probability distribution of where a user might start if they either (1) ignore the recommendation list or (2) choose a video to watch on their own. This distribution reflects the user’s intrinsic preferences and is not something that can be directly influenced or controlled.

**W4**:
Thank you for your question, we will provide a more detailed analysis regarding the transition matrix change in the next version of the paper.

The original group-wise pagerank scores are listed in the last column of Table 1. The target PageRank scores are calculated by setting the first group’s score to $\phi_1 = \phi$, and distributing the remaining PageRank score, $1-\phi_1$, equally among the other groups, as explained in lines 673–678.

For the Blogs dataset, the scores $(0.48, 0.52)$ are nearly evenly distributed between the two groups. Consequently, changes to the transition matrix are substantial for both small and large $\phi$ values. In the Mind dataset, the original scores are $(0.26, 0.74)$, though they were incorrectly listed as $(0.74, 0.26)$ in the paper. As $\phi$ increases, the target scores diverge more significantly, resulting in larger weight changes. For the Slashdot dataset, the initial scores are distributed almost evenly across four groups ($(0.3, 0.17,0.36, 0.17)$). The target scores are $(\phi, \frac{1}{3}(1-\phi),\frac{1}{3}(1-\phi),\frac{1}{3}(1-\phi))$. As $\phi$ approches 0.9, the target scores deviate significantly from the original scores, leading to larger weight changes for higher $\phi$ values.


**W5**:
We believe that the meaningfulness of the ranking is ensured by remaining faithful to the original transition matrix. This fidelity is crucial for maintaining the problem’s practical relevance, as a moderator’s ability to manipulate edge weights is inherently limited.

**W6**:
Assume there are $l$ groups, let $[l] = \{1,\cdots, l\}$ and let the proportion of nodes in each group be denoted as $(P_1, \cdots, P_l)$. Let $U_j$ denote the group-wise PageRank score of group $j$, for all $j \in [l]$.

First we have $1_k^T p= (1 - \gamma)  1_k^T P^T p+ \gamma 1_k^T v \geq \gamma 1_k^T v$ (this is also stated in lines 291-294). 
For group $k$, we have $U_k = 1_k^T p \geq \gamma 1_k^Tv = \gamma P_k$, where the last equalty holds because $v$ is a uniform vector. Thus we proved the lower bound of $U_j$ is $\gamma P_k$ 

It follows that $U_k = 1 - \sum_{j \in [l] \setminus k} U_j \leq 1 - \sum_{j \in [l] \setminus k} \gamma P_j = 1 - \gamma \sum_{j \in [l] \setminus k} P_j = 1 - \gamma (1-P_k)$. In other words, the upper bound of $U_k$ is $ 1 - \gamma (1-P_k)$. 

We will ensure this is explained in detail in the next version of the paper.

We thank the reviewer again for the valuable comments and discussion!


# Answers to the final reviewer

**Q1 and Q2**:
We thank the reviewer for the constructive comments. We agree that the current normalization step differs from the commonly used projection-to-simplex method. Although we want to stress that, it is a widely adopted approach to map the solution to the feasible region and has performed well in our practical experiments. In the next version of the paper, we will revise this projection step and include a convergence analysis to further enhance the work.

**Q3**:
Thank you for your question! In the paper we use the Neumann series to calculate the matrix inverse: 
$
(I - X)^{-1} = \sum_{t=0}^\infty X^t.
$ To answer your question, we need to calculate the error of tructing the series. 

Let $S_k$ denote the partial sum of the Neumann series up to the $k$-th term:
$
S_k = \sum_{t=0}^k X^t = I + X + X^2 + \dots + X^k.
$

The truncation error $E_k$ is defined as:
$
E_k = (I - X)^{-1} - S_k.
$

We can express $E_k$ as the sum of the remaining terms:
$
E_k = \sum_{t=k+1}^\infty X^t.
$

Factoring out $X^{k+1}$ from the series we get:
$
E_k = X^{k+1} \sum_{t=0}^\infty X^t.
$

Using the formula for the Neumann series, the infinite sum $\sum_{t=0}^\infty X^t$ is equal to $(I - X)^{-1}$. Thus:
$
E_k = X^{k+1} (I - X)^{-1}.
$

Taking the matrix norm of both sides, we get:
$
\|E_k\| \leq \|X^{k+1}\| \cdot \|(I - X)^{-1}\|.
$

Given that $|X|<1$, we have:
$
\|E_k\| \leq \|X\|^{k+1} \cdot \|(I - X)^{-1}\|
$, we conclude that the relative approximation error is given by $\|X\|^{k+1}$.

In our paper, $X=(1-\gamma) P$, where, following convention, we set $\gamma = 0.15$. Consequently, $|(1-\gamma) P|_\infty = 0.85$. We conclude that the relative error of using the first $t$ term approximation is $0.85^{t+1}$. By setting $t = 50$ the relative error is smaller than 0.0003.

**Q4**:
In general, $\delta$ and $\epsilon$ are both small. To ensure no edge deletions, we must consider the smallest edge weight in the graph and set the absolute change $\epsilon$ accordingly. Specifically, we require $(1-\delta) \min_{i \neq j}P[i,j] - \epsilon > 0$. Once a $\delta$ value (for example 0.1) is chosen, we can calculate $\epsilon$ accordingly.

**Original question 5 and 6**:

We apologize for missing these two questions.

For Question 5: Please kindly refer to the answers provided for W4 to Reviewer 8Rry.

For Question 6: LFPRU and LFPRN can achieve perfect fairness only when both the jump vector and the matrix edge weights are adjusted. In our experiments, we report the fairness loss for LFPRU and LFPRN based on a uniform restart vector and the revised transition matrix, rather than the revised jump vector. This is why they do not achieve zero loss. We thank the reviewer for raising this point, and we will explicitly clarify this in the next version of the paper.

We thank the reviewer again for the valuable comments and discussion!