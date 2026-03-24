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
