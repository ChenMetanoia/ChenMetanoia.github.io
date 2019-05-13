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
|------------|:----------|----|-----|-----|
| Light      | <span style="color:red">Red</span>       | NO |     | YES |
| Light      | <span style="color:green">Green</span>   | NO |     | YES |
| Light      | <span style="color:blue">Blue</span>     | NO |     | YES |
| Heavy		 | <span style="color:red">Red</span>       | NO |     | YES |
| Heavy		 | <span style="color:green">Green</span>   | NO |     | YES |
| Heavy      | <span style="color:blue">Blue</span>     | NO |     | YES |

If we know that <Light, <span style="color:red">Red</span>> is 'Yes', can you guess how many hypothese left? The answer is $$ 2^5 \times 1 = 32 $$.

The number of hypotheses of <Light, <span style="color:blue">Blue</span> > is 'Yes,' or 'No' is 16.

For now, we know how to calculate the hypothesis. Let us assume two particular assumptions: 
1. 'No' to everything
2. 'Yes' to everything

* In particular:
  * $$<\phi,\phi>$$: 'No' to everything
  * $$<?,?>$$: 'Yes' to everything
  * <Light, <span style="color:red">Red</span>>: 'Yes' to <Light, <span style="color:red">Red</span>> and 'No' to everthing else
  * <Light, $$?$$>: 'Yes' to anything that is **Light**; ignore **Color**; everything else is 'No'
  * Combination of one or more of $$\phi$$ with other feature values is still equivalen $$<\phi,\phi>$$ to 'No' to everything. E.g., $$<\phi, $$<span style="color:red">Red</span>$$>$$ is 'No' to everything

* Most-general hypothesis is where $$h(x)=Yes\ \forall x \in X$$: $$<?,?>$$
* Most-specific hypothesis is where $$h(x)=No\ \forall x \in X$$: $$<\phi,\phi>$$
* These called [hypothesis space](https://en.wikipedia.org/wiki/Version_space_learning){:target="blank"}

But, how do we search this space efficiently?

### Algorithms

#### 1.Find-S

> Initialize $$h$$ to the most specific hypothesis in $$H$$
> For each positive training instance $$x$$
> * For each attribute constraint $$ a_{i} $$ in $$ h $$:
>   * If the constraint $$ a_{i} $$ in $$ h $$ is satisfied by $$ x $$ 
>   * Then do nothing 
>   * Else replace $$ a_{i} $$ in $$ h $$ by the next more general constraint that is satisfied by $$ x $$
> Output hypothesis $$ h $$

* Warning!!!:
In that case, Find-S always return the most specific one, but we do not know whether we need the most specific one or the most general one or interval between them. Moreover, what if our training hata has errors or what if the most specific hypothesis in not unique?

#### 2.List-Then-Eliminate

> VS $$\leftarrow$$ a list of containing every hypothesis in $$H$$
> For each training example, $$<x,c(x)>$$
> * Remove from VS any hypothesis $$h$$ for which $$h(x)\neq c(x)$$
> Output VS

Though this algorithm can output all consistent hypotheses, we need to list all possible explanation. It is possible in small predictions space. However, it is impossible for infinite hypothesis spaces.

#### 3.Candidate-Elimination

> Initialize $$G$$ to the set of maximally-general hypotheses in $$H$$
> Initialize $$S$$ to the set of maximally-specific hypotheses in $$H$$
> For each training example $$d$$, do
> * If $$d$$ is a positive example
>   * Remove from $$G$$ any hypothesis that is inconsistent with $$d$$
>   * For each hypothesis $$s$$ in $$S$$ that is not consistent with $$d$$
>     * Remove $$s$$ from $$S$$
>     * Add to $$S$$ to all minimal gegneralizations $$h$$ of $$s$$ such that
>       * $$h$$ is consistent with $$d$$, and some member of $$G$$ is more general than $$h$$
>     * Remove from $$S$$ any hypothesis that is more general than another hypothesis in $$S$$

<figure class="align-center">
  <a href="#"><img src="{{ '/images/Version_space.jpg' | absolute_url }}" alt=""></a>
  <figcaption>Version Space, Source: Wikipedia</figcaption>
</figure> 