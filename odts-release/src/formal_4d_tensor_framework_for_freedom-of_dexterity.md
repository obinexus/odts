# Formal 4D Tensor Framework for Freedom of Dexterity (FoD)

## Mathematical Definition

### 4D Configuration Space Tensor

Let's define the Freedom of Dexterity tensor **𝓕** as a 4th-order tensor in configuration space:

```
𝓕 ∈ ℝ^(6×6×n×τ)
```

Where:
- First dimension (6): Roll, Pitch, Yaw, X, Y, Z
- Second dimension (6): Velocity components (ẋ, ẏ, ż, θ̇ₓ, θ̇ᵧ, θ̇ᵤ)
- Third dimension (n): Joint/node index
- Fourth dimension (τ): Time evolution

### Formal Tensor Representation

```
𝓕ᵢⱼₖₜ = ∫ φ(qᵢ, q̇ⱼ, nₖ, t) · Γ(G_H, G_E) dΩ
```

Where:
- **φ**: Configuration field function
- **Γ(G_H, G_E)**: Graph constraint function combining Hamiltonian and Eulerian properties
- **Ω**: Reachable configuration manifold

### Hamiltonian-Eulerian Graph Constraints

For a graph **G = (V, E)** with joints as vertices and motion constraints as edges:

```
Γ(G_H, G_E) = {
    1 if path satisfies Hamiltonian cycle AND Eulerian path conditions
    exp(-λ·violation) otherwise
}
```

## Dimension Reduction Framework

### Principal Component Decomposition

Apply Tucker decomposition to the 4D tensor:

```
𝓕 = 𝓖 ×₁ U⁽¹⁾ ×₂ U⁽²⁾ ×₃ U⁽³⁾ ×₄ U⁽⁴⁾
```

Where:
- **𝓖**: Core tensor (reduced dimensions)
- **U⁽ⁱ⁾**: Factor matrices for each mode

### Reduced FoD Operator

The dimension-reduced FoD score becomes:

```
FoD_reduced = ||𝓖||_F · ∏ᵢ rank(U⁽ⁱ⁾) / dim_max
```

### Motion Subspace Extraction

For practical control, we extract the dominant motion subspace:

```
M_dominant = SVD(flatten(𝓕)) → top k singular vectors
```

Where k is chosen such that:
```
Σᵢ₌₁ᵏ σᵢ / Σᵢ₌₁ⁿ σᵢ ≥ 0.95 (95% variance preserved)
```

## Formal FoD Metric Definition

### Complete Freedom of Dexterity Score

```
FoD = ∫∫∫∫ 𝓕ᵢⱼₖₜ · W_safety · H_energy dq dq̇ dn dt
```

Normalized to [0,6] scale:

```
FoD_score = 6 · sigmoid(α · (rank_eff(J) - β·H_norm))
```

Where:
- **rank_eff(J)**: Effective rank of Jacobian with threshold ε
- **H_norm**: Normalized Hamiltonian energy
- **α, β**: Tuning parameters

### Constraint Manifold Projection

The actual reachable space considering all constraints:

```
Ω_reachable = {q ∈ ℝ⁶ | C_joint(q) ∧ C_energy(q) ∧ C_graph(q)}
```

Where:
- **C_joint**: Joint limit constraints
- **C_energy**: Energy budget constraints
- **C_graph**: Graph topology constraints (Eulerian/Hamiltonian)

## Computational Algorithm

```python
def compute_4d_fod_tensor(state, graph, time_window):
    # Initialize 4D tensor
    F = np.zeros((6, 6, n_joints, n_timesteps))
    
    # Fill tensor with configuration reachability
    for t in range(n_timesteps):
        for joint in range(n_joints):
            # Compute reachable configurations
            q_reach = compute_reachable_config(state[t], joint, graph)
            
            # Apply Hamiltonian-Eulerian constraints
            q_valid = apply_graph_constraints(q_reach, graph)
            
            # Fill tensor slice
            F[:,:,joint,t] = configuration_density(q_valid)
    
    # Apply Tucker decomposition for dimension reduction
    core, factors = tucker_decomposition(F, ranks=[3,3,n_joints//2,10])
    
    # Compute reduced FoD score
    fod_score = compute_fod_from_core(core, factors)
    
    return fod_score, core, factors
```

## Safety Integration

### Real-time FoD Monitoring

```
FoD_realtime(t) = α·FoD_current + (1-α)·FoD_predicted(t+Δt)
```

With safety thresholds:
- **FoD < 2.0**: Emergency constraint mode
- **FoD ∈ [2.0, 4.0]**: Assisted mode with partial constraints
- **FoD > 4.0**: Full freedom mode

This formal 4D tensor framework provides:
1. Mathematical rigor for motion analysis
2. Dimension reduction for real-time computation
3. Direct integration with your Hamiltonian-Eulerian graph model
4. Safety-conscious design for exoskeleton control
