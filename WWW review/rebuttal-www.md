
### Reply to reviewer 8Rry 
Thank you for your questions. Please find our responses below.

#### W1

In homophilic graphs, a random walk starting from a red node is more likely to stay in the red group than the blue group.

Figures 3 and 4 in reference [30] demonstrate that red nodes in homophilic graphs primarily retain their personal PageRank scores within the red group. If random walk is forced to restart uniformly from the red group, the final PageRank score, which is calculated as the average of all personalized PageRank scores of red nodes, will disproportionately favor the red group over the blue group.

#### W2
We studied all four questions. Algorithm 1 is designed for problems 1 and 2, and algorithm 4 is designed for problems 3 and 4. 

#### W3

The original $\phi$-fairness loss involves optimizing the jump vector, while the group-adapted fairness loss is defined over a set of meaningful jump vectors, $v_1, \cdots, v_l$. We note that these jump vectors are part of the problem's setup, not the solution itself. The group-adapted fairness loss focuses on optimizing edge weights, which we believe is more actionable and feasible than optimizing the jump vector.

<!-- The original $\phi$-fairness loss involves optimizing the jump vector. However, this approach is impractical in real-world applications, as the jump vector cannot typically be manipulated.

To address this, we defined the group-adapted fairness loss over a predefined set of meaningful jump vectors, $v_1, \cdots, v_l$. We note that these jump vectors are part of the problem's setup, not the solution itself. The group-adapted fairness loss focuses on optimizing edge weights, which we believe is more actionable and feasible than revising the jump vector. -->

#### W4

 For the book dataset, the original and target group PageRank scores are $(0.10, 0.48, 0.42)$ and $(\phi, 1-\frac{\phi}{2}, 1-\frac{\phi}{2})$, respectively. When $\phi$ is small, the two distribution are similar, leading to small fairness loss and weight changes. 

 For the Twitter dataset, the original and target group PageRank scores are $(0.42, 0.58)$ and $(\phi, 1-\phi)$. As $\phi$ moves towards 1, the target diverges from the original PageRank score, thus both fairness loss and weight change increases.
 
 <!-- For the book dataset, the original group PageRank scores are $(0.10, 0.48, 0.42)$, and the target PageRank score are $(\phi, 1-\frac{\phi}{2}, 1-\frac{\phi}{2})$. when $\phi$ is small, the two distribution are similar, leading to small fairness loss and weight changes.  -->
 <!-- When $\phi$ increases and approaches 1, the target diverges significantly from the original, requiring larger transition matrix adjustments and leading to greater fairness loss. -->

<!-- For the Twitter dataset, the original group PageRank scores are $(0.42, 0.58)$, and the target PageRank score is $(\phi, 1-\phi)$. As $\phi$ moves towards 1, the target diverges from the original PageRank score, thus both fairness loss and weight change increases.  -->

<!-- For the Twitter dataset, the original group PageRank scores are $(0.42, 0.58)$, and the target PageRank score is $(\phi, 1-\phi)$. When $\phi$ is near 0.4, the fairness loss and weight change remains small. As $\phi$ moves towards 0 or 1, the target diverges from the original PageRank score, thus both fairness loss and weight change increases.  -->

#### W5

Our definition of good adjustment is to minimize changes to the original transition matrix instead of to limit changes to the PageRank vector for two reasons. First, the original PageRank vector may itself be unfair, thus staying close to the original PageRank score could be undesirable. Second, introducing a constraint to limit changes to the PageRank vector would over-constrain the problem, and complicate its resolution. 

<!-- Our definition of good adjustment is to stay true to the raw data, i.e., we want small change to the transition matrix. We do not have a metric to quantify how good the fair PageRank vector is after change, and the reason is two-fold. First, the original PageRank vector might not be good because it is unfair, so staying true to the original PageRank score may not be a good idea. Second, it we want to add a constraint to limit the amount of changes to the PageRank vector, the problem become over-defined. If we had a good metric, we can add it as a constraint to our current problem, but otherwise we can quantify the change to PageRank vector in a posterior way. -->

#### W6
<!-- Given the lower bound on the PageRank score for each group, we can derive an upper bound.  -->

Assume there are $l$ groups, and the proportion of nodes in each group is represented as $(P_1, \cdots, P_l)$. Since the lower bound PageRank score for group $j$ is $P_j \cdot \gamma$, the upper bound for group $j$ can be expressed as $1 - (1-P_j)\gamma$. 

### Reply to reviewer 8pxn
Thank you for your questions and comments. Please find our responses below.

#### Q1 
For the datasets Books, Blogs, MIND, and Twitter, we use node labels as group indicators. For the Slashdot dataset, since node labels are unavailable, we apply the METIS graph partitioning algorithm (referenced in [13] of the submitted paper) to divide the vertices into groups.

The last column of Table 1 indicates the number of groups, corresponding to the size of the original group-wise PageRank score tuple. We will clarify this in the next version of the paper.

#### Q2

Our framework can accommodate both extreme cases:

