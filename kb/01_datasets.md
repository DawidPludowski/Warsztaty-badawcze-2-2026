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
**Link**: [Skin lesion analysis toward melanoma detection: A challenge at the 2017 International symposium on biomedical imaging (ISBI), hosted by the international skin imaging collaboration (ISIC)](https://ieeexplore.ieee.org/abstract/document/8363547)
 
**Short Description**:
The International Skin Imaging Collaboration (ISIC) is a global partnership that has organized the world's largest public repository of dermoscopic images of skin lesions annotated with expanded metadata. Their [archive](https://www.isic-archive.com/) has been used for consecutive years to host challenges on skin lesion analysis toward melanoma detection. As of today the repository contains 549,571 public images and 1,203,225 in total. 

**Overall volume metrics **: Number of images: 549,571; up to 8 diagnostic groups (depends on edition); up to 80 features for each image (for 2024 edition)

**ISIC Challenges History**

Below is a comprehensive comparison of the major ISIC Challenges over the years. This table provides historical context on how the focus of skin cancer AI research has evolved.

| Year | Primary Dataset Name / Source | Train Images | Diagnostic Scope (Classes) | Key Focus & Historical Innovation |
|:---:|---|---|---|---|
| **2016** | ISIC Archive Subset | 900 | 2 Classes <br>*(Melanoma vs. All)* | **Proof of Concept:** The inaugural challenge. Focused heavily on basic lesion segmentation and establishing the first standardized baseline for deep learning in dermatology. |
| **2017** | ISIC Archive Subset | 2,000 | 3 Classes <br>*(MEL, NV, SK)* | **Interpretability:** Introduced Dermoscopic Feature Extraction (e.g., detecting *streaks* or *pigment networks*). Pushed for models that could "explain" their reasoning using clinical taxonomy. |
| **2018** | [HAM10000](https://pmc.ncbi.nlm.nih.gov/articles/PMC6091241/)  | 10,015 | 7 Classes <br>*(Realistic multi-class)* | **Clinical Realism & Imbalance:** The massive leap in scale. Forced models to handle extreme real-world class imbalance. |
| **2019** | HAM10000 + [BCN20000](https://api.isic-archive.com/collections/249/) | 25,331 | 7 Classes + OOD  | **Out-of-Distribution (OOD):** Added an "unknown" class to test if models could say "I don't know" when faced with novel or unrepresented lesions. Focused heavily on mitigating artifact bias. |
| **2020** | [ISIC 2020](https://doi.org/10.34970/2020-ds01) | 33,126 | 2 Classes <br>*(Melanoma vs. Benign)* | **Patient-Level Context:** Images were grouped by patient ID. Challenged models to replicate the clinical **"Ugly Duckling" sign** (detecting a melanoma by comparing it to the patient's other benign moles). |
| **2024** | [ISIC 2024]( https://doi.org/10.34970/2024-slice-3d) | 401,059 <br> | Binary <br>*(Malignant vs. Benign)* | **3D Total Body Photography (3D-TBP):** A paradigm shift from close-up dermoscopy to processing hundreds of thousands of small crops from wide-field 3D cameras, heavily integrating tabular patient metadata. |

**Classes available in the cited edition**:

| Abbreviation | Full name | Malignant? | Notes |
|---|---|---|---|
| MEL | Melanoma | Yes | Deadliest form of skin cancer; main target for detection |
| NV | Melanocytic Nevus | No | Benign mole; serves as the primary healthy baseline |
| SK | Seborrheic Keratosis | No | Benign non-melanocytic growth; key distractor visually similar to melanoma |

Description of classes for other datasets in archive can be found here ([link to paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC6091241/))

**Challenge Structure (ISIC 2017)**:

The 2017 challenge was structured into three distinct tasks, designed to mimic a dermatologist's step-by-step diagnostic process:

1. **Task 1: Lesion Segmentation**: Generating a binary mask to accurately separate the skin lesion from the surrounding healthy skin and background.
2. **Task 2: Dermoscopic Feature Extraction**: Identifying and localizing specific clinical dermoscopic patterns within the lesion (e.g., *atypical pigment network*, *streaks*, *milia-like cysts*). This was a crucial step towards building interpretable AI models.
3. **Task 3: Disease Classification**: The final diagnostic step. Models had to classify the crop into one of three core categories mentioned above. 

**Known biases**:
1. models tend to predict malignancy based on the presence of surgical skin markings, rulers, or band-aids ([link to paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC6694463/))
2. models tend to perform significantly worse on darker skin tones due to extreme dataset imbalance (the vast majority of images are from fair-skinned populations) ([link to paper](https://arxiv.org/abs/2308.09640))
3. models tend to rely on image artifacts (like dark corners/vignetting) or background skin rather than the lesion itself, maintaining high accuracy even if the actual lesion is completely masked ([link to paper](https://openaccess.thecvf.com/content_CVPRW_2019/papers/ISIC/Bissoto_DeConstructing_Bias_on_Skin_Lesion_Datasets_CVPRW_2019_paper.pdf))



**Related papers**:
 **Related papers (ISIC 2017)**:

**Related papers**:

1. [Skin Lesion Analysis Toward Melanoma Detection: A Challenge at the 2017 ISBI](https://arxiv.org/abs/1710.05006) - The official ISIC 2017 summary detailing the dataset setup, the three clinical tasks, and an analysis of top-performing algorithms.
2. [RECOD Titans at ISIC Challenge 2017](https://arxiv.org/abs/1703.04819) - A top-performing team's breakdown on using transfer learning and specific data augmentation to overcome the small 2,000-image dataset constraint.
3. [Skin Lesion Classification Using Deep Multi-scale Convolutional Neural Networks](https://arxiv.org/abs/1703.01402) - A highly cited approach using a multi-scale Inception-v3 network to successfully mimic how a dermatologist zooms in and out to analyze lesions.
### ImageNet + WordNet

*Note: When used like in [this paper](https://arxiv.org/pdf/2602.11448)*

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
