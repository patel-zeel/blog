---
aliases:
- /ML/2022/01/20/kl-divergence
categories:
- ML
date: '2022-01-20'
description: Understanding KL divergence
layout: post
title: KL divergence v/s cross-entropy
toc: true

---

## Ground

In a classification problem, for a data-point $\mathbf{x}_i$, we have the true label $y_i$ associated with it. 

Let us assume that we have three possible outcomes $\{L1, L2, L3\}$ and for current $\mathbf{x}_i$, corresponding $y_i$ is $L2$. Then **G**round truth probability distribution is the following:

$$
p_G(y = L1) = 0\\
p_G(y = L2) = 1\\
p_G(y=L3) = 0
$$

Let us assume that our classifier model **P**redicted the following distribution:

$$
p_P(y = L1) = 0.1\\
p_P(y = L2) = 0.8\\
p_P(y=L3) = 0.1
$$

## KL divergence

We can use KL divergence to check how good is our model. The formula is:

$$
D_{KL}(p_G\;\rVert\;p_P) = \sum_{y_i \in \{L1, L2, L3\}} p_G(y_i)\log\frac{p_G(y_i)}{p_P(y_i)} 
$$

For our example,

$$
D_{KL}(p_G\;\rVert\;p_P) = \log\frac{1}{0.8}
$$

It is evident that if $p_P(y = L2)$ decreses from $0.8$, $D_{KL}(p_G\;\rVert\;p_P)$ will increase and vice versa. Note that KL divergence is not symmetric which means $D_{KL}(p_G\;\rVert\;p_P) \ne D_{KL}(p_P\;\rVert\;p_G)$.

## Cross-entory

Cross-entropy is another measure for distribution similarity. The formula is:

$$
H(p_G, p_P) = \sum_{y_i \in \{L1, L2, L3\}} - p_G(y_i)\log p_P(y_i)
$$

For our example:

$$
H(p_G, p_P) = -\log 0.8 = \log \frac{1}{0.8}
$$


## KL divergence v/s cross-entropy

This shows that KL divergence and cross-entropy will return the same values for a simple classification problem. Then why do we use cross-entropy as a loss function and not KL divergence?

That's because KL divergence will compute additional constant terms (zero here) that are not adding any value in minimization. 