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

#### **Introduction & Motivation** 
While Large Language Models (LLMs) generate fluent text, precise behavioral control—such as modulating hallucinations—remains difficult due to the limited flexibility and interpretability of traditional alignment methods like RLHF (Reinforcement Learning from Human Feedback). *Activation steering* offers a runtime alternative, yet conventional dense methods are hindered by **superposition** (a phenomenon where neural networks compress more concepts than they have available dimensions, causing unrelated features to become entangled within the same neurons).  

**Sparse Activation Steering (SAS)** addresses this issue by using Sparse Autoencoders (SAEs) to decompose dense activations into a structured dictionary of disentangled, monosemantic features, enabling precise and interpretable interventions.


**How SAS Vector Generation Works (6-Step Process):**
1. **Contrastive Pairs:** Construct a pair of prompts where one completion shows the desired behavior (positive) and the other its exact opposite (negative).
2. **Sparse Extraction:** Extract sparse representations of the model's activations using an SAE encoder $f(a)$.
3. **Filtering:** Remove inactive features using an activation frequency threshold $\tau$.
4. **Isolate Specifics:** Turn off shared features between the positive and negative representations to isolate components specific to the target behavior.
5. **Mean Calculation:** Compute the mean activation vectors from the remaining sparse matrices.
6. **Final Vector Construction:** Subtract the negative mean vector from the positive mean vector. The resulting SAS vector reinforces the intended behavior while suppressing contradicting tendencies.

#### **Background: Activation Steering & Sparse Autoencoders**

**Activation Steering**
Activation steering is an inference-time intervention that modifies LLM latent representations to guide model generation toward or away from specific behaviors (e.g., honesty, refusal, or sentiment) without the need for fine-tuning. 

A foundational method is **Contrastive Activation Addition (CAA)**, where a steering vector $v_{(b,\ell)}$ is computed as the difference between mean activations of positive ($c^+$) and negative ($c^-$) behavior completions across the dataset $D_b$:

$$v_{(b,\ell)} = \frac{1}{|D_b|} \sum_{(p_i,c^+_i,c^-_i) \in D_b} [ a_\ell(p_i, c^+_i) - a_\ell(p_i, c^-_i) ]$$

In traditional dense steering, this vector is added directly to the hidden state $a^t_\ell$ during the forward pass, scaled by a strength parameter $\lambda$:
$$\tilde{a}^t_\ell = a^t_\ell + \lambda \cdot v_{(b,\ell)}$$

#### **The SAS Inference Mechanism**

The core innovation of SAS is performing this steering within the **sparse latent space** provided by a **Sparse Autoencoder (SAE)**. This avoids the "entanglement" of features inherent in dense vectors.

**The Steering Pipeline:**
1.  **Encoding:** Dense activations $a$ from layer $\ell$ are mapped to sparse features using the SAE encoder: 
    $$f(a) = \sigma(W_{enc}a + b_{enc})$$
2.  **Sparse Injection:** The pre-computed SAS steering vector is added to these sparse features, scaled by $\lambda$.
3.  **Consistency Check:** The modified sparse representation is passed through the SAE non-linearity $\sigma$ again. This ensures the steered activations remain within the learned sparse distribution.
4.  **Decoding:** The steered sparse features are projected back into the original dense activation space via the SAE decoder to continue the model's forward pass.

#### **Sparse Autoencoders (SAEs)**

**The Challenge: Superposition**

Large Language Models often compress more concepts than they have available dimensions in their internal representations. This leads to **superposition**—a phenomenon where unrelated concepts become entangled within the same neurons. While efficient for the model, it makes precise control and interpretability nearly impossible.


**The Solution: Dictionary Learning**

SAEs address superposition by decomposing dense activations into a high-dimensional, sparse, and ideally **monosemantic** (single-meaning) feature space.


**Architecture & Mechanics**

An SAE uses an encoder-decoder structure to map a dense activation vector $a \in \mathbb{R}^n$ to a sparse latent representation $f(a) \in \mathbb{R}^M$ (where $M \gg n$):

* **Encoder:** $f(a) = \sigma(W_{enc}a + b_{enc})$
* **Decoder:** $\hat{a}(f) = W_{dec}f + b_{dec}$

