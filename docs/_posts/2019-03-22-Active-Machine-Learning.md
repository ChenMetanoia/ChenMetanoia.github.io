---
title: "Introduction of Active Machine Learning"

categories:
  - Machine Learning
tags:
  - machine learning
  - transparency
  - interaction
---

## 1. Introduce the Main Idea of Transparency and Interaction in Machine Learning

### Transparency

Transparency aims to understand what did the model learn, what do the weights tell us about features and their importance, when an object is
classified by this model, which feature values contributed to each class and
how much.

### Interaction

If we want to label more objects, which one should we label next? Can we
provide any rationales into the learning process?

### Why Transparency?

A perfect example comes from my [professor Mustafa](http://www.cs.iit.edu/~mbilgic){:target="blank"}. He said that sometimes the machine instead provides a suggestion and gives a reason than deciding for the human. In medical recommender system, the doctor prefers the system provides suspected symptoms to the percentage of the diagnosis. If a cancer detection system only gives "This patient has a 79% chance gets cancer". Such a result will make confuse the doctor and the patient. In contrast, if this system said: "This patient has a 79% chance gets cancer because I detected the patient has high compression in the spinal cord and pleural effusion". This diagonal report will be of great help to doctors and dramatically reduce diagnostic time. Moreover, transparency also used in [Trustworthy fraud detection](http://www.cse.lehigh.edu/~sxie/projects.html){target:blank}. We should tell the customer this product evaluation is fake and why at the same time.

## 2. Concept Learning

### Motivation

Different classification task has different purposes such as emails labeled as **spam/~spam**, patient records labeled as **flu/~flu** or reviews labeled as **positive/negative**.

Our goal is inducing a general function that **fits the training data well and generalized well to unseen or future data**. 

### General-to-Specific Ordering of Hypothese

#### Example

Suppose we have two features: **Weight: (Light, Heavy)** and **Color: (Red, Green, Blue**. The class is **YES** and **NO**. Therefore, we have six objects/instances $$ 2\times3=6 $$. and we have 64 hypotheses/functions $$ 2^6=64 $$.

| **Weight** | **Color** | F1 | ... | F64 |
|------------|-----------|----|-----|-----|
| Light      | <span style="color:red">Red</span>       | NO |     | YES |
| Light      | <span style="color:green">Green</span>     | NO |     | YES |
| Light      | <span style="color:blue">Blue</span>      | NO |     | YES |
| Heavy		 | <span style="color:red">Red</span>       | NO |     | YES |
| Heavy		 | <span style="color:green">Green</span>     | NO |     | YES |
| Heavy      | <span style="color:blue">Blue</span>      | NO |     | YES |

If we know that <Light, Red> is 'Yes', can you guess how many hypothese left?
