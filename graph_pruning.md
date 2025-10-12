
**Definition 1: Graph Structure**
Let $G = (V, E)$ be a graph where:

* $V = \{v_1, v_2, ..., v_n\}$ is the set of nodes (vertices)
* $E \subseteq V \times V$ is the set of edges

**Definition 2: Node/Edge Weight Function**
Define a weight function $w: E \cup V \to \mathbb{R}_{\ge 0}$ representing the “importance” or contribution of each node/edge.

**Definition 3: Pruning Threshold**
Let $\tau \in \mathbb{R}_{\ge 0}$ be a threshold below which a node or edge is considered insignificant.

---

### **Pruning Operation**

**Node Pruning:**

$$
V' = \{ v \in V \mid w(v) \ge \tau \}
$$

**Edge Pruning:**

$$
E' = \{ e = (u,v) \in E \mid w(e) \ge \tau \text{ and } u, v \in V' \}
$$

**Pruned Graph:**

$$
G' = (V', E')
$$

---

### **Optional: Cluster-Preserving Pruning**

If the goal is to preserve cluster structure:

1. Compute a cluster function $C: V \to \{1,2,...,k\}$ assigning nodes to clusters.
2. Only prune edges **between clusters** below threshold:

$$
E' = \{ (u,v) \in E \mid w(u,v) \ge \tau \text{ or } C(u) = C(v) \}
$$

This ensures intra-cluster connectivity is preserved while reducing noise.

---

### **Notes**

* This is entirely deterministic and mathematically grounded.
* The weight function $w$ can be any measurable quantity: degree centrality, edge betweenness, connection strength, or even derived from physical constraints in a network.
* Threshold $\tau$ can be tuned based on desired sparsity.