The activation function $\sigma$ (such as ReLU, TopK, or JumpReLU) is critical as it enforces the **sparsity** of the representation.


**Training Objectives**

SAEs are trained by balancing two competing goals in the loss function $L(a)$:
1.  **Reconstruction Loss:** $||a - \hat{a}(f(a))||^2_2$ — ensures the decoded output faithfully matches the original input.
2.  **Sparsity Penalty:** $\lambda \cdot ||f(a)||_1$ — minimizes the number of active features, forcing the model to find the most "important" disentangled directions.


**Interpretability & Control**

The reconstructed activation can be viewed as a linear combination of "dictionary directions" (columns of the decoder matrix $d_i$):
$$\hat{a}(f) = \sum_{i=1}^{M} f_i \cdot d_i$$


Because only a small subset of features $f_i$ are active at any time, SAEs allow for **modular control**. Specific behaviors can be isolated, adjusted, or suppressed independently without the interference typical of dense representation spaces.

#### **3. Method: Sparse Activation Steering (SAS)**

Previous attempts to steer in sparse spaces faced two main hurdles:
1. **Unsupervised Feature Reliance:** Assuming SAE features are perfectly monosemantic is risky; many complex behaviors are actually composed of multiple sub-features.
2. **Dense-to-Sparse Mapping Issues:** Directly encoding "dense steering vectors" into SAEs fails because these vectors often fall outside the SAE's training distribution and contain negative values that the SAE (using ReLU/non-negative activations) cannot process.


Instead of converting dense vectors, SAS derives steering vectors **directly from the sparse representation** using a contrastive approach.

**The 6-Step Generation Process:**
1. **Contrastive Pairing:** Use datasets $D_b$ with prompts $p_i$ and contrastive completions ($c^+$ for desired behavior, $c^-$ for opposite).
2. **Sparse Extraction:** Extract sparse latent matrices $S^+$ and $S^-$ for the layer $\ell$ using the SAE encoder $f_\ell$.
3. **Mean Activation:** Compute sample means $v^+$ and $v^-$ for features. A threshold $\tau$ is used to keep only features that appear consistently across a fraction of the prompts.
4. **Feature Isolation:** Identify and **remove shared features** (set to zero) that appear in both $v^+$ and $v^-$. This eliminates "noise" like shared syntax or positional artifacts, leaving only behavior-specific features.
5. **Vector Composition:** The final SAS vector is defined as: $$v_{(b,\ell)} = v^+_{(b,\ell)} - v^-_{(b,\ell)}$$ The positive term reinforces desired traits, while the negative term suppresses contradictory model tendencies.

During generation, the model's activations are modified in the sparse space while maintaining the structural integrity of the SAE.

**The Steering Equation:**
$$\tilde{a}^t_\ell = \hat{a}^t_\ell \left( \sigma ( f(a^t_\ell) + \lambda \cdot v_{(b,\ell)} ) \right) + \Delta$$

* **$\lambda$ (Steering Strength):** A tunable scalar. $\lambda > 0$ amplifies the behavior; $\lambda < 0$ suppresses it.
* **$\sigma$ (Non-linearity):** Re-applied in the sparse space to ensure the resulting activations remain within the valid non-negative distribution.
* **$\Delta$ (Correction Term):** Defined as $\Delta := a^t_\ell - \hat{a}^t_\ell(f(a^t_\ell))$. This compensates for the SAE's reconstruction error, ensuring the model's performance doesn't degrade due to imperfect encoding/decoding.

#### **Experimental Results & Outlook**

**Key Achievements**
* **Successful Steering:** SAS effectively modulates 7 core behaviors (e.g., refusal, hallucination, sycophancy) in **Gemma-2 (2B/9B)**.
* **Bidirectional Control:** Positive $\lambda$ reinforces behaviors, while negative $\lambda$ suppresses intrinsic model biases.
* **Robust Transferability:** SAEs trained on base models transfer effectively to **instruction-tuned** versions with minimal loss.

