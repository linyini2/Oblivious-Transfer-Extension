# More Efficient Oblivious Transfer Extensions

#### **Protocol 4** Optimized semi-honest secure OT extension protocol

- **Input of $P_S$** : $m$ pairs $(x^0_j,x^1_j)$  of $n$-bit strings, $1 \leq j \leq m$.
- **Input of $P_R$** : $m$ selection bits $r=(r_1,...,r_m)$ .
- **Common Input:** Symmetric security parameter $\kappa$ and number of base-OTs $l=\kappa$ .
- **Oracles and cryptographic primitives:** The parties have an oracle access to the $l\times OT_\kappa$ functionality and use a pseudorandom generator $G:\{0,1\}^\kappa \rightarrow \{0,1\}^m$ and a correlation robust function $H:[m]\times\{0,1\}^l \rightarrow \{0,1\}^n$ .

1. **Initial OT Phase:**

   - $P_S$ initializes a random vector $s=(s_1,...,s_l) \in \{0,1\}^l$ and $P_R$  chooses $l$ pairs of seeds $k^0_i,k^1_i$ each of size $\kappa$ .
   - The parties invokes the $l \times OT_\kappa$-oracle, where $P_S$ acts as the receiver with input $s$ and $P_R$ acts as the sender with inputs $k^0_i,k^1_i$ for every $1 \leq j \leq l$ .

   For every $1 \leq j \leq l$ , let $t^i=G(k^0_i)$ . Let $T=[t^1|...|t^l]$ denote the $m \times l$ bit matrix where its $i$ th column is $t^i$ for $1 \leq j \leq l$ . Let $t_j$ denote the $j$ th row of $T$ for $1 \leq j \leq m$ .

2. **OT Extension Phase：**

   - $P_R$ computes $t^i=G(k^0_i)$ and $u^i=t^i \bigoplus G(k^1_i) \bigoplus r$ , and sends $u^i$ to $P_S$ for every $1 \leq j \leq l$ .

   - For every $1 \leq j \leq l$ , $P_S$ defines $q^i=(s_i\cdot u^i)\bigoplus G(k^{s_i}_i)$ . (Note that $q^i=(s_i\cdot r)\bigoplus t^i$)

   - Let $Q=[q^1|...|q^l]$ denote the $m \times l$ bit matrix where its $i$ th column is $q^i$ . Let $q_j$ denote the $j$ th row of the matrix $Q$ . (Note that  $q^i=(s_i\cdot r)\bigoplus t^i$ and $q_j=(r_j\cdot s)\bigoplus t_j$)

   - $P_S$ sends $(y^0_j,y^1_j)$ for every $1 \leq j \leq m$​​ , where
     $$
     y^0_j=x^0_j\bigoplus H(j,q_j)
     y^1_j=x^1_j\bigoplus H(j,q_j\bigoplus s)For
     $$
     

   - For $1 \leq j \leq m$ , $P_R$ computes $x_j=y^{r_j}_j\bigoplus H(j,t_j)$

3. **Output:** $P_R$ outputs $(x_1,...,x_m)$ ; $P_S$ has no outputs.

   

