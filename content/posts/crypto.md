---
draft: false
---

```elisp
(face-remap-add-relative 'default :family "Noto Serif" :height 150)
```

```elisp
(face-remap-add-relative 'default :family "Roboto Serif" :height 150)
```


## Classic Ciphers {#classic-ciphers}


### Examples {#examples}


### Modern Crypto {#modern-crypto}


### Kerckhoffs Pronciple {#kerckhoffs-pronciple}


## Perfect Secrecy {#perfect-secrecy}


### Brief review of Probability {#brief-review-of-probability}


#### Classical Definition {#classical-definition}


#### Conditional Probability {#conditional-probability}


#### Mutually/ Pairwise independent {#mutually-pairwise-independent}


### Total Probability Formula {#total-probability-formula}


#### Bayes' Theorem {#bayes-theorem}

\\[Pr[X = x|Y = y] =\frac{Pr[X = x] \cdot Pr[Y = y|X = x]}{Pr[Y = y]} \\].


### Perfectly-Secret Encryption {#perfectly-secret-encryption}


#### Definition of a encryption scheme {#definition-of-a-encryption-scheme}

A encryption scheme \\(\Pi\\), also called a cipher or a cryptosystem, is defined by three algorithms Gen, Enc, and Dec, as well as a specification of a finite message space M with |M| &gt; 1.

**Gen**
: a probabilistic algorithm that outputs a key k according to some distribution from a finite key space K.


**Enc**
: an algorithm that takes as input a key \\(k \in K\\) and a message \\(m \in M\\), and outputs a ciphertext c: \\(c \leftarrow Enc\_{k}(m)\\) (probabilistic) OR \\(c := Enc\_{k}(m)\\)(deterministic).


**Dec**
: a deterministic algorithm that takes as input a ciphertext c from the set of all ciphertexts C and a key k ∈ K, and outputs a message \\(m \in M\\)

    \\[ m := Dec\_{k}( c).\\]


#### Definiton of Perfectly secrecy {#definiton-of-perfectly-secrecy}

-   An encryption scheme (Gen,Enc,Dec) over a massage space M is perfect secret if for every possible distribution over M,

\\[Pr[M = m|C = c] = Pr[M = m]\\]

holds for every m ∈ M and every c ∈ C that Pr[C = c] &gt; 0.

\\[Pr[M = m|C = c] = Pr[M = m]\\]
\\[ Pr[M = m|C = c] = \frac{Pr[M = m]Pr[C=c|M=m]}{Pr[C=c]} \\]
\\[ \Rightarrow Pr[C = c | M=m] = Pr[C = c]\\]

\\[Pr[M = m|C = c] = Pr[M = m]\\]
\\[ Pr[M = m|C = c] = \frac{Pr[C=c, M=m]}{Pr[C=c]}\\]
\\[ \Rightarrow Pr[C = c ,  M=m] = Pr[C = c] Pr[M=m]\\]

M=m and C=c are mutually independent.

-   An encryption scheme (Gen, Enc, Dec) over message space M is perfectly secret if and only if for every m, m′ ∈ M, and every c &isin; C:

