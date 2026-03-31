# Concept Activation Vectors

## Classification-based

Classification-based CAVs create a linear model and define the CAV as a normal to the classification hyperplane.

### TCAV (and CAVs idea)

Original paper: [Interpretability Beyond Feature Attribution: Quantitative Testing with Concept Activation Vectors (TCAV)](https://proceedings.mlr.press/v80/kim18d.html), Kim, B., Wattenberg, M., Gilmer, J., Cai, C., Wexler, J., Viegas, F., Sayres, R. (2018)

#### Theory

First of all, CAV (Concept Activation Vector) is a vector in a network's activation space pointing in the direction of a human-defined concept, obtained by training a linear classifier (e.g. linear regression or linear SVM) of concept examples vs. random negatives on the chosen inner layer's activations. TCAV (Testing with CAVs) however is the full method built on top of CAVs - it uses directional derivatives to turn a CAV into a quantitative, class-level score measuring how strongly a concept influences a model's predictions across an entire dataset.

The core problem TCAV addresses is that neural networks operate in a specific vector space $E_m$ (e.g., stating about pixel values), while humans reason in a different space $E_h$ of high-level concepts (e.g., "stripes", "gender", "colour"). An interpretation of a model can be therefore seen as a function $g: E_m \rightarrow E_h$. When $g$ is linear, the authors call it linear interpretability. Linear classifiers have been shown to be sufficient to capture a surprising amount of meaningful structure in neural activations.

But how exactly find the right CAV for a given concept (using TCAV)? There are some steps:

- Step 1 - Define a concept via examples: The user provides a positive set $P_C$ (e.g., images of striped textures) and a negative set $N$ (random images). No model retraining is needed.
  
- Step 2 - Find the Concept Activation Vector (CAV): Feed both sets through the frozen network up to layer $l$, obtaining activations $f_l(x) \in \mathbb{R}^m$. Train a binary linear classifier to separate sets $\\{f_l(x) : x \in P_C\\}$ and $\\{f_l(x) : x \in N\\}$. The vector orthogonal to the resulting decision boundary is the CAV: $v_C^l \in \mathbb{R}^m$ - it points (hopefully) in the direction of the concept within the layer's activation space.
  
- Step 3 - Compute Conceptual Sensitivity: For a given input $x$ and class $k$, the sensitivity to concept $C$ at layer $l$ is the directional derivative of the logit function $h_{l,k}: \mathbb{R}^m \rightarrow \mathbb{R}$ in the direction of the CAV:
  
    $$S_{C,k,l}(x) = \nabla h_{l,k}(f_l(x)) \cdot v_C^l \text{ (dot product)}$$  
  
  This is an unscaled cosine similarity between the gradient of the model output as a function of layer $l$'s activations and the concept direction. A positive value means that nudging the representation toward the concept increases the predicted probability of class $k$.
  
- Step 4 - Compute the TCAV Score: The final metric aggregates over an entire class $X_k$:
  
  $$\text{TCAV}^Q_{C,k,l} = \frac{|{x \in X_k : S_{C,k,l}(x) > 0}|}{|X_k|}$$
  
  This is the fraction of inputs in class $k$ for which the concept has a positive influence on the prediction - a single global number per (concept, class, layer) triple.
  
- Step 5 - Statistical significance testing: To avoid spurious CAVs (a random set of images will always produce *some* CAV), the process is repeated ~500 times with different random negative sets. A two-sided t-test with Bonferroni correction is used to reject CAVs whose TCAV scores are not significantly different from 0.5 (i.e. they are no better than random).
  

##### Relative CAVs

Semantically related concepts (e.g., brown hair vs. black hair) produce CAVs that are far from orthogonal. Rather than comparing a concept against random negatives, a **relative CAV** is trained by opposing two related concept sets directly (e.g., $P_\text{stripe}$ vs. $P_\text{dot} \cup P_\text{mesh}$). The resulting vector $v^l_{C,D}$ defines a 1-D subspace: projecting $f_l(x)$ onto it measures whether $x$ is more similar to concept $C$ or $D$.

##### Why is TCAV better than alternatives?

The authors argue that traditional feature attribution methods (saliency maps) have four key limitations: (1) they are local - each map applies to a single input, so users must manually inspect many images to draw class-wide conclusions; (2) they offer no control over which concepts are surfaced; (3) saliency maps produced by untrained networks can be visually similar to those of trained ones; (4) simple pre-processing steps (e.g., mean shift) or adversarial perturbations (modifying image so that for human's eye it looks the same, but vision models see it completely different) can drastically alter saliency maps without changing model behaviour. A 50-person human experiment confirmed this: saliency maps correctly communicated the more important concept only 52% of the time (barely above the 50% random baseline), and subjects' confidence was no higher when they were correct than when they were wrong, suggesting saliency maps can be actively misleading.

TCAV addresses all four limitations: it requires no ML expertise, works for any user-defined concept (including ones absent from training data labels), needs no model retraining and produces a single quantitative global measure per class.

#### Downstream Tasks

- **Model interpretation** - quantifying which high-level visual features (colour, texture, shape, objects) drive a classification decision, e.g., confirming that "stripes" are important for "zebra" and "red" for "fire engine".
- **Bias and fairness analysis** - detecting whether a model relies on sensitive attributes it was not explicitly trained on. The paper demonstrates this by finding that the concept "female" is highly relevant to the "apron" class and that ping-pong balls are correlated with a specific racial group.
- **Identifying where concepts are learned** - CAV classifier accuracy across layers shows that simple concepts (e.g. colour) are decodable from early layers throughout the network, while abstract concepts (e.g. objects, people) only become linearly separable in deeper layers, confirming the widely-held view of hierarchical feature learning.
- **Medical AI validation** - applying TCAV to a diabetic retinopathy (DR) grading model to verify whether clinically relevant lesion types (microaneurysms, laser scars) drive predictions at each severity level, and to identify where model predictions diverge from a domain expert's heuristics.

#### Datasets

Datasets used during evaluation:

- [ImageNet](https://www.image-net.org/) - general object classification; used to test TCAV on classes such as "zebra", "cab" or "dumbbell".
- [Diabetic Retinopathy (DR) dataset (Krause et al., 2017)](https://www.sciencedirect.com/science/article/abs/pii/S0161642017326982) - retinal fundus images graded on a 0-4 severity scale; used to validate TCAV in a real-world medical setting.
- Controlled caption dataset (authors' own) - images of three classes (zebra, cab, cucumber) with optionally noisy text captions overlaid, used to construct a controlled experiment with an approximated ground truth for TCAV evaluation.
- Some common search engines' images

#### Related Literature

TCAV informally started the subfield of concept-based interpretability, i.e. methods that explain neural network predictions in terms of human-defined semantic concepts rather than individual input features.

- **Ghorbani et al. (2019)** - [*Towards Automatic Concept-based Explanations*](https://proceedings.neurips.cc/paper/2019/hash/77d2afcb31f6493e350fca61764efb9a-Abstract.html): An extension of TCAV that removes the need for manually curated concept sets. ACE automatically discovers visual concepts by segmenting images into patches, clustering them in activation space, and scoring each cluster with TCAV. The result is a set of human-meaningful, globally relevant concepts extracted without any user input.
  
- **Wei Koh et al. (2020)** - [*Concept Bottleneck Models*](https://proceedings.mlr.press/v119/koh20a) - Introduces an architecture where the model first predicts human-defined concepts (e.g., "bone spurs"), then uses them to predict the final label. This allows users to intervene at test time by correcting concept predictions, improving both interpretability and accuracy.
  
- **Pahde et al. (2021)** - [*Reveal to Revise: An Explainable AI Life Cycle for Iterative Bias Correction of Deep Models*](https://link.springer.com/chapter/10.1007/978-3-031-43895-0_56) - A full XAI pipeline for detecting and correcting spurious correlations in models. Reveal to Revise iteratively reveals model weaknesses via attribution outliers or latent concept inspection, localizes the responsible artifacts in the input data, and revises model behavior accordingly. Validated on medical imaging tasks (melanoma detection, bone age estimation).

### SVM (first CAV from TCAV work)

## LR

## Classification-free

Classification-free CAVs are created based on statistics and do not assume any specific distribution of data in latent space.

### FastCAV

### PatCAV

## SAE-based

### S&PTopK

### SAE PRobe
