# Writeup | Jeffrey Xie
---

## Paper Design

The energy function is given in the problem statement so the only design decision is picking `h`, `beta`, `eta`. `h = 0` because the image is roughly balanced between black and white. `beta` and `eta` need to be balanced -- if eta dominates we just keep the noisy image, if beta dominates we over-smooth and lose detail.

---

## Model / Algorithm Description

Coordinate descent as described in the assignment. Initialize `x = y`, then for each pixel compute the energy change from flipping: `delta_E = -2 * x_i * (h - beta * neighbor_sum - eta * y_i)`. Flip if delta_E < 0. Repeat until nothing flips.

I tried a few values by hand. Setting eta too high (like 3+) made the algorithm barely correct anything since it trusts the noisy observation too much. Setting beta too low made it converge in one pass without fixing much. `h = 0.0`, `beta = 1.0`, `eta = 1.0` gave the best results I found.

---

## Evaluation

Starting accuracy from the noisy image is ~90% (10% of pixels flipped). After coordinate descent with `beta=1.0, eta=1.0` the recovery hits ~94.4% and converges in about 10 iterations. This is below the 96% target. I think the ceiling might be partly due to the BW pre-procesing step. Converting the clean RGBA image to binary for comparison might not be a perfect ground truth. Visually the result is noticeably cleaner than the noisy input.

---

## Reflections

Compared to Lab 3A where I was fighting tensor shapes for hours this was more straightforward. The math translated to code pretty directly.
