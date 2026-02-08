# Write Up
*Jeffrey Xie*
*Feb. 8, 2026*

My individual write ups are in each part in the markdown of the `.ipynb` files. I will add additional comments here:

# Part A
This was quite standard. I followed the instructions. What I could have done better was mapping all combinations of features and determining the set of features that would optimize the loss. 

# Part B
This took the longest out of the three. There are more degrees of freedom to this. For one, the `README.pdf` instructed to keep the categorical data as strings, using a difference of `1` when differing in l2 distance calculations. 

One consideration is perhaps not all categories are equal distance apart. Surely a table and a chair are closer together in some semantic subspace than an iguana. This may be too pedantic though. 

My results are not fantastic. It is interesting how the model for lens would have perfect accuracy on low k's, likely a byproduct of the smaller dataset. I believe there is a tradeoff: if k increases, you are able to capture more variance but your bias decreases. Vice versa. 

Nonetheless, the effect k would likely be larger for larger datasets. 

# Part C
This part felt like a leetcode design question. I had to utilize many nested dimensions. I also became well acquainted with ternary functions and one liners. I felt my analysis in 2C covered most of what I wanted to say, but I do want to  re-emphasize:
1. The precision was particularly low. For industry applications, different metrics matter more. It could be catastrophic if an important email was wrongly classified as spam. 
2. Temporal changes. Demand changes and spam changes. To improve this, we could use an ever-evolving dataset. 

Moreover, I wonder how we can extend this spam detector to an AI detector. I hypothesize that for AI, we cannot use the Naive Bias since the connection of words is likely more indicative than presence of words for AI. 