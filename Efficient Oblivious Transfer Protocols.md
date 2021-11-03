# Efficient Oblivious Transfer Protocols  

#### Protocol 2.1 (Oblivious Transfer Using A Random Oracle)

**Input:** The chooser's input is ==$\sigma\in \{0,1\}$== , and the sender's input is two strings ==$M_0,M_1$== 

**Output:** The chooser's output is ==$M_\sigma$== .

**Preliminaries:** The protocol operates over a group $Z_q$ of a prime order. More specifically, $G_q$ can be a subgroup of order q of $Z^*_p$ , where $p$ is prime and $q|p-1$ . Let $g$ be a generator group, for which the computational Diffie-Hellman assumption holds. The protocol uses a function $H$ which is assumed to be a random oracle.

**Initialization:** The sender chooses a random element $C\in Z_q$ and publishes it. (It is only important that the chooser will not know the discrete logarithm of $C$ to the base $g$ . It is irrelevant whether the sender knows the discrete log of $C$ .)

**Protocol:**

- [ ] The chooser picks a random $1 \leq k \leq q$ , sets public keys ==$PK_\sigma=g^k$== and ==$PK_{1-\sigma}=C/PK_\sigma$== , and sends $PK_0$ to the sender.
- [ ] The sender computes $PK_1=C/PK_0$ and chooses random $r_0,r_1 \in Z_q$ . It encrypts $M_0$ by $E_0=(g^{r_0},H(PK^{r_0}_0 \bigoplus M_0))$ , and encrypts $M_1$ by $E_1=(g^{r_1},H(PK^{r_1}_1 \bigoplus M_1))$ , and sends the encryptions $E_0,E_1$ to the chooser.
- [ ] The chooser computes $H((g^{r_\sigma})^k)=H(PK^{r_\sigma}_\sigma)$ and uses it to decrypt $M_\sigma$ 

------

#### A single 1-out-of-2 oblivious transfer

Consider protocol 2.1 with the modification that the sender uses the same random value $r$ for both encryptions. Namely, $r=r_0=r_1$ . Since it holds that $PK_0=PK_1=C$ , it also holds that $(PK_0)^r=(PK_1)^r=C^r$ . Therefore, the operation of the sender can be defined as follows:

- [ ] **Initialization:** Choose a random $r$ , computer $C^r$ and $g^r$ .
- [ ] **Transfer:** After receiving $PK_0$ , compute $(PK_0)^r$ and $(PK_1)^r=C^r/(PK_0)^r$ . Send $g^r$ and the two encryptions, $H((PK_0)^r,0)\bigoplus M_0$  and $H((PK_1)^r,1)\bigoplus M_1$​ to the chooser.

------

#### Protocol 3.1 Many Simultaneous 1-out-of-N OT's With the Same $g^r$ 

**Initialization:** The sender chooses $N-1$ random constants $C_1,C_2,...,C_{N-1}$  (it will hold that $PK_0 \cdot PK_i=C_i$). It also chooses a random $r$ and computes $g^r$ . The values $C_1,C_2,...,C_{N-1}$ and $g^r$ are sent to the chooser and play the role of the public key of the sender. The same values will be used for all transfers.

The sender precomputes for every $1 \leq i \leq {N-1}$  the value $(C_i)^r$ .

**Transfer:** The sender's input is $M_0,M_1,...,M_{N-1}$ . The chooser's input is $\sigma \in \{0,...,N-1\}$ . She should learn $M_{\sigma}$ .

- [ ] The chooser selects a random $k$ and sets $PK_\sigma=g^k$ . If $\sigma \neq 0$ she computes $PK_0=C_\sigma/PK_\sigma$ . She sends $PK_0$ to the sender and can already computes a decryption key $(g^r)^k=(PK_\sigma)^r$ .

- [ ] The sender computes $(PK_0)^r$ and then for every $1 \leq i \leq N-1$​ computes (without doing any additional exponentiations)
  $$
  (PK_i)^r=(C_i)^r/(PK_0)^r
  $$
  

- [ ] The sender chooses a random string $R$ . He then encrypts each $M_i$ by computing $H((PK_i)^r,R,i)\bigoplus M_i$ , and sends these encryptions and $R$ to the chooser.

- [ ] The chooser uses $H((PK_\sigma)^r,R,\sigma)$ to decrypt $M_\sigma$ .