\\[Pr[C=c|M=m] = Pr[C=c][M=m'] = Pr[C=c]\\]

-   Perfect adversary indistinguishability Encryption scheme \\(\Pi = (Gen, Enc, Dec)\\) with message space M is **perfectly indistinguishable** if for every A it holds that

\\[Pr[PrivK\_{\emph{A},\Pi}^{eav}] = \frac{1}{2}\\]

-   Encryption scheme \\(\Pi = (Gen, Enc, Dec)\\) with message space M is perfectly secret if and only if it is perfectly indistinguishable.


### The One-Time Pad {#the-one-time-pad}

1.  Find an integer \\(l > 0. M = \\{0, 1\\}^{l}, K = \\{0, 1\\}^{l}, C = \\{0, 1\\}^{{l}}\\).
2.  Gen: \\(K \leftarrow K, Pr[K = k] = 1/2l   k \in K.\\)
3.  Enc_K(M):  \\(C:= M \oplus K\\)
4.  Dec_K(C):  \\(M:= C \oplus K\\)

The One-Time Pad is a perfectly-secret encryption scheme.
**Prove that it is perfectly-secret**


### Limitations of Perfect Secrecy {#limitations-of-perfect-secrecy}

-   key should be as long as the message
-   One key can only be used once to be safe
-   only secure against ciphertext-only attack

Let (Gen,Enc,Dec) be a perfectly-secret encryption scheme over a
message space M, and let K be the key space as determined by Gen. Then |K| ≥ |M|.

Proof: Consider the uniform distribution over \\(\mathcal{M}\\) (as the input), we know there is a c ∈ C such that Pr[C = c] &gt; 0. According to the definition of perfect secrecy, we know for every \\(m \in \mathcal{M}\\),
\\[Pr[M=m|C=c] = Pr[M=m] = 1/|\mathcal{M}| > 0 \\]
which implies there is at least one key k for each m such that Deck(c) = m. Accordingly, there are at least |M| different keys in K, one for each different m ∈ M. Thus, we have |K| ≥ |M|.


### Shannon's Theorem {#shannon-s-theorem}

Let (Gen, Enc, Dec) be an encryption scheme over a message space \\(\mathcal{M}\\) for which |M| = |C| = |K|. This scheme is perfectly secret if and only if:

1.  Every key k ∈ K is chosen with equal probability 1/|K| by algorithm Gen.
2.  For every m ∈ M and every c ∈ C, there exists a single key k ∈ K such that Enck(m) outputs c.


## Computatinoal Security {#computatinoal-security}


### The Asymptotic Approach of Defining Computational Security {#the-asymptotic-approach-of-defining-computational-security}


#### Computational security {#computational-security}

Information-theoretically Secure (Perfectly Secure)
: Adversaries with unlimited computation capability do not have enough information to launch a successful attack, thus always fail.


Computationally Secure
: Efficient adversaries have the information, and can potentially succeed with some very small probability.


#### Efficient adversaries {#efficient-adversaries}

-   Efficient adversaries = Randomized algorithms + Polynomial-time

bounded = **Probabilistic Polynomial-Time** (bounded) algorithms = PPT algorithms

Randomized algorithm
: currently accepted as feasible and powerful computations by practical computers.

Polynomial-time bounded
: Given the security parameter n, the algorithm runs no more than poly(n) steps, where poly(n) is a polynomial of n, i.e. can be represented as

\\[ poly(n) =  a\_{k}n^{k} + a\_{k-1}n^{k-1} +  + a\_{1}n + a\_{0}\\]


#### Negligible success probabilities {#negligible-success-probabilities}

-   A function superpoly(·) is superpolynomial if.f. for every constant c, it holds that

\\[superpoly(n) > n^{c}\\]
where n is sufficiently large

-   A function \\(negl(\cdot)\\) from the natural numbers to the non negative real numbers is negligible if for every positive polynomial p there is an N such that for all integers n &gt; N it hods that

\\[negl(n) < \frac{1}{p(n)}\\]

or if for every positive integer c, there is an \\(N\_{c}\\) such that for all integers n &gt; N_c it holds that

\\[negl(n) < \frac{1}{n^{c}}\\]

example : \\(1/2^{n}, 1/n!\\)

Let negl1 and negl2 be negligible functions. Then,

1.  If there exists an integer Nc such that f(n) &lt; negl1(n) holds for all n ≥ Nc, f(n) is negligible.

2.  The function negl3(n) = negl1(n) + negl2(n) is also negligible.

3.  For any positive polynomial p, the function negl4 defined by negl4(n) = p(n) · negl1(n) is negligible.


### Computationally Secure Encryption {#computationally-secure-encryption}


#### Definition of private-key encryption schemes {#definition-of-private-key-encryption-schemes}

**with security parameter**
A private-key encryption scheme is a tuple of probabilistic
polynomial-time algorithms (Gen, Enc, Dec) such that:
1 The key-generation algorithm Gen: k ← Gen(1n).
2 The encryption algorithm Enc: c &larr; Enc_k(m), where the plaintext massage m ∈ {0, 1}∗.
3 The decryption algorithm Dec: \\(m := Dec\_{k}( c)\\).

If Enc is only defined for messages m ∈ {0, 1}l(n), then we say (Gen, Enc, Dec) is a fixed-length private-key encryption for messages of length l(n).
Almost always, \\(Gen(1^n): k \leftarrow  \\{0, 1\\}^n\\).


#### The indistinguishable encryption {#the-indistinguishable-encryption}

The adversarial indistinguishability experiment \\(PrivK\_{{\mathcal{A}, \Pi}}^{eav}(n)\\)

A private-key encryption scheme \\(\Pi = (Gen, Enc, Dec)\\) has indistinguishable encryptions in the presence of an eavesdropper or is EAV-secure, if for every PPT adversary \\(\mathcal{A}\\) there is a negligible function negl such that for all n,
\\[Pr[PrivK\_{\mathcal{A}, \Pi}^{eav}(n) = 1] \leq \frac{1}{2} + negl(n)\\]
where the probability is taken over the randomness used by A and the randomness used in the experiment.

Or,

\\[|Pr[out\_{\mathcal{A}}(PrivK\_{\mathcal{A, }\Pi}^{{eav}}(n,0))=1] - Pr[out\_{\mathcal{A}}(PrivK\_{\mathcal{A, }\Pi}^{{eav}}(n,1))=1]| \leq negl(n)\\]


### Constructing Computationally-secure Encryptions {#constructing-computationally-secure-encryptions}


#### Pseudo-random generators(PRG) {#pseudo-random-generators--prg}


##### What is PRG {#what-is-prg}

Let l be a polynomial and let G be a deterministic polynomial-time algorithm such that for any n and any input \\(s \in \\{0,1\\}^n\\), the output G(s) is a string of length l(n). G is a pseudo-random generator if the following conditions hold:

1.  (Expansion): For every n, l(n) &gt; n.
2.  Pseudo-randomness: For any PPT algorithm D, there is a negligible function negl such that

\\[|Pr[D(G(s)) = 1] - Pr[D( r) = 1] | \leq negl(n)\\]
where the first probability is taken over uniform choice of \\(s \in \\{0,1\\}^n\\)and the randomness of D, and the second probability is taken over uniform choice of r ∈ {0, 1}l(n) and the randomness of D.

We Call l the expansion factor of G.


##### PRG's features {#prg-s-features}

1.  PRG is deterministic
2.  PRG's distribution is not uniformly random
3.  The brutal-force attack can differentiate PRG with truly randomness, but requires O(2n) cycles/time.


#### A secure fixed-length encryption scheme {#a-secure-fixed-length-encryption-scheme}

We construct our first secure encryption scheme with PRG:

Let G be a PRG with expansion factor l. Define a private-key encryption scheme for messages of length l as follows:

-   Gen: on input \\(1^{n}\\), choose uniform \\(k \in \\{0, 1\\}^n\\) and output it as the key.
-   Enc: on input a key \\(k \in \\{0, 1\\}^n\\) and a message \\(m \in {0, 1}^{l(n)}\\), output the ciphertext

\\[c:= G(k) \oplus m\\]

-   Dec: on input a key \\(k \in \\{0, 1\\}^n\\) and a ciphertext c &isin; {0, 1}<sup>l(n)</sup>, output the message/plaintext

\\[m := G(k) \oplus c\\]

This is a fixed-length private-key encryption scheme that has indistinguishable encryptions in the presence of an eavesdropper.

Prove. (?)


## Stream Cipher {#stream-cipher}


### Stream Ciphers and Pseudo-random Generators {#stream-ciphers-and-pseudo-random-generators}


#### Stream Cihpers {#stream-cihpers}

**stream cipher**:: is a pair of deterministic algorithms

-   Init: \\(st\_{0}:= Init(s, [IV])\\)
    -   s: seed
    -   IV: initialization vector(optional)
    -   st<sub>x</sub> : state x
-   GetBits: \\((y\_{i}, st\_{i}):= GetBits(st\_{i-1}\ for\ i = 1,2,...)\\)
-   keystream: \\(y\_{1}, y\_{2}, y\_{3}, ...\\)
    -   y is often a block of bits


### Encrypt with stream ciphers {#encrypt-with-stream-ciphers}


#### Basics {#basics}

Encryption: XOR(plaintext, keystream)

Pros
: efficient. arbitrary length

Cons
: key/keystream one-time useage, vulnerable to bit-flipping attacks


#### Modes of operation {#modes-of-operation}

-   Synchronized mode
-   Unsynchronized mode


### Linear-Feedback Shift Registers {#linear-feedback-shift-registers}


#### LFSR {#lfsr}

Shift: \\(S\_{i}^{(t+1)}:= S\_{i+1}^{(t)}\\), \\(i \in [0, n-2] \cap Z\\)
linear-feedback: \\(S\_{n-1}^{(t+1)}:=\oplus\_{i=0}^{n-1} c\_{i}S\_{i}^{(t)} \\)
start to repeat at most \\(2^{n}\\)
not secure for crypto


#### Reconstruction attack on LFSR {#reconstruction-attack-on-lfsr}

2n output bits makes adversary reconstruct LFSR


### Adding Nonlinearity (to LFSR) {#adding-nonlinearity--to-lfsr}


#### RC4 {#rc4}

should no longer be used


##### Init algorithm {#init-algorithm}

INPUT: k(16bit)
OUTPUT: Initial state(S, i, j)

```python
for i in range(255):
  S[i] = i
  T[i] = k[i % 16]
j = 0
for i in range(255):
  j = j + S[i] + T[i]
  swap(S[i], S[j])
return (S, 0, 0)
```


##### GetBits algorithm {#getbits-algorithm}

INPUT: Current state (S, i, j)
OUTPUT: Output byte y; updated state (S, i, j)

```python
i = i + 1
j = j + S[i]
swap(S[i], S[j])
t = S[i] + S[j]
y = S[t]
return (S, i, j), y
```


#### Attack RC4-based WEP {#attack-rc4-based-wep}


### Two Tips on stream cipher usages {#two-tips-on-stream-cipher-usages}

-   Stop using unsafe stream ciphers.
-   block ciphers are generally preferable to stream ciphers


## CPA Security and Pseudorandom Functions {#cpa-security-and-pseudorandom-functions}


### Need for stronger Security {#need-for-stronger-security}


#### The indistinguishable multiple encryptions {#the-indistinguishable-multiple-encryptions}


##### Single encryption vs multiple encryption {#single-encryption-vs-multiple-encryption}

pass


##### The multiple-message eavesdroppping experiment {#the-multiple-message-eavesdroppping-experiment}

1.  Given the security parameter n, A outputs a pair of equal-length lists of messages

M0 = (m0,1, . . . , m0,t) and M1 = (m1,1, . . . , m1,t), with |m0,i| = |m1,i| for all i, and sends them to the challenger C.

1.  C computes a key k by running Gen(1n), a uniform bit b ∈ {0, 1}, ci ← Enck(mb,i) for all i, and sends ⃗C = (c1, . . . , ct) to A.
2.  A outputs a bit b′.
3.  The output of the experiment is defined to be 1 if b′ = b, and 0 otherwise.


##### The indistinguishable multiple encryptions {#the-indistinguishable-multiple-encryptions}

 A private-key encryption scheme \\(\Pi = (Gen, Enc, Dec)\\) has indistinguishable multiple encryptions in the presence of an eavesdropper if for all PPT adversaries \\(\mathcal{A}\\) there is a negligible function negl such that
 \\[Pr[PrivK\_{\mathcal{A}, \Pi}^{mult}(n) = 1] \leq \frac{1}{2} + negl(n)\\]
where the probability is taken over the randomness used by \\(\mathcal{A}\\) and the randomness used in the experiment.


#### CPA-security {#cpa-security}


##### The CPA indistinguishability experiment \\(PrivK\_{\mathcal{A}, \Pi}^{cpa}(n)\\) {#the-cpa-indistinguishability-experiment-privk-mathcal-a-pi-cpa--n}

1.  A key k is generated by running \\(Gen(1^n)\\).
2.  The adversary \\(\mathcal{A}\\) is given input \\(1^n\\) and oracle access to \\(Enc\_k(\cdot)\\), and outputs a pair of messages \\(m\_0\\), \\(m\_1\\) of the same length.
3.  A uniform bit b ∈ {0, 1} is chosen, and then a ciphertext ←\\(c \leftarrow Enc\_k(m\_{b})\\) is computed and given to \\(\mathcal{A}\\).
4.  The adversary \\(\mathcal{A}\\) continues to have oracle access to\\(Enc\_k(\cdot)\\), and outputs a bit b′.
5.  The output of the experiment is defined to be 1 if b′ = b, and 0 otherwise. In the former case, we say that A succeeds.


### Pseudorandom Functions {#pseudorandom-functions}


#### Pseudorandom Functions {#pseudorandom-functions}

-   A function is a mapping from \\(\\{0, 1\\}^{l\_{in}}\\) to \\(\\{0, 1\\}^{l\_{out}}\\). If lin = lout, we say the function is length-preserving.
-   Let \\(Func\_{n}\\) denote the set of all functions from \\(\\{0, 1\\}^{n}\\) to \\(\\{0, 1\\}^{n}\\).
-   A (uniformly) random function is a function that is chosen uniformly at random from \\(Func\_{n}\\).

-   A keyed function \\(F : \\{0, 1\\}^{l\_{key}} \times \\{0, 1\\}^{l\_{in}} →\rightarrow \\{0, 1\\}^{l\_{out}} \\) is a two-input function, where the first input is called the `a key` and denoted `k`. We only consider F is length-preserving, meaning lkey(n) = lin(n) = lout(n) = n.
-   If there is a polynomial-time algorithm that computes F(k, x) given k and x, we say F is efficient.
-   In typical usage, a key k is chosen and fixed, and we are interested in the single-input function Fk : {0, 1}∗ → {0, 1}∗ denoted by

\\[F\_{k}(x) = F(k,x)\\]

-   If we choose k uniformly at random, the keyed function F induces a natural distribution on Funcn.
-   If the function \\(F\_{k}\\) (for a uniformly random key k) is indistinguishable from a (uniformly) random function, we say \\(F\_{k}\\) is pseudorandom.


#### Formal definition of pseudorandom function {#formal-definition-of-pseudorandom-function}

Consider a length-preserving keyed function F : {0, 1}lkey(n) × {0, 1}lin(n) → {0, 1}lout(n) (i.e. lkey(n)=lin(n)=lout(n)=n), and f is a random function that is uniformly chosen from Funcn. F is a pseudorandom function if for all PPT distinguishers D, there is a negligible function negl such that,
\\[|Pr[D^{F\_{k}(\cdot)} (1^{n})= 1] - Pr[D^{f(\cdot)}(1^{n})| = 1] \leq negl(n)\\]
where Funcn is the set of all functions mapping n-bit string to n-bit string, the first probability is taken over uniform choice of k ∈ {0, 1}n and the randomness of D, and the second probability is taken over uniform choice of f ∈ Funcn and the randomness of D.


### Constructing CPA-secure encryption with PRFs {#constructing-cpa-secure-encryption-with-prfs}


#### Constructing CPA-secure encryption with PRFs {#constructing-cpa-secure-encryption-with-prfs}


### The existence of PRFs {#the-existence-of-prfs}


#### Pseudorandom permutations {#pseudorandom-permutations}

-   A permutation (function) is a bijection or a one-to-one mapping from \\(\\{0,1\\}^n\\) to \\(\\{0,1\\}^{n}\\)
-   Let \\(Perm\_n\\) be the set of all permutations on \\(\\{0,1\\}^n\\).
-   \\(|Perm\_n|= (2^n)!\\)
-   A random permutation on \\(\\{0,1\\}^n\\) f is a permutation that is chosen uniformly at random from \\(Perm\_n\\).
-   Let F be a keyed function. If lin = lout, and for all k the function Fk : {0, 1}lin → {0, 1}lout is one-to-one, we call F a keyed permutation.
-   We call \\(l\_{in}\\) the block length of F.
-   If both \\(F\_k(x)\\) and its inverse function \\(F\_k^{-1}(y)\\) can be computed within polynomial time given k, x and k, y resp., we say F is `efficient`.
-   If NO efficient algorithm can distinguish between a Fk (for uniform key k) and a random permutation, i.e. a function that is chosen uniformly at random from Permn, we say Fk is a pseudorandom permutation.


#### Pseudorandom permutations and PRFs {#pseudorandom-permutations-and-prfs}


#### PRFs and block ciphers {#prfs-and-block-ciphers}


#### PRF and PRG {#prf-and-prg}

-   construct a PRG G from a PRF F for any desired l

\\[G(s) = F\_{s}(1) || F\_s(2) || ... || F\_s(l)\\]

-   Also, a PRG G with expansion factor \\(n \cdot 2^{t(n)}\\) can be used to construct a PRF with small input length \\(F: \\{0,1\\}^n \times \\{0,1\\}^{t(n)} \rightarrow \\{0,1\\}^n \\)by setting

\\[F\_k(x) = y\_{x\cdot n} y\_{x\cdot n +1} ... y\_{x\cdot n + n - 1}\\]

where y0, y1, . . . , are the bits generated by G(k), and t(n) = O(log n).


## Block Cipher {#block-cipher}


## Message Authentication {#message-authentication}


### The Need for Message Integrity {#the-need-for-message-integrity}

For message integrity authentication, encryption DOES NOT directly solve our problem.


### Message Authentication Code(MAC) {#message-authentication-code--mac}


#### Definition of MAC {#definition-of-mac}

A message authentication code (or MAC) \\(\Pi\\) consists of three probabilistic polynomial-time algorithms (Gen, Mac, Vrfy):

1.  The **key-generation** algorithm `Gen` takes as input the security parameter 1n and outputs a key k with |k| ≥ n.

2.  The **tag-generation** algorithm `Mac` takes as input a key k and a message m ∈ {0, 1}∗, and outputs a tag t:

\\[t \leftarrow Mac\_{k}(m)\text{ or } t := Mac\_{k}(m)\\]

1.  The deterministic **verification algorithm** Vrfy takes as input a key k and a message m and a tag t, it outputs a bit b, with b = 1 meaning valid and b = 0 meaning invalid:

\\[b:=Vrfy\_{k}(m, t)\\]


#### Correctness and security requirements of MAC {#correctness-and-security-requirements-of-mac}


##### Correctness of MAC {#correctness-of-mac}

Correctness
: it is required that for every n, every key k output by Gen(1n), and every m ∈ {0, 1}∗, it holds that

\\[Vrfy\_{k}(m,Mac\_{k}(m))=1\\]

-   When Mac is deterministic, a canonical way to perform verification is to simply recompute the tag and check for equality.


##### The message authentication experiment \\(Mac-forge\_{\mathcal{A}, \Pi}\\) {#the-message-authentication-experiment-mac-forge-mathcal-a-pi}

1.  A key k is generated by running \\(Gen(1^{n})\\) and kept secret from the adversary.
2.  The adversary A is given input 1n and oracle access to Mack(·). The adversary eventually outputs (m, t). Let Q denote the set of queries that A asked its oracle.


#### How to use MAC {#how-to-use-mac}


### Constructing Secure MACs {#constructing-secure-macs}


#### Constructing a fixed-length MAC {#constructing-a-fixed-length-mac}


#### CBC-MAC for handling long messages {#cbc-mac-for-handling-long-messages}


### Authenticated Encryption {#authenticated-encryption}


#### Definition of AE {#definition-of-ae}


#### CCA security {#cca-security}


#### Unforageable encryption {#unforageable-encryption}


#### Constructing AE {#constructing-ae}
