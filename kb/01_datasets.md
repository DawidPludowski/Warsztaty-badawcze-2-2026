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

**Link**: [FunnyBirds: A Synthetic Vision Dataset for a Part-Based Analysis of Explainable AI Methods](https://openaccess.thecvf.com/content/ICCV2023/html/Hesse_FunnyBirds_A_Synthetic_Vision_Dataset_for_a_Part-Based_Analysis_of_ICCV_2023_paper.html)

FunnyBirds is a **synthetic computer vision dataset** designed specifically for the evaluation of explainable AI methods. The dataset contains artificially generated bird species composed of several **semantic parts - concepts** such as beaks, wings, eyes, feet, and tails. Each bird class is a unique combination of these parts.
Unlike most real-world datasets, FunnyBirds allows **controlled semantic interventions**, such as removing individual parts of an object, which enables the estimation of the **true importance of each concept** for model predictions. This makes the dataset particularly suitable for studying **concept-based explanations and the faithfulness of XAI methods**.


#### Volume metrics

- **Number of images**: 50 500  
- **Training images**: 50 000  
- **Test images**: 500  
- **Number of classes**: 50 synthetic bird species  
- **Number of concepts**: 5 parts per image  


#### Concepts available

Each image contains the following **five parts**: beak, wings, eyes, feet and tail. 
Each concept has multiple variants:

- **4 beak types**
- **3 eye types**
- **4 feet types**
- **9 tail types**
- **6 wing types**

These variants are **created manually using fixed geometric shapes and colors**. This makes each element clear and easy to distinguish, with a well-defined meaning.

Bird classes are created by **combining these predefined concept variants**, resulting in a large space of possible bird configurations. From the total **2 592 possible combinations**, **50 classes are randomly sampled** to build the final dataset.

#### Why is it relevant?

FunnyBirds is useful for concept-based explainability research because:

- the concepts represent parts of an object that people can easily understand
- you can change or remove parts (for example, remove a beak)
- it is possible to know how important each concept really is
- the data is fully controlled during creation
- it avoids many problems that occur in real-world datasets

**Hypotheses to be evaluated:**

- Do XAI methods correctly identify which object parts are truly important?
- Can explanations correctly rank how important each part is (e.g., beak vs wings)?
- Do different neural network types produce different quality explanations?

#### Known limitations

1. The dataset is synthetic, so it does not include all the complexity of real-world images.
2. The objects are simple and have only five concepts, which limits variety.
3. Models trained on FunnyBirds may not work as well on real-world data.

However, these limitations are intentional, since the goal is to create a **controlled environment for analyzing explainability methods**.

#### Related papers
https://arxiv.org/abs/2406.10025 - 
ProtoS-ViT: Visual foundation models for sparse self-explainable classifications


https://arxiv.org/abs/2202.03482 - 
Navigating Neural Space: Revisiting Concept Activation Vectors to Overcome Directional Divergence

### ISIC

**Link**: [https://ieeexplore.ieee.org/abstract/document/8363547](https://ieeexplore.ieee.org/abstract/document/8363547) (*Note: ISIC is released in multiple versions, almost 1 version per year, so newer versions should be mentioned/compared*)

### ImageNet + WordNet

*Note: When used like in [this paper](https://arxiv.org/pdf/2602.11448)*

## With one concept

### MetaShift

**Link**: [MetaShift: A Dataset of Datasets for Evaluating Contextual Distribution Shifts and Training Conflicts](https://arxiv.org/abs/2202.06523)

*Warning: this dataset is not really good in terms of quality*.

### CounterAnimals

**Link**: [A Sober Look at the Robustness of CLIPs to Spurious Features](https://proceedings.neurips.cc/paper_files/paper/2024/hash/dd59fad18638714e6c447a3b7b9c4160-Abstract-Conference.html)

### Waterbirds

*Warning: this dataset is not really good in terms of quality*.

**Link**: [Environment Inference for Invariant Learning](https://proceedings.mlr.press/v139/creager21a.html?%3Butm_source=hs_email&%3Butm_medium=email&%3B_hsenc=p2ANqtz-9GoNXKtgh3kIYhDbN6wuqn6vTgNYaUE_B6t5EpPdQ9phgpRXVhYpkLoFHDJ7S-TWBi8nwc&ref=the-batch-deeplearning-ai)

### NICO animals

*Warning: this dataset is not really good in terms of quality*.

**Link**: [Towards Non-I.I.D. image classification: A dataset and baselines](https://www.sciencedirect.com/science/article/pii/S0031320320301862)
