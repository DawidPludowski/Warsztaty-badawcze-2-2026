# Models

## Task-specific models

### ResNet

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