**Core Parameters**
* **Threshold ($\tau$):** Controls feature consistency. Lower $\tau$ (0.7) yields stronger shifts; higher $\tau$ (0.9) isolates "core" monosemantic features.
* **Strength ($\lambda$):** Acts as a linear "volume knob" for behaviors. Increasing $\lambda$ from $\pm 1$ to $\pm 2$ intensifies the effect without collapsing model coherence.

**Perspectives**
* **Modular Alignment:** Sparse spaces allow for **feature compositionality**, enabling the simultaneous and independent adjustment of multiple traits.
* **Surgical Safety:** Provides a path for high-precision safety interventions by targeting specific features rather than broad, dense directions.

**Performance Across Tasks**
* **Multiple-Choice Precision:** SAS vectors significantly shifted model probabilities toward desired behaviors in held-out tests. The most effective steering occurred in **intermediate layers** (Layers 12–14), where high-level behavioral concepts are formed.
* **Open-Ended Generation:** Using LLM-as-a-judge (GPT-4o), the study confirmed that SAS effectively guides long-form text generation. Adding an "Answer is:" prefix further enhanced steerability.
* **Benchmark Integrity:** Unlike fine-tuning, moderate steering ($\lambda = 1$ to $2$) maintained or even **improved performance** on standard benchmarks like MMLU and TruthfulQA (e.g., non-hallucination vectors increased factual accuracy).

**Key Technical Milestones**
* **Scaling Monosemanticity:** Increasing SAE dictionary size (up to 1M) directly improved **feature disentanglement**. Larger SAEs resulted in sparser SAS vectors, meaning fewer but more precise neurons control specific behaviors.
* **Feature Compositionality:** Proved that multiple sparse vectors can be used simultaneously. For example, the model could be steered toward a "Myopic" behavior while independently adjusting "Gender" preferences without the two signals interfering.
* **Behavioral Correlation:** Discovered "cross-over" features between behaviors; for instance, features used for **Refusal** overlap with those that suppress **Hallucination**, suggesting underlying semantic links in the model's latent space.

**Optimization & Stability**
* **$\Delta$ (Delta) Correction:** The researchers introduced a correction term to offset information lost during SAE encoding. This stabilized the model, especially in early layers, preventing performance fluctuations.
* **Bidirectional Strategy:** The best results came from using both positive features (to reinforce) and negative features (to suppress), rather than one-sided steering.

**Future Perspectives**
* **Surgical Interventions:** SAS allows for context-dependent behavior modification (e.g., turning on "Factuality" only for medical queries) without permanently altering the model's weights.
* **Scaling Potential:** Further scaling of SAEs and higher-quality contrastive data are expected to produce even more reliable and "monosemantic" control knobs for AI alignment.

#### **Critical Summary: Pros & Cons**

The **Sparse Activation Steering (SAS)** framework represents a significant step toward more controllable and interpretable LLMs. Below is a summary of the strengths and limitations of the proposed approach and its evaluation.

##### **Pros**
* **Performance & Reliability:** SAS achieves strong empirical results comparable to traditional dense steering, and in some cases, even improves performance on standard benchmarks like TruthfulQA and MMLU.
* **Enhanced Interpretability:** By operating in sparse latent spaces, the method provides modular control over disentangled, monosemantic features, which is a major leap over "black-box" dense steering.
* **Comprehensive Experimental Coverage:** The study demonstrates high efficacy across a wide variety of behaviors (e.g., hallucination, sycophancy, refusal) and tasks (multiple-choice vs. open-ended generation).

##### **Cons**
* **Limited Evaluation Scope:** The study is primarily restricted to the **Gemma** model family (2B and 9B variants). It remains to be seen how well these specific SAE features generalize across other architectures (e.g., Llama or Mistral).
* **Narrow Methodological Comparison:** While SAS is compared to baseline dense steering, there is a lack of extensive comparison with other emerging sparse steering or advanced fine-tuning alignment methods.
* **Computational Overhead:** The analysis does not fully account for the computational costs associated with using **large SAE dictionaries** (up to 1M features) during real-time inference, which may impact latency in production environments.

Overall, the article presents a solid and empirically validated approach to behavior control in LLMs, though further validation and theoretical grounding would strengthen its conclusions.

### S&PTopK

### SAE PRobe
