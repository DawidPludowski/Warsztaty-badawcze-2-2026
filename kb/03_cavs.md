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

### SAS (Sparse Activation Steering)

[Steering Large Language Model Activations in Sparse Spaces](https://arxiv.org/pdf/2503.00177)

#### Introduction

Modern Large Language Models (LLMs) generate outputs based on patterns learned from vast datasets. However, they do not inherently distinguish between *desirable* and *undesirable* behaviors (e.g., being factual vs. hallucinating, helpful vs. sycophantic). **Behavior steering** refers to techniques that allow us to *actively guide* a model’s outputs toward specific traits without retraining the entire model.

**Why this matters:**
- Changing LLMs behaviour without costly and rigid **fine-tuning**
- Enable **customization** (adapt behavior to specific use cases), for example:
  - Improve **safety** (reduce harmful or misleading outputs)
  - Increase **reliability** (e.g., factual accuracy)

##### Limitations of Previous Approaches

Earlier methods often relied on **dense representations**, where model behavior is controlled using vectors in a compressed latent space. However, dense representations suffer from **superposition**, a phenomenon where multiple unrelated concepts are encoded within the same set of neurons or dimensions. Instead of having a one-to-one mapping between features and meanings, the model reuses its limited representational capacity, causing different concepts to overlap in the same space.

**Key Problems**
- Dense vectors tend to **mix multiple concepts into a single representation**
- A single “direction” may simultaneously encode:
  - tone
  - topic
  - style
  - bias

**Consequences:**
- Difficult to isolate and control a *specific behavior*
- Steering one trait may unintentionally affect others
- Leads to **unpredictable and less interpretable outcomes**

**The "Out-of-Distribution" (OOD) Challenge**

Earlier attempts to translate dense steering vectors into sparse spaces faced a critical barrier: dense vectors are inherently **out-of-distribution** for Sparse Autoencoders (SAEs). SAEs are optimized to reconstruct natural model activations during generation, not artificially constructed difference vectors.

- **Inaccurate Reconstruction:** Passing a dense steering vector through an SAE often strips it of its original steering properties or introduces unwanted artifacts.
- **Negative Projections:** SAEs typically operate in a **non-negative feature space** (enforced by activation functions like ReLU or JumpReLU). Dense vectors naturally contain negative values, meaning standard SAEs cannot process them correctly and discard crucial behavioral information.
- **Mapping Errors:** Instead of cleanly activating specific sparse features, out-of-distribution dense vectors create noise across multiple neurons simultaneously, completely negating the primary benefit of sparsity - clean behavior isolation.

#### Proposed Solution: Sparse Activation Steering (SAS)

Sparse Activation Steering (SAS) is built on a simple but powerful shift in perspective: instead of controlling model behavior through **dense, entangled directions**, it operates in a **high-dimensional sparse feature space**, where representations are more structured and decomposed.

The key idea is that model activations can be expressed in terms of many **fine-grained features**, each capturing a narrow and more interpretable aspect of behavior. By working in this space, SAS enables direct manipulation of these features, rather than indirectly influencing behavior through coarse, mixed signals.

##### Core Intuition
- In a sparse representation:
  - Most features are **inactive (zero)** at any given time
  - Only a small subset becomes active for a specific input
- This creates a setting where:
  - Features are more **specialized**
  - Individual dimensions are more likely to correspond to **distinct concepts**

##### Key advantages
- **Better separation of behaviors**  
  Individual traits (e.g., “refusal”, “factuality”) are less entangled and can be isolated more cleanly

- **More precise control**  
  Specific behaviors can be adjusted without unintentionally affecting others

- **Improved interpretability**  
  Active features are more meaningful, making the model’s behavior easier to understand and analyze

- **Modularity**  
  Different behaviors can be combined, adjusted, or removed independently, enabling flexible control (e.g. we can increase the tendency to take risks while answering with more empathy)

##### Obtaining Sparse Representations

Sparse Autoencoders (SAEs) learn these representations by minimizing a combination of **reconstruction error** and a **sparsity penalty**:

$$
L(a) = \underbrace{\| a - \hat{a}(f(a)) \|_2^2}_{\text{Reconstruction Loss}} + \lambda \underbrace{\| f(a) \|_1}_{\text{Sparsity Penalty}}
$$

- The $\ell_2$ term ensures faithful reconstruction of the input activations.
- The $\ell_1$ term enforces sparsity, keeping only a small subset of features active.

#### Constructing and Applying Concept Vectors

After introducing sparse representations, we now describe how specific behaviors are translated into controllable vectors and injected into the model. The key idea is to identify which sparse features correspond to a desired behavior, construct a direction that amplifies those features while suppressing opposing ones, and inject this direction into the model’s internal activations during generation, enabling targeted, fine-grained control without disrupting the model’s overall behavior.

  
##### Step 1: Contrastive Setup

We start with a dataset of prompts $p_i$ and two types of completions:
- $c^+$: examples exhibiting the **desired behavior**
- $c^-$: examples exhibiting the **opposite behavior**

This contrast allows us to isolate what *distinguishes* the behavior of interest.


##### Step 2: Sparse Feature Extraction

For a given layer $\ell$, both types of completions are passed through the model and encoded into a sparse space:

- $S^+ = f_\ell(p_i, c^+)$  
- $S^- = f_\ell(p_i, c^-)$  

where $f_\ell$ maps activations into sparse feature representations.


##### Step 3: Aggregating Consistent Features

We compute average activations across samples:

- $v^+$: mean feature activations for desired behavior  
- $v^-$: mean feature activations for opposite behavior  

To ensure robustness, we apply a threshold $\tau$ and keep only features that appear consistently across many examples, i.e., features for which the fraction of samples with non-zero activation exceeds $\tau$.


##### Step 4: Isolating Behavior-Specific Features

In this step, we filter out noise by removing features that appear in both $v^+$ and $v^-$ (e.g., shared syntax or formatting), setting them to zero so that only features specific to the desired and opposing behaviors remain.


##### Step 5: Building the Concept Vector

The final steering vector is defined as:

$$
v_{(b,\ell)} = v^+_{(b,\ell)} - v^-_{(b,\ell)}
$$

**Intuition:**
- $v^+$ captures what we want to **encourage**
- $v^-$ captures what we want to **suppress**

Their difference produces a direction that cleanly represents the target behavior.

##### Step 6: Steering the Model During Generation

At inference time, we modify the model’s activations at layer $\ell$:

$$
\tilde{a}^t_\ell = \hat{a}^t_\ell \left( \sigma \big( f(a^t_\ell) + \lambda \cdot v_{(b,\ell)} \big) \right) + \Delta
$$

**Key Components**

- **$\lambda$ (steering strength):**  
  Controls intensity  
  - $\lambda > 0$: amplifies the behavior  
  - $\lambda < 0$: suppresses it  

- **$\sigma$ (non-linearity):**  
  Ensures the modified representation remains valid in the sparse (non-negative) space  

- **$\Delta$ (correction term):**  
  Compensates for reconstruction error introduced by the sparse encoding, preserving model quality  

#### Results and Key Takeaways

Sparse Activation Steering demonstrates that operating in a high-dimensional sparse space enables **clean isolation of individual behavioral features**, which can then be **combined compositionally** to control multiple traits independently without interference. The method scales well: increasing the dimensionality (larger SAE dictionaries) improves **feature disentanglement**, leading to more precise and interpretable control signals. Importantly, the most effective steering occurs in **intermediate layers**, where abstract behavioral representations emerge, rather than in early layers. Empirically, SAS achieves reliable, bidirectional behavior control, transfers across model variants, and maintains (or even improves) benchmark performance, suggesting that sparse representations provide a practical and robust foundation for fine-grained, modular alignment of LLMs.


### S&PTopK

### Rethinking Sparse Autoencoders: Select-and-Project for Fairness and Control from Encoder Features Alone

### [Link]
[OpenReview: Rethinking Sparse Autoencoders: Select-and-Project for Fairness and Control from Encoder Features Alone](https://openreview.net/forum?id=UjHe8F48eE)

### [Theory]
**Introduction:**
The proposed method leverages the encoder of a Sparse Autoencoder (SAE) to isolate and control specific concepts in the latent space, treating the SAE encoder weights similarly to Concept Activation Vectors (CAVs) acting as attribute detectors. The mechanism works in the following steps:
1. **Feature Selection:** After mapping to a sparser space via the encoder, the model selects the top k most important hidden features related to the target concept. This selection is done in one of three ways, two of which are:
   * Calculating the correlation between each hidden feature and its CLIP score, then ranking them by the highest correlation.
   * Training a logistic regression model on the SAE features to predict the concept, and selecting the features with the highest absolute weights.
2. **Aggregation (Optional):** Aggregating the k most informative features.
3. **Orthogonal Projection:** Based on the selected features (forming matrix A), an orthogonal projection is calculated using the formula: 
   V = I - αA(A^TA)^-1A^T
   The parameter α controls the balance between preserving the model's overall utility and the strength of control over the specific attribute.
4. **Application:** The resulting matrix V is applied to the input of the encoder.

**Why the authors claim it is better:**
The authors argue their solution is superior because it operates directly on encoder features rather than decoder reconstructions. It functions as a projection in the original embedding space rather than overwriting embeddings as a sum of decoder weights. This ensures the method remains faithful to the model's initial representations, thereby maintaining the model's general utility and achieving higher performance in other domains.

### [Downstream tasks]
* **Gender Bias Analysis:** Using hair color as a downstream classification task to analyze and mitigate gender bias.
* **Toxicity Reduction:** Evaluating the model's ability to generate text with reduced aggression. This was evaluated using an LLM-as-a-judge, comparing a standard baseline model, previous SAE-based approaches, and the proposed approach (S&P Top-K) which uses modified activations.

### [Datasets]
* **CelebA**
* **FairFace**
* **Custom LLM-generated data** (used specifically for the toxic/aggressive opinion generation task).

### [Related literature]
Extensive related literature building upon this specific CAV method is difficult to provide, as the paper is quite recent and was actually rejected from an A* conference, but it was accepted for Neurips workshop.

The lack of adoption and the rejection stem from the following reviewer critiques, which highlight current gaps in this method's theoretical and practical standing:
* **Lack of novelty:** Orthogonal projection is a widely used technique, and the paper failed to position itself well against existing, established methods.
* **Poor interpretability:** The paper lacked visualizations that would help explain how the top k features from the sparse encoder actually contribute to semantic concepts.
* **Weak theoretical foundations:** Reviewers noted that the procedure felt empirical (trial-and-error) rather than being grounded in solid theory.
* **Evaluation flaws:** The evaluation lacked proper calibration and enhancement of the baseline methods it was compared against, particularly in the LLM-as-a-judge context.

### SAE PRobe
