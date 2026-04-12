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

**Article link**: [The Caltech-UCSD Birds-200-2011 Dataset](https://authors.library.caltech.edu/27452/1/CUB_200_2011.pdf) (*direct link to the pdf download*); it is an extension of a previously published dataset, which can be found under this link: [Caltech-UCSD Birds 200](https://authors.library.caltech.edu/records/cyyh7-dkg06)

**Dataset link**: [raw files](https://data.caltech.edu/records/65de6-vp158) or [kaggle, with file content preview](https://www.kaggle.com/datasets/wenewone/cub2002011/data)

**Short Description**: The dataset contains images of birds labelled by species, annotated with bounding boxes, part locations (where in the picture are various bird body parts), and attribute labels (e.g. beak shape, color, etc.). The dataset seems quite popular, with over 5700 citations on its expanded version (which is the one linked above); its authors recommend it mostly for multi-class object detection. 

**Volume metrics**: 
- number of images: 11,788; 
- number of attributes: 28 per image (each with multiple options, in total 312 binary attributes);
- number of bird species: 200; the classes are reasonably well-balanced, with 40-60 images in each class. 

**Attributes**: There are 28 attributes, each with a fixed set of possible values: 
| Attribute | Values |
| :--- | :--- |
| **Bill shape**       | cone, all-purpose, dagger, hooked seabird, hooked, curved (up or down), spatulate, needle, specialized |
| **Wing Color**       | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Upperparts Color** | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Underparts Color** | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Breast Pattern**   | solid, striped, spotted, multi-colored |
| **Back color**       | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Tail shape**       | forked tail, rounded tail, notched tail, fan-shaped tail, pointed tail, squared tail |
| **Upper tail color** | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Head Pattern**     | malar, eyebrow, capped, eyering, unique pattern, striped, spotted, crested, masked, plain, eyeline |
| **Breast Color**     | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Throat Color**     | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Eye color**        | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green |
| **Bill length**      | shorter than head, about the same as head, longer than head |
| **Forehead Color**   | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Under tail color** | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Nape color**       | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Belly color**      | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Wing shape**       | pointed-wings, tapered-wings, long-wings, rounded-wings, broad-wings |
| **Size**             | small (5-9 in), very small (3-5 in), medium (9-16 in), very large (32-72 in), large (16-32 in) |
| **Shape**            | perching-like, tree-clinging-like, gull-like, duck-like, swallow-like, upright-perching water-like, sandpiper-like, upland-ground-like, chicken-like-marsh, pigeon-like, long-legged-like, hummingbird-like, hawk-like, owl-like |
| **Back pattern**     | solid, striped, spotted, multi-colored |
| **Tail pattern**     | solid, striped, spotted, multi-colored |
| **Belly Pattern**    | solid, striped, spotted, multi-colored |
| **Primary Color**    | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Leg color**        | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Bill color**       | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Crown color**      | blue, black, orange, buff, brown, grey, white, red, pink, rufous, yellow, olive, purple, green, iridescent |
| **Wing Pattern**     | solid, striped, spotted, multi-colored |

All of the above attributes are represented as a set of multiple binary attributes, of which there is 312 in total. Each of the attributes is connected with a specific body part that it applies to, according to a table below: 
| Body Part | Attributes |
| :--- | :--- |
| **Beak**             | bill shape, bill color, bill length |
| **Belly**            | belly pattern, belly color |
| **Throat**           | throat color |
| **Crown**            | crown color |
| **Tail**             | upper tail color, under tail color, tail pattern, tail shape |
| **Back**             | back color, back pattern |
| **Forehead**         | forehead color |
| **Nape**             | nape color |
| **Eye**              | eye color |
| **Wing**             | wing pattern, wing color, wing shape |
| **Breast**           | breast pattern, breast color |
| **Bird (all parts)** | size, shape |
| **Head**             | head pattern |
| **Leg**              | leg color |
| **Body**             | underparts color, upperparts color, primary color |

The complete list of body parts contains: back, beak, belly, breast, crown, forehead, left eye, left leg, left wing, nape, right eye, right leg, right wing, tail, throat. Each picture is annotated with the pixel location and visibility of all the body parts. 

**Example Images**: 
| Yellow Headed Blackbird | Bobolink | Red Faced Cormorant | Indigo Bunting |
| :--- | :--- | :--- | :--- |
| ![Yellow Headed Blackbird](image-2.png) | ![Bobolink](image-1.png) | ![Red Faced Cormorant](image-3.png) | ![Indigo Bunting](image-4.png) |

**Relevance**: Concepts are well-labelled, so the dataset should be pretty reliable for all concept detection tasks. They're also quite human-readable, since they're all based on objective visual traits such as color or shape. Moreover, it's easy to isolate images containing a particular concept (to later use for CAV computation). 

**Data Gathering & Labelling Methodology**: The data for different species was initially gathered by collecting all images from the Wikipedia site for this species; then, additional images were found through Flickr image search using the Wikipedia pictures as reference. Later, they were filtered and annotated by thousands of workers through Amazon Mechanical Turk website. Multiple workers' responses for the same image were compared against one another to create a single, coherent final label. As a result of this methodology, one should be cautious when using the dataset with pre-trained models, as many of them might have also been trained on the same Flickr images. 

**Known issues**: 
- Images returned by a Flickr query are often a result of one photographer taking many images of the same bird in a very short period of time, so the dataset often contains multiple near-identical images of a given bird. This causes issues with the train-test split: if some of these images are part of the training set, and some are in the test set, this results in a sort of data leakage (nearly identical pictures in training and test set). Thus, it is strongly recommended that we use the train-test split proposed by the authors, which intentionally avoids this issue. 
- There are some spurious correlations between the bird features and the type of background in the pictures - e.g. water birds often appear with a blue water background, while land birds are more often photographed against grass or leaves. Thus, classification models often learn the background instead of the actual bird; this phenomenon was studied by researchers who created the Waterbirds dataset, which artificially changes the background on some pictures to one that's unusual for its bird species ([Sagawa et al., "Distributionally Robust Neural Networks for Group Shifts: On the Importance of Regularization for Worst-Case Generalization"](https://arxiv.org/pdf/1911.08731)). 
- Similarly as above, there are also correlations between species and the pose/placement of the bird in the picture (e.g. birds that are easily scared are more often photographed from a distance), which also causes some biases in models ([Branson et al., "Bird Species Categorization Using Pose Normalized Deep Convolutional Nets"](https://arxiv.org/pdf/1406.2952)). 

**Related Literature**: 
- [Koh et al., "Concept Bottleneck Models"](https://arxiv.org/pdf/2007.04612): Uses CUB-200 dataset to train bottleneck models, ie. models which first train a model to predict some concepts (e.g. "wing color") and then predicting the label based only on these concepts, which allows for high explainability and possible model correction based on the mistakes at concept level. 
- [Jaderberg et al., "Spatial Transformer Networks"](https://arxiv.org/pdf/1506.02025): Introduces a Spacial Transformer module, which can be inserted into a neural network architecture and taught to transform the data in the network (so that it becomes invariant to translation, scale, rotation, and more generic warping); it uses CUB-200 to test its performance. 

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

This work introduces a vision transformer model that can explain its own predictions using interpretable concepts. It is related to FunnyBirds because the dataset allows testing whether the model correctly identifies which object parts are important for its decisions.


https://arxiv.org/abs/2202.03482 - 
Navigating Neural Space: Revisiting Concept Activation Vectors to Overcome Directional Divergence

This paper analyzes CAVs and shows that they can incorrectly capture unrelated features instead of true concepts. FunnyBirds is particularly useful in this context, as its controlled design allows verifying whether CAV methods correctly identify the importance of specific object parts.

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


### Different versions of ImageNet that are interesting for our course

#### tieredImageNet-H
- **Paper** [META-LEARNING FOR SEMI-SUPERVISED FEW-SHOT CLASSIFICATION](https://arxiv.org/abs/1803.00676)
- **Impact** 1992 citation as for 12.04.2026, Published as a conference paper at ICLR 2018
- **Dataset** [tiered-imagenet-tools](https://github.com/yaoyao-liu/tiered-imagenet-tools) [Not confirmed kaggle link](https://www.kaggle.com/datasets/arjun2000ashok/tieredimagenet)
- **Short Description** A reorganization of the ImageNet hierarchy (ILSVRC-12) designed to create a more rigorous and realistic meta-learning environment by ensuring the training, validation, and testing sets are semantically disjoint. It groups the 608 leaf classes into 34 high-level nodes (e.g., mammal, tool) and partitions these nodes near the root of the hierarchy tree, meaning the test-time classes are fundamentally different from those seen during training. The "H" variant extends this logic to a deep 12-level hierarchy specifically optimized for evaluating "Mistake Severity" (MS) and ensuring hierarchical consistency in model predictions.
- **Metrics** 608 classes, 779,165 images, and a 12-level deep hierarchy tree. Default image resolution is 84×84 pixels.
- **Concepts** 34 high-level semantic nodes (e.g., "mammal," "tool," "appliance") that partition the 608 leaf classes. Concepts are defined by their taxonomic path from the root to the leaf.
- **Relevance** Useful for studying "Mistake Severity" and hierarchical consistency; models can be evaluated on whether their predictions follow logical semantic paths across 12 levels of granularity.

- **Related Literature** 

- - [Meta-Transfer Learning for Few-Shot Learning](https://openaccess.thecvf.com/content_CVPR_2019/html/Sun_Meta-Transfer_Learning_for_Few-Shot_Learning_CVPR_2019_paper.html) This paper is directly related to dataset. 1749 citations as of 12.04.2026, Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2019

- - [Making Better Mistakes](https://openaccess.thecvf.com/content_CVPR_2020/html/Bertinetto_Making_Better_Mistakes_Leveraging_Class_Hierarchies_With_Deep_Networks_CVPR_2020_paper.html) (ECCV 2020): Introduces hierarchical losses to penalize errors that are semantically distant in the hierarchy.

#### ImageNet-Free (ImageNet-F) (also known as ImageNet-3L)
- **Paper** [Free-Grained Hierarchical Visual Recognition](https://arxiv.org/abs/2510.14737) 
- **Impact** 1 citation as of 12.04.2026, published on October 2025, current version on 08.04.2026, Accepted to CVPR 2026
- **Dataset** not published yet, here is the repo [ImageNet-3L](https://github.com/pseulki/FreeGrainLearning)
- **Short Description** A restructured version of ImageNet-1K that introduces a consistent three-level taxonomy (basic, subordinate, and fine-grained) to address the "noisy" nature of the original WordNet hierarchy. It focuses on the problem of "free-grained" recognition, where training labels can appear at any taxonomic depth depending on visual clarity (e.g., a distant object labeled only as "bird" vs. a close-up labeled as "bald eagle"). The dataset removes overly abstract nodes (like "Entity") to ensure every branch supports meaningful multi-level prediction and visual grounding across different scales of detail.
- **Metrics** 1,000 classes organized into 20 basic (coarse), 127 subordinate (mid), and 505 fine-grained classes.
- **Concepts** 3 levels of granularity defined by visual cues: global features (coarse), part-based features (mid), and local details (fine).
- **Relevance** Focuses on "free-grain" training where labels may appear at any taxonomy level; relevant for learning consistent concepts from incomplete or mixed-granularity labels.
- **Related Literature:** 

- - [H-CAST](https://openreview.net/forum?id=7HEMpBTb3R) (2024): Introduces a hierarchical consistency loss that aligns visual features across depths to improve classification and internal image segmentation.


#### ImageNet-21K-P
- **Paper** [ImageNet-21K Pretraining for the Masses](https://arxiv.org/abs/2104.10972)
- **Impact** 1092 citations as of 12.04.2026, Accepted to NeurIPS 2021 (Datasets and Benchmarks Track)
- **Dataset** [ImageNet-21K-P](https://github.com/Alibaba-MIIL/ImageNet21K)
- **Short Description** A cleaned and democratized version of the full original 21,841-class ImageNet collection. The pre-processing pipeline removes infrequent classes (fewer than 500 images), resulting in a more balanced set of 11,221 classes, and utilizes recursive WordNet parenthood to transform single labels into semantic multi-labels (e.g., "swan" inherits "bird," "vertebrate," and "animal"). To improve accessibility, images are "squish-resized" to 224×224 pixels, reducing the storage footprint from 1.3TB to a manageable 250GB, while significantly improving downstream performance on a wide variety of architectures.
- **Metrics** 14,197,122 images across 11,221 cleaned classes (down from 21,841 original classes); images are squish-resized to 224×224 pixels to reduce storage to 250GB.
- **Concepts** Recursive semantic lineage; images are associated with their original label and all its ancestors (e.g., "swan" inherits "animal").
- **Relevance** Allows training "Concept-based Interpretable Models" with ground truth at every level of abstraction; utilizes "Semantic Softmax" to exploit hierarchical structures.
- **Related Literature:** 

- - [An Image is Worth 16x16 Words](https://arxiv.org/abs/2010.11929) (ICLR 2021): Validates that large-scale architectures like ViT benefit significantly from the conceptual diversity of the 21K collection.

- - [Hierarchical Graphical Knowledge Representation](https://link.springer.com/chapter/10.1007/978-3-031-20044-1_7) (ECCV 2022): Proposes HGR-Net to scales zero-shot learning to tens of thousands of categories using class inheritance relations.


#### BREEDS Benchmarks
- **Paper** [BREEDS: Benchmarks for Subpopulation Shift](https://arxiv.org/abs/2008.04859)
- **Impact** 236 citations as of 12.04.2026, Published at the International Conference on Learning Representations (ICLR) 2021
- **Dataset** [BREEDS-Benchmarks](https://github.com/MadryLab/BREEDS-Benchmarks)
- **Short Description** A benchmark suite designed to evaluate model robustness to distribution shifts by repurposing ImageNet classes into "superclasses" and "subpopulations". By splitting the original classes into disjoint source and target sets, it simulates realistic shifts; for example, a model might be trained on specific breeds of dogs like "Poodles" and "Terriers" but must recognize "Dalmatians" during testing. This methodology enables the assessment of whether a model has truly grasped the invariant "core concept" (e.g., "dog-ness") or is merely memorizing shortcut features unique to its training subpopulations.
- **Metrics** Includes ENTITY-13 (13 superclasses, 20 subpopulations each), ENTITY-30 (30 superclasses, 8 subs each), LIVING-17 (17 superclasses, 4 subs each), and NON-LIVING-26 (26 superclasses, 4 subs each).
- **Concepts** Invariant semantic "superclasses" (e.g., "dog") that must be recognized across novel, unseen "subpopulations" (e.g., "Dalmatian" vs. "Poodle").
- **Relevance** Critical for distinguishing "concept understanding" from "label memorization"; probes whether models learn the core features of a concept regardless of specific subpopulation shifts.
- **Related Literature:**

- - [In Search of Lost Domain Generalization](https://arxiv.org/abs/2007.01434) (ICLR 2021): Evaluates domain adaptation methods on BREEDS, finding many fail to outperform simple baselines.


#### ImageNet-X
- **Paper** [ImageNet-X: Understanding Model Mistakes with Factor of Variation Annotations](https://arxiv.org/abs/2211.01866)
- **Impact** Over 62 citations as of 2026; used for "model debugging".
- **Dataset** [ImageNet-X](https://github.com/facebookresearch/imagenetx)
- **Short Description** A set of diagnostic human annotations for the entire ImageNet validation set and a subset of training data, pinpointing the specific "factors of variation" that lead to model failures. Each image is contrasted against "prototypical" high-confidence examples to identify attributes like object pose, background complexity, lighting, and various forms of occlusion (e.g., object blocked by a person). By isolating these environmental and geometric factors, researchers can determine whether a model's concept detection is truly invariant to realistic scene variations or dependent on spurious correlations.
- **Metrics** 50,000 images in the validation set plus a random 12,000-image training subset, each annotated with up to 16 factors.
- **Concepts** 16 human-interpretable factors: pose, background, lighting, size, color, texture, shape, occlusion (person/object), partial view, and co-occurrences.
- **Relevance** Directly aligns with concept detection tasks by identifying whether a model's "concept" of an object is invariant to geometric or environmental factors.


#### Tiny ImageNet
- **Dataset** [Tiny ImageNet, kaggle mirror](https://www.kaggle.com/c/tiny-imagenet), Subset of ILSVRC-2010, introduces as part of the CS231n: Convolutional Neural Networks for Visual Recognition course.
- **Short Description** A scaled-down, computationally efficient subset of ImageNet (ILSVRC-2010) featuring 200 object categories with lower-resolution (64×64 pixels) images. It provides a manageable testbed for exploring the relationship between visual texturing and classification accuracy, and it is widely used to prototype "Hierarchical Deep CNNs" (HD-CNNs) that separate "easy" coarse classes (like animals vs. vehicles) before employing expert fine-grained classifiers. The dataset is particularly useful for students to investigate how hyperparameters like receptive field size impact accuracy when visual information is limited.
- **Metrics** 200 classes, 120,000 labeled images downsampled to 64×64 pixels. Each class has 500 train, 50 val, and 50 test images.
- **Concepts** WordNet hierarchy synsets; often adapted to two-level "coarse-to-fine" conceptual structures.
- **Relevance** Sandbox for testing "Hierarchical Deep CNNs" (HD-CNNs) that separate easy coarse classes (e.g., vehicle vs. animal) before using expert classifiers for fine-grained distinction.
- **Related Literature** 

- - [Tiny ImageNet Challenge](https://cs231n.stanford.edu/reports/2017/pdfs/930.pdf) (2017) Benchmarks more modern architectures like Inception-ResNet and DenseNet on the $64 \times 64$ resolution images, showing that although filter distributions change significantly, these models still struggle with ambiguous subject placement in "Tiny" frames.

- - [Tiny ImageNet Visual Recognition Challenge](https://cs231n.stanford.edu/reports/2015/pdfs/yle_project.pdf) (2015) Investigates the impact of hyperparameters like receptive field size and dropout layers on classification accuracy, identifying that model performance is highly sensitive to texturing and specific geometric properties.

- - [Tiny ImageNet Classification with Convolutional Neural Networks](https://cs231n.stanford.edu/reports/2015/pdfs/leonyao_final.pdf) (2015) This report details a deep CNN architecture with 5 convolutional layers using $3 \times 3$ filters, demonstrating that an ensemble of three such networks can achieve a 56.7% test set error rate on the 200-class subset.



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
