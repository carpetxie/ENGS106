# Writeup | Jeffrey Xie
---

## Task 1: Binary SVM with a Nonlinear Kernel

### Kernel Choice

I implemented a degree-2 polynomial kernel:

```
K(x, y) = (x · y / n_features + 1)²
```
I chose this over RBF because it was more computationally efficient. Kernel matrix is just a simple `X @ X.T` while RBF requires computing pairswise squared Euclidean distances. 

I divided by `n_features = 784` to normalize the dot products into the range [0, 1] before applying the polynomial nonlinearity, otherwise the kernel values will be unreasonably high. 

### Handling Non-Overlapping (Non-Separable) Distributions

I used soft-margins to handle non-separability. I used a slack variable to allow points to violate the hard margin. 

---

## Task 2: Predictive Model

### Bias Term

After solving the Quadratic Progrmaming(QP), the bias `b` is recovered from the KKT complementarity conditions. For any support vector `xₙ` that lies exactly on the margin (`0 < αₙ < C`).

I average this expression over all such "free" support vectors for numerical stability.

If no free support vectors exist (all bounded at `αᵢ = C`), I fall back to averaging over all support vectors.

---

## Task 3: One-vs-Rest vs. One-vs-One

### One-vs-Rest (OVR)

Ten binary SVMs are trained, one per class. Class `c` is labeled +1 and all other classes are
labeled -1. At prediction time, all 10 decision scores are computed and the class with the
highest score is chosen. 

### One-vs-One (OVO)

`C(10, 2) = 45` binary SVMs are trained, one per pair of classes. Each classifier sees only
the samples from its two classes (roughly 360 samples each). At prediction time, each of the
45 classifiers casts a vote for its winning class; the class with the most votes wins.



I do want to note the class imbalce:
OVR suffers from a ~1:9 positive-to-negative imbalance in each binary problem. The decision
boundary may be skewed toward predicting the majority class (-1). OVO avoids this entirely
since each binary problem contains only the two classes of interest, which are roughly balanced.

---

## Task 4: Hyperparameter Tuning for C

### Search Strategy

Running the full 1800-sample QP multiple times for different C values would be slow. So instead:

1. A random 400-sample stratified subset of the training data was drawn (each class
   proportionally represented).
2. Split 75/25 into a mini-train (300 samples) and mini-validation (100 samples).
3. OVR was trained and evaluated at **8 values of C** drawn from `np.logspace(-1, 2, 8)`:
   `[0.1, 0.215, 0.464, 1.0, 2.15, 4.64, 10.0, 21.5, ...]`
   C=0.1 and C=1 matters far more than between C=10 and C=11.
5. The best C from the subset search was used to train the final full model.


**Best C found:** `[13.895]`


---

## Task 5: Confusion Matrix Analysis

I notice that the shirt is the primary failure point. Only 7 of 19 Shirts classified correctly. The other 12 scatter across nearly every upper-body category. This makes sense as "Shirt" in Fashion MNIST is an inherently ambiguous label that visually overlaps with almost every other top.

Three classes are perfect: Trouser, Sandal, Bag. That also makes sense since their shapes are more distinguishable. I'd expect their classes to be more far apart. 

Footwear confusion is one-directional. Sneaker (7) gets confused with Sandal (5) four times and Ankle Boot (9) twice, but Sandal and Ankle Boot are
nearly perfect themselves. This means the model is reasonably confident about what a sandal/boot looks like, but some sneakers look ambiguously like
both.



---


