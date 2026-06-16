# Models

## Task-specific models

### ResNet

Original paper: [**Deep Residual Learning for Image Recognition**](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html), Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun (2016)

#### Performance

ResNet won the **ILSVRC 2015** competition (ImageNet dataset), taking 1st place in image classification, detection, and localization, as well as 1st place at **COCO 2015** in detection and segmentation. The 152-layer model achieved **a top-5 error** (the correct class appears in the model's 5 best guesses) **of 3.57% on ImageNet** - surpassing human-level performance on this benchmark. Its greatest achievement was proving that networks of extreme depth (152 layers, 8 times deeper than VGG, yet with lower computational complexity) can be trained successfully without the degradation problem.

#### Novelty

Before ResNet, a common assumption was that more layers should provide better representations. In practice, there were two problems. **Vanishing/exploding gradients** - they were partially solved by careful initialization and batch normalization. Still, the **degradation problem** remained: beyond a certain depth, training accuracy dropped sharply - not due to overfitting, but because the error increased on the *training set*. Current optimizers simply failed to find good parameters for very deep networks in reasonable time.

ResNet's solution is the **residual (skip) connection**. Instead of asking a block of layers to learn a full mapping `x -> H(x)` from scratch, the block learns only the residual `x -> F(x) = H(x) − x`, and the original input `x` is added back at the end:

```
output = F(x) + x
```

The intuition (supported by examples from other fields, such as solving Partial Differential Equations) is that optimizing a residual is easier than learning a full transformation. In the extreme case where the identity transformation is the optimal option, the block just needs to drive its weights to zero - the shortcut moves the signal unchanged. Without this trick, nonlinear layers would need to approximate the linear function `f(x) = x` through activations, which is much harder to train.

Key implementation details:

- The shortcut adds the block's input directly to its output - no extra parameters (previous ideas included parameters, which could close the shortcut and block gradient flow in backpropagation), no added computational cost, fully compatible with backpropagation;
- When input and output dimensions differ, a 1×1 convolution can project the input to the required dimensions;
- Each residual block contains at least two layers for the effect to be meaningful (for one layer it would just result in a regular layer behavior);
- Works for both fully connected and convolutional layers;
- Skip connections are placed every few layers throughout the network;
- Deeper ResNets (50+ layers) use a bottleneck block: three consecutive convolutions (1×1, 3×3, 1×1), where the first reduces the channel count, the 3×3 processes the compressed representation, and the last restores it - keeping computational cost low at greater depths.

Experiments on both ImageNet and CIFAR-10 confirm that deep residual networks achieve lower training error than plain networks (without residual connections) of the same depth. At publication, ResNet-152 was the deepest successfully trained network ever.

#### Related Literature

- **He et al. (2016)** - [*Identity Mappings in Deep Residual Networks*](https://link.springer.com/chapter/10.1007/978-3-319-46493-0_38) - continuation of original paper, analysis of signal propagation in residual blocks. Authors propose a redesigned block layout (pre-activation ResNet v2) that simplifies optimization and improves generalization. Experiments include a 1001-layer network on CIFAR-10;
- **Geirhos et al. (2019)** - [*ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness*](https://openreview.net/forum?id=Bygh9j09KX&trk=public_post_comment-text) - demonstrates that ResNet classifies objects based on local texture rather than shape, e.g. a cat silhouette covered in elephant skin texture is confidently classified as an elephant;
- **Bello et al. (2021)** - [*Revisiting ResNets: Improved Training and Scaling Strategies*](https://proceedings.neurips.cc/paper_files/paper/2021/hash/bef4d169d8bddd17d68303877a3ea945-Abstract.html) - a systematic analysis of how training and scaling strategies (depth, width, image resolution, regularization) affect ResNet's accuracy on ImageNet. Authors demonstrate that a properly optimized ResNet (ResNet-RS) achieves accuracy comparable to EfficientNet while using less memory and training faster.

#### **Adoption of residual connections in other architectures:**

Residual connections from ResNet were adopted by many subsequent architectures - most notably the **Transformer** ([*Attention Is All You Need*](https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html), Vaswani et al., 2017), where skip connections wrap every attention block and feed-forward layer, enabling effective stacking of dozens of layers. Today they are present in virtually every deep learning architecture.

#### Trained Instances

- [**microsoft/resnet-50**](https://huggingface.co/microsoft/resnet-50) - 50-layer ResNet v1.5 architecture trained with the original training recipe (short training, basic data augmentation, SGD optimizer). Default configuration in HuggingFace Transformers library;
- [**microsoft/resnet-152**](https://huggingface.co/microsoft/resnet-152) - 152-layer ResNet v1.5 architecture, trained just like the resnet-50 (above);
- [**timm/resnet50.a1_in1k**](https://huggingface.co/timm/resnet50.a1_in1k) - the same 50-layer ResNet v1.5 architecture, but with different training based on [*ResNet Strikes Back*](https://openreview.net/forum?id=NG6MJnVl6M5), Wightman et al., 2021 (more epochs, more advanced data augmentation and other improvements). Has higher accuracy on ImageNet than basic version.

### Vision Transformer (ViT)

#### Original paper

[**An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale**](https://arxiv.org/pdf/2010.11929), Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, Neil Houlsby (2020)

#### Performance

When pre-trained on large amounts of data (14M-300M images), ViT achieves an accuracy similar or higher than CNNs such as ResNet; in particular, the best model reaches the accuracy of 88.55% on ImageNet, 90.72% on ImageNet-ReaL, 94.55% on CIFAR-100, and 77.63% on the VTAB suite of 19 tasks. When trained just on the medium-sized datasets (e.g. ImageNet), it achieves accuracies that are just a few percentage points lower than ResNets on comparable size. Additionally, the pre-training of ViTs on large datasets requires significantly less computational resources - up to **4 times less computational resources** than with CNNs (e.g. BiT, which is a ResNet-based model).

#### Novelty

ViT proposes an entirely new architecture for computer vision models. Before ViT, the standard for vision models were CNNs, which have image-specific **inductive biases** (built-in architectural assumptions about how images generally work, which help the model learn faster):
1. **Locality:** Pixels close to each other are more related.
2. **Translation invariance:** An object is considered the same regardless of where in the image it is placed.

ViT doesn't have any such image-specific inductive biases; instead, it uses a pure **Transformer** architecture that was originally designed for NLP tasks:
- The image is divided into a grid of fixed-size patches (e.g., 16x16 pixels).
- These patches are flattened and linearly projected into 1D embeddings. 
- Additionally, positional embeddings are added so that the model has information about which patches correspond to which parts of the original image.
- This understanding of the original images is later used in a Transformer architecture, using mechanisms of self-attention that allow it to infer connections between separate tokens (patches). 

This makes the models consider images in a more global sense, instead of focusing only on the connections and patterns found in neighbouring pixels - it means ViT can easily link parts of an image that are far away from each other. 

#### Related Literature (Known Issues)

- **Touvron et al. (2021)** - [*Training data-efficient image transformers & distillation through attention*](https://arxiv.org/pdf/2012.12877) - ViTs require a massive amount of training data due to the afore-mentioned lack of inductive biases that CNNs have, meaning that they need to be pre-trained on huge datasets in order to learn how to process images. The authors of this paper introduced DeiT (Data-efficient Image Transformers) which are based on ViT architecture, implement some modifications (e.g. data augmentation or using CNNs as 'teachers' in order to make the ViT training process faster) and can effectively be trained on ImageNet-1K from scratch. 
- **Paul and Chen (2022)** - [*Vision Transformers are Robust Learners*](https://arxiv.org/pdf/2105.07581) - shows how ViTs are generally more robust than CNNs (ie. perform better than CNNs on corrupted images or images with unusual backgrounds, which CNNs are likely to misclassify). 
- **Naseer et al. (2021)** - [*Intriguing Properties of Vision Transformers*](https://arxiv.org/pdf/2105.10497) - Shows that when presented with texture and shape of the same object, while CNNs often make decisions based on texture, ViTs perform better than CNNs and comparable to humans on shape recognition, which means they are better for e.g. recognizing object shapes in less textured data such as paintings. Hence, their output also changes less after patch-level spatial permutations or common natural corruptions (e.g., noise, blur, contrast and pixelation artefacts). However, they are still vulnerable to adversarial attacks and some image corruptions. 

#### Trained Instances

- [**google/vit-base-patch16-224**](https://huggingface.co/google/vit-base-patch16-224) - The base ViT architecture (86M parameters) using 16x16 patch resolution, pre-trained on ImageNet-21k (14 million images) and fine-tuned on ImageNet-1K at 224x224 resolution.
- [**google/vit-large-patch16-224**](https://huggingface.co/google/vit-large-patch16-224) - The large ViT variant (304M parameters) using 16x16 patches, pre-trained on ImageNet-21k and fine-tuned on ImageNet-1K.
- [**facebook/deit-base-patch16-224**](https://huggingface.co/facebook/deit-base-patch16-224) - a base-sized version of DeIT (Data-efficient Image Transformer), which is a more efficiently trained Vision Transformer (ViT); pre-trained on ImageNet-1k. 

### ConvNext

### EfficientNet

## Self-supervised foundational models

### CLIP

[Link]

* **Publikacja:** "Learning Transferable Visual Models From Natural Language Supervision" (A. Radford, J. W. Kim, C. Hallacy i inni, OpenAI).
* **Link:** [https://proceedings.mlr.press/v139/radford21a](https://proceedings.mlr.press/v139/radford21a)

[Performance]

* **Przewaga nad modelami nadzorowanymi:** Wygrywa z w pełni nadzorowanym ResNet z ostatnią warstwą liniową do klasyfikacji na 16 z 27 badanych zbiorów danych w ustawieniu *zero-shot* (bez wcześniejszego treningu na tych danych).
* **Rozpoznawanie akcji:** Znacznie lepiej radzi sobie na zbiorach wideo – dowodzi to, że język naturalny lepiej uczy czasowników niż rzeczowników.
* **Rekord na STL10:** Na tym zbiorze (niska jakość, 10 klas) osiąga 99,3% skuteczności w *zero-shot*, co jest najlepszym dotychczasowym wynikiem.
* **Dobry wynik na zbiorze ImageNet:** Dorównuje nadzorowanemu modelowi ResNet-50 bez wykorzystania żadnego z ponad miliona przykładów treningowych z tego zbioru.
* **Porównanie z *few-shot*:** Osiąga wyniki na poziomie modeli trenowanych na 4 przykładach na klasę, podczas gdy on sam nie widział wcześniej żadnego przykładu z tej klasy - będąc trenowanym na otwartym zbiorze zdjeć z internetu. Dorównuje również skutecznością ówczesnemu liderowi (BiT-M ResNet-152x2), który był uczony na 16 przykładach dla klasy.
* **Efektywność przestrzeni cech:** Po wygenerowaniu embeddingów zdjęć za pomocą pewnych algorytmów (np. ResNet) CLIP potrafi je częściej lepiej przypisać niż klasyfikatory wytrenowane stricte na konkretnym zbiorze danych. Największa różnica jaka była zaobserwowana to potrzeba aż 184 zdjęć na klasę, żeby zwykły klasyfikator radził sobie tak jak CLIP - na zbiorze FER2013.
* **Skalowalność i moc obliczeniowa:** Skuteczność mocno rośnie wraz ze wzrostem mocy obliczeniowej (podobnie jak w modelach GPT). Model jest 3 razy bardziej efektywny obliczeniowo niż klasyczne sieci konwolucyjne.
* **Odporność na przesunięcie rozkładu (*data distribution shift*):** CLIP jest wysoce odporny na np. zmiany perspektywy. Podczas gdy klasyczne modele zapamiętywały wzorce specyficzne dla ImageNet, w CLIP różnica dokładności między znanymi a nowymi danymi spadła o 75%.
* **Wada *fine-tuningu*:** Tradycyjne douczanie na docelowym zbiorze zwiększa w nim dokładność, ale kosztem drastycznego spadku skuteczności na innych danych.

[Novelty]

* **Zmiana paradygmatu:** Modele nie uczą się ze sztywnie przypisanych etykiet, ale z surowego tekstu z internetu, wykorzystując nowoczesne metody reprezentacji języka.
* **Uczenie kontrastowe:** Model nie działa jak klasyfikator. Mapuje dane do wspólnej przestrzeni, gdzie maksymalizuje podobieństwo odpowiadających sobie próbek i minimalizuje je względem pozostałych. Uczy się dopasowywać pary tekst-obraz zamiast klasycznej ekstrakcji cech pod klasyfikator.
* **Zbiór danych WIT (WebImageText):** Autorzy odrzucił publiczne zbiory (jak YFCC100M), ponieważ po odfiltrowaniu szumu (np. nazw plików "2307634.jpg") zostawało zbyt mało danych. Nowy zbiór WIT zawiera 400 milionów par obraz-tekst (skala zbliżona do danych treningowych GPT-2). Wcześniejsze rozwiązania (VirTex, ConVIRT) operowały jedynie na setkach tysięcy przykładów.
* **Proces treningowy:** W każdej iteracji model przetwarza macierz wszystkich kombinacji $N \times N$ (obrazy i słowa). Maksymalizuje podobieństwo poprawnych par na przekątnej i minimalizuje wszystkie pozostałe kombinacje.
* **Uproszczenie modelu:** Dzięki ogromnej skali danych wyeliminowano problem *overfittingu*. Pozwoliło to na redukcję augmentacji obrazów do minimum, uproszczenie architektury oraz łatwiejszy dobór hiperparametrów.

[Related Literature (Known Issues)]

* **Znaczenie danych:** *Demystifying CLIP Data* ([arxiv.org/abs/2309.16671](https://arxiv.org/abs/2309.16671)). Udowadnia, że za sukces CLIP-a odpowiada głównie jakość i skala danych, a nie sama architektura. Autorzy z sukcesem odtworzyli model, korzystając z otwartego zbioru CommonCrawl.
* **Uprzedzenia i bias:** *Evaluating CLIP: Towards Characterization of Broader Capabilities and Downstream Implications* ([arxiv.org/abs/2108.02818](https://arxiv.org/abs/2108.02818)). Model przejął błędy poznawcze z danych internetowych. Na zbiorze *FairFace* (po dodaniu etykiet "animal"/"gorilla") błędnie sklasyfikowano ok. 5% danych, w tym 14% zdjęć osób czarnoskórych. 16% mężczyzn powiązano z kategoriami przestępczymi (w porównaniu do 10% kobiet).
* **Ataki typograficzne:** *Multimodal Neurons in Artificial Neural Networks* ([distill.pub/2021/multimodal-neurons/](https://distill.pub/2021/multimodal-neurons/)). Model priorytetyzuje tekst nad cechy wizualne. 
 
Przykładowo: jabłko z przyklejoną etykietą "iPod" jest klasyfikowane jako iPod z niemal 100% pewnością.
* **Problem z kompozycją i relacjami:** *Winoground: Probing Vision and Language Models for Visio-Linguistic Compositionality* ([openaccess.thecvf.com/...](https://openaccess.thecvf.com/content/CVPR2022/papers/Thrush_Winoground_Probing_Vision_and_Language_Models_for_Visio-Linguistic_Compositionality_CVPR_2022_paper.pdf)). Testy na zbiorze *Winoground* wykazują, że CLIP działa jak *bag-of-words* (worek słów) – ignoruje kolejność wyrazów. Nie odróżnia np. opisu "(a) some plants surrounding a lightbulb" od "(b) a lightbulb surrounding some plants".
* **Ślepota wizualna:** *Eyes Wide Shut? Exploring the Visual Shortcomings of Multimodal LLMs* ([openaccess.thecvf.com/...](https://openaccess.thecvf.com/content/CVPR2024/html/Tong_Eyes_Wide_Shut_Exploring_the_Visual_Shortcomings_of_Multimodal_LLMs_CVPR_2024_paper.html)). Odkryto pary obrazów (zbiór *Multimodal Visual Patterns*), które model uznaje za niemal identyczne, mimo że dla człowieka różnią się w sposób oczywisty.

[Trained Instances]
Dostępne są konwolucyjne i atencyjne instancje.

**Warianty atencyjne (Vision Transformer - ViT):**
* `openai/clip-vit-base-patch32`
* `openai/clip-vit-base-patch16`
* `openai/clip-vit-large-patch14` – najbardziej dokładny model z rodziny, wykorzystujący najmniejszy rozmiar "łatki" do detalu.



**Warianty konwolucyjne (architektura ResNet):**
* `timm/resnet50_clip.openai` 
* `timm/resnet101_clip.openai` – głębsza sieć (101 warstw), lepiej radząca sobie ze złożonymi wzorcami.

### DINO family

### SigLIP

### BLIP

## Fine-tuning techniques

### Freezing

### LoRA

### Linear probing