(1) Each node is treated as a group. In this senario, we can modify Equation (3.1) by setting the group-wise target PageRank scores, $\phi$, as a n-dimensional vector, where $\phi_k$ represents the target PageRank score for node $k$. Additionally, $1_k$ can be set as the indicator for node $k$. In Equation (3.4), we can further set $v_k$ to be the indicator of node $k$.
(2)
When all nodes belong to one group, there are no fairness issues to address. If we need to assign specific PageRank score to certain subgroup, we can modify $\mathbf{\phi}$, $1_k$, and ${v}_k$ in Equation (3.1) and (3.4) accordingly.

#### cons (The experimental results in Section 5.4 do not demonstrate the superiority to LFPR [30])
We compared FairGD with LFPR on the Blogs and Mind dataset. On the Mind dataset, FairGD achieved both smaller fairness loss and smaller weight changes compared to LFPR. On the Blogs dataset, although FairGD exhibited slightly higher fairness loss than LFPR, the absolute fairness loss was minimal (less than 0.015), indicating that both algorithms produced near-perfectly fair solutions. Furthermore, FairGD outperformed LFPR in weight change, and achieved smaller weight changes for 8 out of 9 $\phi$ values.

These results confirm that FairGD effectively achieves fairness with minimal modifications to the transition matrix, showcasing its strengths relative to LFPR.

### Reply to reviewer uQRE

#### Weaknesses 1
While we did not explicitly study the intra-group individual fairness, our framework can accomodate it. For instance, if we aim to assign the same PageRank score to all nodes within a group, we can define an individual target vector $\phi_I$, where $\phi_I[j]$ represents the target PageRank score for node $j$. We can modify Equation (3.1) into
$L(P,\gamma, v) = \frac{1}{K} \sum_{k=1}^{K} (1_k^Tp  - \phi_k) + \sum_{j=1}^{b}p[j]-\phi_I[j]$, where $p$ is the PageRank vector correcponding to parameters $(P,\gamma, v)$. 
This formulation incorporates individual fairness as an additional term in the fairness loss function, and enables the study of both group and individual fairness within the same framework.
We believe that this is a nice idea and we will investigate it further in our followup work.

#### Weaknesses 2
Please refer our answer to **cons** to reviewer 8pxn.

#### Q1

Although we did not address this scenario in the draft, our framework is capable of handling such cases. Suppose the nested grouping structure can be represented as a tree, where the root node denotes all vertices belonging to a single group, and the leaves indicate each vertex as an individual group. Each layer of the tree represents a different grouping of vertices.

To account for this nested grouping structure, we can compute a loss function for each layer (i.e., each grouping of vertices) based on the definitions in Equations (3.1) and (3.4). The problem can then be approached by minimizing the sum of the loss functions across all layers.

### Reply to reviewer Xfax

Thank you for your questions; we will clarify the time complexity in the next version.

Q2: Our problem constraint involves the intersection of two convex regions. Specifically, the constraint $(1-\delta)P[i,j] -\epsilon \leq \hat{P} \leq (1+\delta)P[i,j] + \epsilon$ defines a box, and the constraint $\sum_{j\in [n] }P[i,j] = 1$ defines a simplex. Lines 3–9 of Algorithm 3 implement the alternating projection method [1].

In this method, Lines 5–6 project a candidate solution onto the box, specifically, it implements

$$
P_{\text{box}}(\mathbf{x})_i =
\begin{cases}
l_i & \text{if } x_i < l_i, \\
x_i & \text{if } l_i \leq x_i \leq u_i, \\
u_i & \text{if } x_i > u_i.
\end{cases}
$$

and Lines 7–8 employ the widely applied normalizarion technique to map the projected point in the box to a feasible solution
<!-- , which should not hurt the convergence. -->


<!-- 
and Lines 7–8 project the solution from the box onto the simplex. The alternating projections, iteratively applied within the while-loop, ensure convergence to a feasible point that is closest to the input solution. This iterative approach is well-suited for scenarios where the feasible region is the intersection of multiple convex sets. -->

Q1: With a sufficiently large $t$, the approximated gradient $y_k$ will become tight and eventually achieves machine precision. When the gradient is exact, the smoothness of the objective function guarantees convergence to a local minimum. Furthermore, the feasibility of the updated transition matrix ensures that the KKT conditions are satisfied.

<!-- Q1: The smoothness of our objective function ensures that the projected gradient descent method converges to a local optimum and satisfies the Karush-Kuhn-Tucker (KKT) conditions. However, this guarantee relies on the use of the exact gradient, which requires a computationally expensive matrix inversion. 
Given that in implementation, we calculate the PageRank score with the same power iteration we use to approximate the gradient, the gradient is indeed exact, instead of approximated.  -->

<!-- To mitigate this, we employ the well established power iteration method to approximate the gradient, this approximation significantly reduces computational cost and in the meantime maintains strong practical performance. -->

Q3: The primary computational load of the FairGD algorithm lies in line 8, where matrix multiplication is performed for $t$ times, As a result, each gradient descent step has a time complexity of $O(n^2t)$.

Q4: Indeed, the constraint $C(R)$ allows edge deletion, which is why we also propose an alternative constraint, $C_R(P)$. By setting small values for $\delta$ and $\epsilon$, we can prevent edge deletions from the graph, and ensure its structure remains intact.

Q5: Indeed, the loss function could be formulated to minimize the maximum loss among all groups, this can be a very interesting future work to explore!