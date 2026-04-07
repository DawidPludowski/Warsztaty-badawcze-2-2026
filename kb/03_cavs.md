# Concept Activation Vectors

## Classification-based

Classification-based CAVs create a linear model and define the CAV as a normal to the classification hyperplane.

### SVM (first CAV from TCAV work)

## LR

## Classification-free

Classification-free CAVs are created based on statistics and do not assume any specific distribution of data in latent space.

### FastCAV

### PatCAV

## SAE-based

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
