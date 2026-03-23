# Datasets

## With multiple concepts

### CelebA

**Link**: [Deep Learning Face Attributes in the Wild](https://openaccess.thecvf.com/content_iccv_2015/html/Liu_Deep_Learning_Face_ICCV_2015_paper.html)

**Short Description**: The dataset contains images of celebrities, annotated with 40 attributes (concepts), along with landmarks and bounding boxes (irrelevant for the concept detection task). The dataset is **super** popular for various tasks in machine learning, like segmentation, image generation, bias detection, etc. Notably, it has no explicit classes for the classification task. However, one can use any of the 40 attributes for classification.

**Volume metrics**: Number of images: 202,599; number of unique persons: 10,177 celebrities; number of concepts: 40 per image

**Concepts available**: `bald`, `no beard`, `mustache`, `chubby`, `smiling`, `attractive`, etc. *A known issue* is that some concepts are arbitrary (e.g., `young`, `attractive`) or mislabeled. The methodology of labelling is not clear. The full list is provided below.

| Attribute | Description | Attribute | Description |
|----------|-------------|----------|-------------|
| 5_o_Clock_Shadow | Visible beard shadow | Goatee | Goatee beard |
| Arched_Eyebrows | Eyebrows arched upward | Gray_Hair | Gray hair color |
| Attractive | Subject perceived as attractive | Heavy_Makeup | Noticeable makeup |
| Bags_Under_Eyes | Visible under-eye bags | High_Cheekbones | Prominent cheekbones |
| Bald | No hair on scalp | Male | Male gender |
| Bangs | Hair covering forehead | Mouth_Slightly_Open | Mouth slightly open |
| Big_Lips | Relatively large lips | Mustache | Mustache present |
| Big_Nose | Relatively large nose | Narrow_Eyes | Narrow eye shape |
| Black_Hair | Black hair color | No_Beard | No facial hair |
| Blond_Hair | Blond hair color | Oval_Face | Oval face shape |
| Blurry | Image is blurry | Pale_Skin | Light/pale skin tone |
| Brown_Hair | Brown hair color | Pointy_Nose | Sharp nose shape |
| Bushy_Eyebrows | Thick eyebrows | Receding_Hairline | Hairline receding |
| Chubby | Full/round face | Rosy_Cheeks | Pinkish cheeks |
| Double_Chin | Visible double chin | Sideburns | Sideburn facial hair |
| Eyeglasses | Wearing eyeglasses | Smiling | Subject smiling |
| Wearing_Earrings | Wearing earrings | Straight_Hair | Straight hair texture |
| Wearing_Hat | Wearing a hat | Wavy_Hair | Wavy hair texture |
| Wearing_Lipstick | Wearing lipstick | Wearing_Necklace | Wearing necklace |
| Wearing_Necktie | Wearing necktie | Young | Subject appears young |

**Why is it relevant?**: Concepts are self-explanatory and do not require an expert's knowledge to state hypotheses about their relations. It is used almost everywhere in computer vision research as a good benchmarking standard. **Hypothesis to be evaluated**:

- Do `no beard` and `goatee`, etc., are truly hierarchical? Can we do better CAVs for these concepts?
- Are there other relations of concepts? E.g., `heavy makeup` and `lipstick` should be related
- CelebA is known for its biases (e.g., one of the genders has typically blonde hair). Can we remove them?

**Known biases**:

1. models tend to predict blonde people as women ([link to paper](https://papers.nips.cc/paper_files/paper/2023/hash/2ecc80084c96cc25b11b0ab995c25f47-Abstract-Conference.html))
2. models tend to predict smiling based on gender ([link to paper](https://openaccess.thecvf.com/content/CVPR2024/papers/Zhang_Distributionally_Generative_Augmentation_for_Fair_Facial_Attribute_Classification_CVPR_2024_paper.pdf))

**Known hiearchies**:

*Note: the hierarchies are found statistically as "$A \rightarrow B$ if B is present in >99% observations with A, and A is present in <99% observations with B" to remove "if and only if" situations.*

1. `5_o_Clock_Shadow` $\rightarrow$ $\lnot$ `No_Beard`
2. `Goatee` $\rightarrow$ $\lnot$ `No_Beard`
3. `5_o_Clock_Shadow` $\rightarrow$ `Male`
4. `Bald` $\rightarrow$ `Male`
5. `Goatee` $\rightarrow$ `Male`
6. `Heavy_Makeup` $\rightarrow$ `No_Beard`
7. `Mustache` $\rightarrow$ `Male`
8. `Rosy_Cheeks` $\rightarrow$ `No_Beard`
9. `Sideburns` $\rightarrow$ `Male`
10. `Wearing_Lipstick` $\rightarrow$ `No_Beard`
11. `Wearing_Necktie` $\rightarrow$ `Male`

**Related papers**:

- [A Quantitative Analysis of Labeling Issues in the CelebA Dataset](https://link.springer.com/chapter/10.1007/978-3-031-20713-6_10) - TL;DR labels/concepts in CelebA are contradictory, bad, etc.
- [Consistency and Accuracy of CelebA Attribute Values](https://openaccess.thecvf.com/content/CVPR2023W/VDU/html/Wu_Consistency_and_Accuracy_of_CelebA_Attribute_Values_CVPRW_2023_paper.html) - TL;DR labels/concepts in CelebA are not consistent, authors perform manual analysis with independent experts, notably, they propose a corrected version of one of the concepts

### Cub-200

**Link**: [The Caltech-UCSD Birds-200-2011 Dataset](https://authors.library.caltech.edu/27452/1/CUB_200_2011.pdf) (*Note: direct link to the pdf download*)

### FunnyBirds

*Note: This is a particularly interesting dataset for us, as it allows us to generate any relation between concepts synthetically*.

**Link**: [FunnyBirds: A Synthetic Vision Dataset for a Part-Based Analysis of Explainable AI Methods](https://openaccess.thecvf.com/content/ICCV2023/html/Hesse_FunnyBirds_A_Synthetic_Vision_Dataset_for_a_Part-Based_Analysis_of_ICCV_2023_paper.html)

### ISIC

**Link**: [https://ieeexplore.ieee.org/abstract/document/8363547](https://ieeexplore.ieee.org/abstract/document/8363547) (*Note: ISIC is released in multiple versions, almost 1 version per year, so newer versions should be mentioned/compared*)

### ImageNet + WordNet

*Note: When used like in [this paper](https://arxiv.org/pdf/2602.11448)*

### ImageNet - content

**Link**: [ImageNet: A Large-Scale Hierarchical Image Database](https://ieeexplore.ieee.org/document/5206848) and the [Official ImageNet Website](https://www.image-net.org/).

![Sample images from ImageNet](https://viso.ai/wp-content/uploads/2024/04/ImageNet.jpg)
*Sample images from ImageNet, sourced from viso.ai.*

**Impact**: As of March 2026, the foundational 2009 paper has recorded **93,041 citations** on Google Scholar. The project represents a fundamental shift in AI research, famously summarized by its creator, Fei-Fei Li: 
> *"The paradigm shift of the ImageNet thinking is that while a lot of people are paying attention to models, let’s pay attention to data. Data will redefine how we think about models."*

It established the **ILSVRC** (ImageNet Large Scale Visual Recognition Challenge), which served as the definitive benchmark for deep learning progress between 2010 and 2017.

**Short Description**: ImageNet is a large-scale visual ontology based on the **WordNet** lexical structure. It was designed to map the physical world into a structured digital format by associating objects with a hierarchical network of concepts . It pioneered a "data-centric" paradigm, demonstrating that massive scale and taxonomical diversity are essential for developing robust and generalizable visual recognition systems.

**Metrics**: 
* **Total Volume**: 14,197,122 images categorized into 21,841 distinct synsets.
* **ILSVRC Subset**: The most widely used version for model training, focusing on 1,000 selected categories and containing 1,431,167 images
* **Verification**: To remove irrelevant images from a pool of over 160 million candidates, labels were manually verified by over 50,000 workers via Amazon Mechanical Turk.

**Concepts**: 
The dataset is structured using **Synsets** (Sets of Synonyms) from WordNet:
* **Semantic Precision**: Instead of using ambiguous keywords, ImageNet groups synonymous terms into a single synset (e.g., "dog" and "Canis familiaris"). This resolves polysemy, distinguishing between different meanings of the same word, such as "bank" as a financial institution vs. a river bank.
* **Taxonomical Hierarchy**: Concepts are linked via **hypernymy** (is-a relationships). A "Siberian Husky" is nested under "Working Dog," which is under "Dog," and eventually "Mammal." This structure allows for evaluating models based on their hierarchical understanding of categories.

**Relevance**: 
In the context of concept detection and Explainable AI:
1. **Feature Extraction Baseline**: Most models used for Concept Activation Vectors (CAVs) are pre-trained on ImageNet. Their internal "understanding" of visual concepts is fundamentally shaped by the distribution of these 1,000 categories.
2. **Semantic Error Analysis**: The hierarchy enables the measurement of "semantic distance" - identifying if a model's failure is a fine-grained misclassification (e.g., two similar dog breeds) or a catastrophic semantic error (e.g., mistaking an animal for a vehicle).

**Known biases**:

1. **Geographic and Cultural Skew**: The dataset is heavily biased toward Western, high-income perspectives. Models trained on ImageNet often fail to recognize common objects when they appear in non-Western cultural contexts, such as traditional wedding attire or regional tools. ([link to paper](https://openaccess.thecvf.com/content_CVPRW_2019/html/cv4gc/de_Vries_Does_Object_Recognition_Work_for_Everyone_CVPRW_2019_paper.html))
2. **ImageNet Roulette and The Person Subtree**:
The "person" subtree inherited a stagnant WordNet ontology of 2,832 categories, including derogatory and non-visual labels . Due to the low imageability of abstract concepts, models were forced to perform "automated phrenology" by linking physical appearance to social or moral traits . A comprehensive audit removed 1,593 unsafe synsets (600,040 images) and 1,081 non-imageable categories (443,547 images). ([link to paper](https://dl.acm.org/doi/10.1145/3351095.3375709))

**Related Literature**:

* **AlexNet (2012)**: The pivotal architecture that utilized ImageNet to prove the superiority of Convolutional Neural Networks over traditional feature engineering ([link to paper](https://proceedings.neurips.cc/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html)).
* **ResNet (2016)**: Introduced Residual Learning, enabling the training of networks with over 100 layers on ImageNet ([link to paper](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html)).

## With one concept

### MetaShift

**Link**: [MetaShift: A Dataset of Datasets for Evaluating Contextual Distribution Shifts and Training Conflicts](https://arxiv.org/abs/2202.06523)

*Warning: this dataset is not really good in terms of quality*.

### CounterAnimals

#### Overview
**CounterAnimal** is a specialized evaluation dataset introduced in the paper  
[A Sober Look at the Robustness of CLIPs to Spurious Features](https://proceedings.neurips.cc/paper_files/paper/2024/hash/dd59fad18638714e6c447a3b7b9c4160-Abstract-Conference.html).

The dataset is designed to systematically evaluate the robustness of vision-language models (particularly CLIP) against **spurious correlations**, with a focus on background-dependent biases learned from large-scale web data.

#### Motivation
Traditional robustness benchmarks often exhibit **ImageNet Bias**, meaning they are tailored to older supervised models and fail to capture the weaknesses of modern models like CLIP. Although CLIP appears robust on these benchmarks, such evaluations do not expose the **shortcut learning** behavior induced by web-scale training.

CounterAnimal addresses this gap by providing a dataset explicitly constructed to test whether models rely on **contextual cues (e.g., background)** instead of **object-centric features**.

#### Spurious Correlations
A **spurious correlation** refers to a false association learned by a model between an object and its surrounding context.

- **Example:** A model trained on many images of cows on green grass may learn *"green background → cow"*.
- **Failure Case:** The model fails when presented with a cow on a beach due to the absence of the expected background.

CounterAnimal evaluates whether models truly recognize animals or rely on such contextual shortcuts (e.g., identifying an ice bear only when it appears on snow).

#### Dataset Composition

- **Total Images:** 13,100
- **Number of Classes:** 45 animal classes
- **Source:** Images collected from the iNaturalist platform
- **Class Origin:** Animal categories derived from the ImageNet-1K label set

##### Easy vs. Hard Split
The dataset is divided based on background familiarity:

- **Easy Set:** 7,174 images  
  - Contain animals in typical or commonly associated backgrounds  
  - Example: ice bear on snow  

- **Hard Set:** 5,926 images  
  - Contain animals in atypical or uncommon backgrounds  
  - Example: ice bear on grass  

This split enables direct measurement of performance degradation under background distribution shifts.

#### Dataset Construction Pipeline

The dataset was created using a combination of automated filtering and manual annotation through a four-step process:

##### 1. Data Collection
- Retrieved **300–800 raw images per class** from iNaturalist  
- Queries based on ImageNet-1K animal class names  

##### 2. Data Curation (Manual Cleaning)
Images were manually filtered using the following criteria:

- **Label Noise:** Removal of incorrectly labeled animals  
- **Feature Noise:** Removal of corrupted or broken images  
- **Obscurity:** Removal of images containing multiple animal classes  
- **Clarity:** Removal of images where the animal is heavily occluded or too small  

##### 3. Background Labeling
- Each image was manually annotated with one of **14 background categories**, including:
  - snow
  - grass
  - water
  - road
  - hand
  - sky
  - (and other environment types)

##### 4. Spurious Correlation Discovery
- A CLIP model was used to evaluate sensitivity to background changes:
  - For each class, if altering background conditions led to an **accuracy drop > 5%**, the class was selected
- Images were then categorized into:
  - **Easy:** Backgrounds aligned with learned correlations
  - **Hard:** Backgrounds that break learned correlations

#### Evaluation Protocol

- **Setting:** Zero-shot classification (zero-shot classification is when an AI identifies an object it has never seen before by comparing the image to a written description (like "a photo of a flamingo"))
- **Task:** Identify the correct animal class without task-specific training
- **Label Space:** Typically evaluated over the full **ImageNet-1K categories**
- **Goal:** Measure robustness to background-induced distribution shifts


#### Key Findings from Experiments

##### 1. Generality of Bias
- Significant performance drops (**17–30% or more**) observed across:
  - Different CLIP architectures (e.g., ViT-B/32, ViT-L/14)
  - Different pretraining datasets (e.g., LAION, OpenAI)
- Indicates that spurious correlations are inherent to web-scale training

##### 2. ImageNet Paradox
- Standard ImageNet-trained models outperform CLIP on background-shifted (Hard) samples
- Suggests CLIP relies more heavily on contextual information than traditional models

##### 3. Data Quality vs. Quantity
- Increasing dataset size (e.g., LAION-400M → LAION-2B) does **not** eliminate bias
- High-quality curated datasets (e.g., DataComp, DFN) significantly improve robustness
- Emphasizes importance of **data quality over scale**

##### 4. Model Size Effects
- Larger model architectures show improved robustness
- Suggests increased capacity helps disentangle object features from background context

#### Strategic Conclusions

1. **Distribution Shift Remains Unresolved**  
   Large vision-language models still rely on shortcut learning and have not achieved true semantic understanding.

2. **Need for New Benchmarks**  
   Existing benchmarks are insufficient for evaluating modern models. CounterAnimal provides a targeted tool for diagnosing background bias.

3. **Importance of Data Curation**  
   Robustness depends more on dataset quality and diversity than sheer scale.

### Waterbirds

*Warning: this dataset is not really good in terms of quality*.

**Link**: [Environment Inference for Invariant Learning](https://proceedings.mlr.press/v139/creager21a.html?%3Butm_source=hs_email&%3Butm_medium=email&%3B_hsenc=p2ANqtz-9GoNXKtgh3kIYhDbN6wuqn6vTgNYaUE_B6t5EpPdQ9phgpRXVhYpkLoFHDJ7S-TWBi8nwc&ref=the-batch-deeplearning-ai)

### NICO animals

*Warning: this dataset is not really good in terms of quality*.

**Link**: [Towards Non-I.I.D. image classification: A dataset and baselines](https://www.sciencedirect.com/science/article/pii/S0031320320301862)
